Linux是目前应用最广泛的服务器操作系统，基于Unix，开源免费，由于系统的稳定性和安全性，市场占有率很高，几乎成为程序代码运行的最佳系统环境。linux不仅可以长时间的运行我们编写的程序代码，还可以安装在各种计算机硬件设备中，如手机、路由器等，Android程序最底层就是运行在linux系统上的。

# 一、linux的目录结构

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p1.pstatp.com/large/pgc-image/ee3dd42ebce94c7e9a2b83de4ec15d94)

/ 下级目录结构

- bin (binaries)存放二进制可执行文件
- sbin (super user binaries)存放二进制可执行文件，只有root才能访问
- etc (etcetera)存放系统配置文件
- usr (unix shared resources)用于存放共享的系统资源
- home 存放用户文件的根目录
- root 超级用户目录
- dev (devices)用于存放设备文件
- lib (library)存放跟文件系统中的程序运行所需要的共享库及内核模块
- mnt (mount)系统管理员安装临时文件系统的安装点
- boot 存放用于系统引导时使用的各种文件
- tmp (temporary)用于存放各种临时文件
- var (variable)用于存放运行时需要改变数据的文件

# 二、linux常用命令

**命令格式**：命令 -选项 参数 （选项和参数可以为空）

```
如：ls -la /usr
```

**2.1 操作文件及目录**

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p1.pstatp.com/large/pgc-image/cf758f4180d9402d853412a0ddaf5bec)



![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p1.pstatp.com/large/pgc-image/68f400778bfe4113ac5587b83d45ee57)



![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p3.pstatp.com/large/pgc-image/47fe3c8a041647ec8a92231936b38828)



**2.2 系统常用命令**

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p3.pstatp.com/large/pgc-image/44ff5fd0cdd1418eaecb3b7af7697b3f)



![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p1.pstatp.com/large/pgc-image/65cc37fe74a640908722756ed4d1c19c)



**2.3 压缩解压缩**

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p1.pstatp.com/large/pgc-image/2a74e83f954e4131b8d45ef459359aa9)



**2.4 文件权限操作**

- linux文件权限的描述格式解读

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p9.pstatp.com/large/pgc-image/e2537a04891a4fef9122cf5d5086daf6)



- r 可读权限，w可写权限，x可执行权限（也可以用二进制表示 111 110 100 --> 764）
- 第1位：文件类型（d 目录，- 普通文件，l 链接文件）
- 第2-4位：所属用户权限，用u（user）表示
- 第5-7位：所属组权限，用g（group）表示
- 第8-10位：其他用户权限，用o（other）表示
- 第2-10位：表示所有的权限，用a（all）表示

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p1.pstatp.com/large/pgc-image/8716ec81b53946d794375021e59a6c50)



# **三、linux系统常用快捷键及符号命令**

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p9.pstatp.com/large/pgc-image/2b7b6d5b677b4c8ebe4a7c0b58a59e81)



# **四、vim编辑器**

vi / vim是Linux上最常用的文本编辑器而且功能非常强大。只有命令，没有菜单，下图表示vi命令的各种模式的切换图。

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p1.pstatp.com/large/pgc-image/e3f1f632ee974a65958559e6c69d3404)



**4.1 修改文本**

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p3.pstatp.com/large/pgc-image/d7861c74d952464d98b0ebe8258b26f7)



**4.2 定位命令**

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p1.pstatp.com/large/pgc-image/232016e008ce4da19e860ee110f22715)



**4.3 替换和取消命令**

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p3.pstatp.com/large/pgc-image/7b2c2ecad8e144eebdbc7bcd35477bd0)



**4.4 删除命令**

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p9.pstatp.com/large/pgc-image/883cd6f971b24cb8a3b36e80189fe50a)



**4.5 常用快捷键**

![Linux最常用命令：简单易学，但能解决95%以上的问题](http://p1.pstatp.com/large/pgc-image/b06c80b62abb49dbace120735796c236)