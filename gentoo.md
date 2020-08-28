# gentoo知识

## gentoo安装

### 下载镜像，配置硬件

使用VM来安装虚拟机，下载最小iso文件，配置系统所需硬件，启动虚拟机。

### 开启后相关配置

开启后选择内核  启用sshd，设置root密码。设置后可以通过ssh连接来操作，因为虚拟机安装gentoo的输入不是很人性化。

    gentoo dosshd passwd=密码

passwd命令可以修改root密码

创建用户 `useradd -m -G users xuerb`  使用 `passwd xuerb` 来设置密码。

确定端口名称 `ifconfig` 或者 `ip addr`

手动加载网络模块  ```ls /lib/modules/`uname -r`/kernel/drivers/net```查找网络提供了哪些内核模块

modprobe 模块名称  来加载模块

### 磁盘分区

两种分区技术 DOS（MBR）和GPT

#### DOS分区

DOS分区使用32位标识符，主分区信息存储在磁盘的开始区域，大小是512b，只支持四个主分区

#### GPT分区

GPT分区使用64位标识符，存储区域很大，当系统和固件之间的系统软件接口是UEFI（而不是BIOS）时，GPT几乎是强制性的，因为DOS disklabel会出现兼容性问题。GPT还携带CPR32校验用来检测头部和分区表中的错误，并且在尾部还有备份GPT

ebuild存储库默认位于/var/db/repos/gentoo

使用BIOS引导分区，GPT分区方案

设置分区，使用parted方式进行分区 `parted -a optimal /dev/sda`进行分区

设置GPT标签

    mklabel gpt （MBR布局则是 mklabel msdos）
使用 兆字节来初始化

    unit mib
通知parted从1 MB开始，以3 MB结尾创建一个分区

    mkpart primary 1 3
命名1分区为grub

    name 1 grub 
给1分区添加一个标签

    set 1 bios_grub on
创建交换分区

    mkswap /dev/sda3  
激活交换分区

    swapon /dev/sda3

#### 创建文件系统

设置完分区，需要在分区上安装文件系统

    mkfs.ext4 /dev/sda4  mkfs.ext2 /dev/sda2  

挂载根分区

    mount /dev/sda4 /mnt/gentoo

#### 下载stage3包

设置时间

    ntpd -q -g //自动设置
下载stage3包，进入

    wget stage3包链接地址
解压

    tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner

#### 配置编译选项

为了优化Gentoo，可以设置几个变量来影响Gentoo官方支持的软件包管理器Portage的行为。

Portage会去读取`/etc/portage/make.conf`文件，在`/mnt/gentoo/usr/share/portage/config/make.conf.example`中可以找到所有可能变量的注释列表。

通过编辑`/mnt/gentoo/etc/portage/make.conf` 来配置环境变量

    nano -w /mnt/gentoo/etc/portage/make.conf
可在此文件中设置Gentoo镜像,写入

    GENTOO_MIRRORS="http://mirrors.163.com/gentoo/"

配置Gentoo ebuild资料库 通过`/etc/portage/repos.conf/gentoo.conf`文件配置Gentoo ebuild存储库，文件包含更新软件包存储库所需的同步信息（ebuild和相关文件的集合，其中包含Portage下载和安装软件包所需的所有信息）。

创建 repos.conf目录

    mkdir --parents /mnt/gentoo/etc/portage/repo.conf

将Portage提供的Gentoo存储库配置文件复制到（新创建的）repos.conf目录中

    cp /mnt/gentoo/usr/share/portage/config/repos.conf  /mnt/gentoo/etc/portage/repo.conf/gentoo.conf 
修改sync-uri为国内的源

    rsync://mirrors.ustc.edu.cn/gentoo-portage/
需要复制DNS信息，确保进入新环境后网络仍然可以正常工作

    cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
挂载必要的文件系统，确保在根目录更改后，新环境仍可以正常运行。

    mount --types proc /proc /mnt/gentoo/proc
    mount --rbind /sys /mnt/gentoo/sys
    mount --make-rslave /mnt/gentoo/sys
    mount --rbind /dev /mnt/gentoo/dev
    mount --make-rslave /mnt/gentoo/dev
