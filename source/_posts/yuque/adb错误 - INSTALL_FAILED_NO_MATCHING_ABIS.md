
---

title: adb错误 - INSTALL_FAILED_NO_MATCHING_ABIS

date: 2019-04-16 19:41:52 +0800

tags: []

---
# 背景
换组啦，去了UC国际浏览器，被拥抱变化了。还在熟悉阶段，尝试了下adb，然后就碰到了这个INSTALL\_FAILED\_NO\_MATCHING\_ABIS的坑。。。

# 解决方法
`INSTALL_FAILED_NO_MATCHING_ABIS` is when you are trying to install an app that has native libraries and it doesn't have a native library for your cpu architecture. For example if you compiled an app for _armv7_ and are trying to install it on an emulator that uses the _Intel_ architecture instead it will not work.

了解大概原理：应用使用了原生库（NDK，Native Lib），这些库的编译目标通常是arm架构的cpu，在x86的模拟器上运行就会报这样的错误。

知道原因了就简单了，新建一个arm架构的模拟器，蛋疼的是这种模拟器卡的要死，无法工作

# 后续
这个问题的排查其实很快就找到原因了的，但实际解决还是隔了一天。

第一个是：因为太卡了，没有耐心等待手机模拟器的打开，adb install命令也没有耐心等待；

第二个是：在新建arm架构的模拟器的时候，as提示我不建议创建arm架构的，强烈建议使用x86的，最初定位还以为因为系统是64位的而新建的是32位的cpu的问题，导致还是去创建了一个X64的模拟器

# 参考资料
https://stackoverflow.com/questions/24572052/install-failed-no-matching-abis-when-install-apk  
https://juejin.im/post/5a30dca7f265da4324807033
