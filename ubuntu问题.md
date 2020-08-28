# ubuntu 系统操作

## ubuntu更新后，谷歌浏览器字体颜色很浅

这是由于许多网页并没有指定字体，然后浏览器将调用系统默认字体配置。

解决方法：

安装字体：

>apt-get install ttf-wqy*

编辑字体设置：

>vim /etc/fonts/conf.avail/69-language-selector-zh-cn.conf

将 edit 标签下的第一个 string 标签内容改为 `WenQuanYi Zen Hei`

## 添加开机启动脚本  

需求：使用 nginx + uwsgi 部署 django 项目，nginx 设置开机自启动，uwsgi 是 python 的一个库，编写脚本使它开机运行。

### 建立脚本并给执行权限

```
#！/bin/bash
cd /home/scanv/PycharmProjects/movie/movie_git/  
source /home/scanv/PycharmProjects/test/venv/bin/activate  
uwsgi --ini uwsgi.ini
```

> chmod a+x start-uwsgi.sh

### 复制到 /etc/init.d/

> cp start-uwsgi.sh /etc/init.d/

### 添加开机自启动

> update-rc.d start-uwsgi.sh defaults

出现 warning : `insserv: warning: script 'start-uwsgi.sh' missing LSB tags and overrides`，这是因为脚本缺少初始化信息，初始化脚本不符合 LSB（Linux 标准库）标准。  
在脚本中添加信息：

```
#! /bin/bash

### BEGIN INIT INFO
# Provides:          start-uwsgi
# Required-Start:    $local_fs $network
# Required-Stop:     $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: start uwsgi
# Description:       start uwsgi to provide web server with nginx
### END INIT INFO
```

添加之后再执行命令即可。

### 删除开机启动

update-rc.d -f start-uwsgi.sh remove

## proc 目录解读

### uptime 文件

两列数据，第一列为系统启动时间 num1(s)，第二列是系统空闲时间 num2(s),系统空闲时间有时会比运行时间大，是因为系统空闲时间的计算，是把 SMP (多 cpu)算进去，系统的空闲率(%) = num2/(num1*N) 其中N是SMP系统中的CPU个数。

### loadavg

前三个数表示 5 分钟、10分钟、15分钟内对应的平均负载，第四个数 分子为运行的进程数，分母为总进程数，最后一个是最近运行的进程 id

### meminfo

内存信息

### stat

内核/系统的信息,包括 cpu 每个核心的信息，系统进程整体的统计信息。时间单位一般定义为jiffies(一般地等于10ms)。  
包括：  
CPU指标：user，nice, system, idle, iowait, irq, softirq  
intr：系统启动以来的所有interrupts的次数情况  
ctxt: 系统上下文切换次数  
btime：启动时长(单位:秒)，从Epoch(即1970零时)开始到系统启动所经过的时长，每次启动会改变。
此处指为1500827856，转换北京时间为2017/7/24 0:37:36  
processes：系统启动后所创建过的进程数量。当短时间该值特别大，系统可能出现异常  
procs_running：处于Runnable状态的进程个数  
procs_blocked：处于等待I/O完成的进程个数

### vmstat

显示的是从内核导出的虚拟内存的统计信息。  
包括：  
pgpgin 从启动到现在读入的内存页数  
pgpgout 从启动到现在换出的内存页数  
pswpin 从启动到现在读入的交换分区页数  
pswpout 从启动到现在换出的交换分区页数  

### net/snmp

各层网络协议的收发包的情况。

### net/dev

设备的网卡信息  

bytes: 接口发送或接收的数据的总字节数  
packets: 接口发送或接收的数据包总数  
errs: 由设备驱动程序检测到的发送或接收错误的总数  
drop: 设备驱动程序丢弃的数据包总数  
fifo: FIFO缓冲区错误的数量  
frame: 分组帧错误的数量  
colls: 接口上检测到的冲突数  
compressed: 设备驱动程序发送或接收的压缩数据包数  
carrier: 由设备驱动程序检测到的载波损耗的数量  
multicast: 设备驱动程序发送或接收的多播帧数

### mounts

挂载信息

包括设备地址、挂载路径、文件系统类型、文件挂载参数、文件系统备份频率、开机 fsck 的顺序，为0不check

### diskstats