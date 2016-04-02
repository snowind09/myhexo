---
title: Hexo 换终端/换电脑小记
date: 2016-04-02 14:00:53
tags: [Hexo]
categories: 技术
---
## 主要思路
1. [参考第四节](http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more)
2. 流程图
![](http://7xshxx.com2.z0.glb.clouddn.com/hexo_process.png)

3. 对于多终端来说，按照git pull->编辑博客->git push->hexo g -d的顺序就可以实现多终端同步效果

## 实施前提

1. 旧终端/电脑已安装hexo环境
2. 旧终端/电脑hexo正常工作
3. 旧终端/电脑能通过hexo g -d正常发布至远程xxx.github.io

## 实施步骤

1. 新建远程repo

 在github上新增repo，例如名为myhexo，地址为github.com/xxx/myhexo

2. 在**旧终端**中创建本地repo
 
 进入本地hexo目录

   > git init

 查看未提交的文件（默认不包含public、deploy文件）

   >git status

  根据查看结果将本地hexo建站原始文件纳入版本控制（我本地是scaffolds、source、themes文件夹以及.gitignore、_config.yml、package.json文件）

   >git add ...
   >git commit -m ...
  
  需要注意的是.gitignore中需要增加一行，忽略~结尾的文件
   
  > *~
 
3. 在**旧终端**中创建远程分支连接

   >git remote add origin git@github.com:xxx/myhexo.git
   >git remote -v 
   
   显示如下：
   >origin    git@github.com:XXX/myhexo.git (fetch)
   >origin    git@github.com:XXX/myhexo.git (push)
   
4. 在**旧终端**中提交本地文件至远程repo

  >git push -u origin master

5. 换了终端以后

  在**新终端**中安装node、git环境，配置github sshkey
 
  在**新终端**中创建空目录作为hexo工作目录，从远程仓库中clone出之前备份的repo
  
  > mkdir hexo
  > cd hexo
  > git init
  > git clone git@github.com:xxx/myhexo.git

  在**新终端**中git clone成功后，本地出现myhexo文件夹，开始安装hexo环境
   
  > cd myhexo
  > npm install hexo
  > npm install
  > npm install hexo-deployer-git

  查看本地同步效果，访问localhost:4000
  
  > hexo g
  > hexo server

6. 在**新终端**中继续使用和管理hexo
    
   新建文章并修改
  
 > hexo new xxx
   
   提交原始md文件至远程repo
 
 > git status
 > git add
 > git commit -m xx
 > git push
 
   发布静态网页
 
  > hexo g -d

7. 切换至**旧终端**使用hexo

   更新网站原始文件

 > git pull
 > hexo g
 > hexo server

  测试更新成功后，表明终端顺利切换完成

  
## 遇到的坑
1. windows环境下，本地4000端口可能被占用，此时需要修改本地hexo目录下的
node_modules/hexo-server/index.js 的端口配置
