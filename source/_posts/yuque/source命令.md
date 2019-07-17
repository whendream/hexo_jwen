
---

title: source命令

date: 2019-07-08 14:46:14 +0800

tags: []

---
<a name="wNOpI"></a>
# 背景
source命令，之前一直用来加载环境变量的，source一下然后执行某个sh，使其环境变量生效，但对细节没有追究；<br />今天在看公司一个sh脚本的时候发现有个sh只有source命令，按照之前的理解source命令并没有执行的过程呀，难道一个source也可以执行命令吗？

<a name="jiAqZ"></a>
# 过程
百度/google了一下，发现标题多为source和sh/. 执行的区别，那么就先确定了source也有执行命令的效果，且他还有一定的区别

source命令：<br />source命令也称为“点命令”，也就是一个点符号（.）,是bash的内部命令。<br />功能：使Shell读入指定的Shell程序文件并依次执行文件中的所有语句<br />source命令通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录。<br />用法：<br />source filename 或 . filename<br />source命令(从 C Shell 而来)是bash shell的内置命令;点命令(.)，就是个点符号(从Bourne Shell而来)是source的另一名称。

source filename 与 sh filename 及./filename执行脚本的区别在那里呢？<br />1.当shell脚本具有可执行权限时，用sh filename与./filename执行脚本是没有区别得。./filename是因为当前目录没有在PATH中，所有"."是用来表示当前目录的。<br />2.sh filename 重新建立一个子shell，在子shell中执行脚本里面的语句，该子shell继承父shell的环境变量，但子shell新建的、改变的变量不会被带回父shell，除非使用export。<br />3.source filename：这个命令其实只是简单地读取脚本里面的语句依次在当前shell里面执行，**没有建立新的子shell。那么脚本里面所有新建、改变变量的语句都会保存在当前shell里面**。

<a name="dvB21"></a>
# 实例
举例说明：

1. 新建一个test.sh脚本，内容为:A=1
1. 然后使其可执行chmod +x test.sh
1. 运行sh test.sh后，echo $A，显示为空，因为A=1并未传回给当前shell
1. 运行./test.sh后，也是一样的效果
1. 运行source test.sh 或者 . test.sh，然后echo $A，则会显示1，说明A=1的变量在当前shell中

<a name="yN3u3"></a>
# 后记
翻阅文档后，恍然大悟，保留到当前shell中确实可以生效；

<a name="m39uh"></a>
# 资料
[http://www.51testing.com/html/38/225738-206878.html](http://www.51testing.com/html/38/225738-206878.html)

