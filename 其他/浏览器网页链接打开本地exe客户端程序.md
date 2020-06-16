浏览器网页链接打开本地exe客户端程序



我们经常可以看到在浏览器打开客户端的场景：浏览器打开 QQ 聊天窗口，百度网盘打开网盘客户端下载等。

我们如何使用浏览器网页链接打开本地 exe 客户端程序？

**步骤如下**

1. 新建注册表文件 `szztClient.reg`, **客户端的名称和客户端的地址可以自己定义。**



```reg
[HKEY_CLASSES_ROOT\szztClient]
@="szztClientProtocol"
"URL Protocol"=""

[HKEY_CLASSES_ROOT\szztClient\DefaultIcon]
@="C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe,1"

[HKEY_CLASSES_ROOT\szztClient\shell]
@=""

[HKEY_CLASSES_ROOT\szztClient\shell\open]
@=""

[HKEY_CLASSES_ROOT\szztClient\shell\open\command]
@="\"C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe\" \"%1\""
```

1. 双击运行写入注册表
2. 在网页添加链接



```html
<a href="szztClient://">打开客户端</a>
```







# [Windows 注册自定义的协议](https://www.cnblogs.com/z5337/p/4713474.html)