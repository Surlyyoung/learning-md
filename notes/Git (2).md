---
title: Git
created: '2019-12-12T05:32:49.090Z'
modified: '2020-03-06T01:53:31.075Z'
---

# Git

### git配置
1. 当前目录 git bush
2. git init --在本地创建一个本地仓库 ，文件夹中多出一个.git文件夹
3. git config --global user.email "your email" --输入注册阿里云的username和邮箱
4. git config --global user.name "your name" --global 全局注册 告知身份
5. ssh-keygen -t rsa --输入指令，生成ssh key，默认在c:/user/yourcount ./ssh文件夹下面的id_rsa.pub文件里面 拷贝到代码版本管理处（GitHub等）

### 项目源代码管理
+ 新建项目（已有项目）；
+ 新建repository，复制远程地址；// (git log可查看日志)
+ git remote add origin [repository address]；// 链接远程仓库；
+ git pull origin master；// (把项目（如果有）拉取到本地仓库主分支)
+ git add . // 添加项目（所有或单个文件）至本地仓库
+ git commit -m "description" // 编写描述
+ git push origin master；// (把项目推送到主分支，参数-u指全部文件)
+ git checkout -b [a new branch name]；// (新建一个分支)
+ git push origin [the new branch name]；// (再次推送项目到新建分支)

