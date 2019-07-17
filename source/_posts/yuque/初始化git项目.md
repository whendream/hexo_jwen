
---

title: 初始化git项目

date: 2019-04-16 20:24:28 +0800

tags: []

---
<a name="66879f44"></a>
### Command line instructions

<a name="d8418652"></a>
##### Git global setup

git config --global user.name "你的名字"<br />
git config --global user.email "你的邮箱"

<a name="ebc6fc20"></a>
##### Create a new repository

git clone ${git地址} cd demo<br />
touch README.md<br />
git add README.md<br />
git commit -m "add README"<br />
git push -u origin master

<a name="97618b45"></a>
##### Existing folder or Git repository

cd existing_folder<br />
git init<br />
git remote add origin ${git地址} git add .<br />
git commit<br />
git push -u origin master


