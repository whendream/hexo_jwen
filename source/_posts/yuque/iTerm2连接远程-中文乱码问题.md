
---

title: iTerm2连接远程-中文乱码问题

date: 2019-04-16 19:48:00 +0800

tags: []

---
# 现象
mac 上用是iterm2终端, Shell 环境是zsh。

ssh 到Linux 服务器上查看一些文件时，中文乱码。  这种情况一般是终端和服务器的字符集不匹配，MacOSX下默认的是utf8字符集。

iterm2本地显示中文正常，但ssh到服务端发现中文乱码

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555415235863-d2ac29ee-4825-402d-9147-ec93c5f3edfa.png)

# 解决方法
输入locale可以查看字符编码设置情况，而我的对应值是空的。  而默认的.zshrc没有设置为utf-8编码，所以本地和服务器端都要在.zshrc设置，步骤如下，(bash对应.bash\_profile或.bashrc文件)。

1.在终端下输入  
vim ~/.zshrc

2.在文件内容末端添加：

```
export LC\_ALL=en\_US.UTF-8 
export LANG\=en\_US.UTF-8
```
接着重启一下终端，或者输入source ~/.zshrc使设置生效。  
连接服务器，中文显示都正常了。

  
\---------------------  
作者：大道泛兮  
来源：CSDN  
原文：https://blog.csdn.net/u013931660/article/details/79443037  
版权声明：本文为博主原创文章，转载请附上博文链接！
