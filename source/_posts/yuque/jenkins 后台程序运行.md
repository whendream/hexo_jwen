
---

title: jenkins 后台程序运行

date: 2019-04-17 19:56:11 +0800

tags: []

---
\# 背景
=====

jenkins持续集成，需要任务后台执行（nohup执行）结果发现jenkins的job执行完后，看不到运行的进程

\# 步骤
=====

原因就是这么一个情况：**Jenkins任务结束时候自动关掉了所有的子进程**

**不过可以设置一些东西让其可以在后台运行**

**其实就是在脚本中加入一句**

BUILD\_ID=DONTKILLME

问题的根本在于是Jenkins使用processTreeKiller杀掉了所有子进程，而且这是Jenkins的默认行为。其实回头来看这个问题，就发现Jenkins的做法非常合理。当一次build异常结束，或被人终止时，必然需要结束所有这次build启动的子进程。下面的link提供了更多细节，以及解决方法。https://wiki.jenkins-ci.org/display/JENKINS/ProcessTreeKiller
