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

### 删除开机启动。

update-rc.d -f start-uwsgi.sh remove