使用非Gentoo安装媒体时，这可能还不够。某些发行版使`/dev/shm`成为`/run/shm` /的符号链接，在chroot之后，该链接无效。使`/dev/shm/`正确地安装在前面的tmpfs可以解决此问题：

    test -L /dev/shm && rm /dev/shm && mkdir /dev/shm
    mount --types tmpfs --options nosuid,nodev,noexec shm /dev/shm
设置模式为1777  `chmod 1777 /dev/shm` （1是t权限，需要上级目录的写权限，才能对此文件进行删除）

#### chroot进入新的安装环境

在这些步骤完成后，我们就可以通过chroot进入新的安装环境，将根目录从当前安装环境，更改为安装系统。
chroot的三个步骤
1.使用chroot 将根目录位置从/（在安装介质上）更改为/mnt/gentoo/（在分区上）chroot /mnt/gentoo /bin/bash
2.使用source命令将某些设置（/etc/profile中的那些设置）重新加载到内存中 source /etc/profile
3.更改提示，帮助我们记住现在处于chroot环境中 export PS1="(chroot) ${PS1}"
挂载启动分区  mount /dev/sda2 /boot
配置portage
从Gentoo镜像中获取最新的快照并安装 emerge-webrsync
更新ebuild存储库时，使用 emerge --sync ，它使用rsync协议将Gentoo ebuild信息库（该信息先前是通过emerge-webrsync获取的）更新为最新状态。
阅读新闻 新闻项是一种通过Gentoo ebuild存储库向用户推送重要消息的通信媒介。通过eselect工具来管理
三种操作：list显示新闻概述，read阅读新闻项目，purge阅读完新闻后删除。
选择配置文件：通过 eselest profile list 查看系统当前的配置文件，使用 eselect profile set num 来选择配置文件，选择no-multilib配置文件，他是没有32位4应用和库的纯64位环境。
更新 World set 它包括System set(包含标准Gentoo Linux安装正常运行所需的软件包)和Selected set(包含管理员明确安装的软件包)
使用 emerge --ask --verbose --update --deep --newuse @world
配置USE变量 SE是Gentoo向用户提供的最强大的变量之一。可以对某些程序进行编译，无论是否支持这些项目。在USE变量中，用户定义映射到compile-options上的关键字。默认的USE设置位于系统使用的Gentoo配置文件的make.defaults文件中。
检查默认的USE变量的方法是 emerge --info | grep ^USE
在/var/db/repos/gentoo/profiles/use.desc中可以找到有关可用USE标志的完整说明。
在/etc/portage/make.conf中可以添加和删除默认的USE变量。
配置ACCEPT_LICENSE变量 Portage使用ACCEPT_LICENSE变量来确定允许哪些软件包，而不会提示用户先前接受的许可证。可以在/etc/portage/package.license中对每个程序包进行例外处理 。可以通过更改/etc/portage/make.conf在整个系统范围内进行自定义。
在/etc/portage/make.conf 写入 ACCEPT_LICENSE="-* @FREE"，也可以根据需要添加每个软件包的覆盖。
选择时区 在/usr/share/zoneinfo/中查找可用的时区，然后写进/etc/timezone文件。 选择 Asia/Shanghai作为时区 echo "Asia/Shanghai" > /etc/timezone 然后，重新配置包，将基于/etc/timezone条目更新/etc/localtime文件。/etc/localtime文件用于让系统的C类库知道系统在什么时区。  emerge --config sys-libs/timezone-data
配置语言环境 系统应该支持的地区应该在/etc/locale.gen中提到。 在其中添加 zh_CN.UTF-8 UTF-8， zh_CN GBK， en_US ISO-8859-1，en_US.UTF-8 UTF-8
然后运行 locale-gen。它将生成/etc/locale.gen文件中指定的语言环境。locale -a 验证当前可用的语言环境，使用eselect来设置。eselect locale list可显示可用的目标，eselect locale set VALUE选择，选择zh_CN.utf8
手动设置的话，在/etc/env.d/02locale文件写入  LANG="zh_CN.UTF-8" 
然后重新加载环境 env-update && source /etc/profile && export PS1="(chroot) ${PS1}"

