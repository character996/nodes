# gitbook

## 安装node.js

安装 NPM
>apt install npm

安装新版本的nodejs

npm安装了Node工具包n，使用apt-get安装不上新版本的nodejs
>npm install n -g

安装nodejs
>n stable

## 安装gitbook

>npm install gitbook-cli -g

安装完之后创建电子书
>gitbook init

如果执行gitbook init之后，一直卡在 installing 上，可能是因为npm使用的国外的源，可以切换为国内的源,版本的问题可以切换为3.0.0版本

### 切换淘宝的源

>npm config set registry=http://registry.npm.taobao.org -g

### 切换版本

```
gitbook uninstall 3.2.3 #卸载
gitbook fetch 3.0.0 #安装
```

## gitbook配置

`gitbook init`之后，会创建两个文件，README.md和SUMMARY.md（目录文件），编辑目录文件使文件可以展示。

### SUMMARY.md配置为：

```
# Summary
* [Introduction](README.md)
* [Part I linux知识](part1/README.md)
     * [Gentoo知识](part1/gentoo.md)
```
后续可根据内容继续添加。

### book.json文件

此文件是gitbook的配置文件。内容为：

```
{
 "title": "笔记",
 "auther":"xuerb",
 "description":"学习笔记总结",
 "language":"zh-hans",
 "plugins": [
    "highlight",
    "-sharing",
    "search",
    "-lunr",
    "prism",
    "anchors",
    "todo"
 ]
}
```

之后运行`gitbook serve` 可在网页中看到展示和生成静态文件。
`gitbook build`可在 _book 文件夹下创建静态文件
