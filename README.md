# 个人博客
准备从博客园迁移到个人博客上来
- 服务器： vultr
- 博客框架： hexo
- 代码管理： github

工作流程 
1. 本地windows机器写博客（公司 or 在家），github管理代码;
2. 通过hexo命令调试，部署到linux上的私人git上；
3. linux服务器上搭建私人git，用于存放静态资源；
4. linux上通过nginx反向代理到指定目录；
5. 开放80端口用于访问