安装linux内核 
在/usr/src/中安装Linux内核源码，并有一个符号连接叫作linux将指向安装的内核源码
通过 emerge --ask sys-kernel/gentoo-sources 来安装内核
然后需要编译和配置内核源代码，使用genkernel工具来完成。
首先 emerge --ask sys-kernel/genkernel 来安装工具，接下来编辑/etc/fstab文件来使包含有第二个值为/boot/的那条的第一个值指向到正确的设备。 写入 /dev/sda2/boot ext2 defaults 0 2 最后通过 genkernel all 来完成安装。
genkernel安装完成之后，将创建一个内核、全部的模块和初始化内存文件（initramfs）。
配置模块   查看所有可用的模块 find /lib/modules/<kernel version>/ -type f -iname '*.o' -or -iname '*.ko' | less  在 /etc/modules-load.d/*.conf 是需要自动加载的模块，/etc/modprobe.d/*.conf 是模块的附加选项。
安装固件可以安装sys-kernel/linux-firmware包，大多数固件都打包在里。emerge --ask sys-kernel/linux-firmware
文件系统信息 /etc/fstab文件里有系统的分区信息，每行六个字段来定义分区信息。设备信息，分区挂载点，文件系统，挂载选项，是否dump，检查顺序。

网络信息
主机名，编辑/etc/conf.d/hostname  写入hostname="tux" 主机名为xue
域名 编辑/etc/conf.d/net  不配置域名需要在/etc/issue 中将 .\0 去掉  
配置网络 首先安装net-misc/netifrc  在 /etc/conf.d/net  写入ip定义 
config_eth0="192.168.0.2 netmask 255.255.255.0 brd 192.168.0.255"
routes_eth0="default via 192.168.0.1"
需要DHCP写入 config_eth0="dhcp"
然后设置启动时自动启用网络连接
cd /etc/init.d
ln -s net.lo net.eth0写入息。在/etc/hosts文件中定义，它将帮助你将那些无法被域名解析器解析的主机名解析成IP地址。在/etc/hosts 写入127.0.0.1    xue  localhost 

设置系统信息
passwd设置root密码
配置引导和启动项 使用/etc/rc.conf配置系统的服务，启动和关闭。此目录下是配置项。

安装系统工具 
系统日志工具 app-admin/sysklogd 安装使用emerge --ask app-admin/sysklogd  然后将它添加到默认运行级别 rc-update add sysklogd default
Cron守护程序 cron守护程序执行计划的命令。如果需要定期执行某些命令（例如每天，每周或每月），则非常方便。emerge --ask sys-process/cronie  添加到默认运行级别 rc-update add cronie default
安装文件索引 emerge --ask sys-apps/mlocate 
为了远程访问，将sshd init脚本添加到默认运行级别 rc-update add sshd default
如果需要终端访问，注释掉在/etc/inittab中的串行控制台部分
联网工具 安装DHCP客户端  emerge --ask net-misc/dhcpcd

安装引导加载程序 GRUB2 emerge --ask --verbose sys-boot/grub:2 EFI用户需要确保GRUB_PLATFORMS="efi-64"已启用，  echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf  然后安装emerge --ask sys-boot/grub:2
安装程序是 grub-install /dev/sda 如果引导程序的磁盘是/dev/sda,安装到/boot/grub/。使用UEFI需要通过grub-install --target=x86_64-efi --efi-directory=/boot 来安装，并且在安装前要确保EFI 系统分区已经挂载。
最后用户在/etc/default/grub文件和/etc/grub.d中特别配置的脚本文件来生成GRUB2。使用 用户在/etc/default/grub文件和/etc/grub.d中特别配置的脚本文件来生成GRUB2。在输出中应该可以找到Linux镜像。

最后退出chroot环境并卸载所有已安装的分区 
cd
umount -l /mnt/gentoo/dev{/shm,/pts,}
umount -R /mnt/gentoo
reboot 来测试是否安装成功。