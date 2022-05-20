# Git语法
[toc]

​		Git是分布式版本控制系统，只能追踪文本文件的变更。所有需要记录的变更文件都必须在版本仓库中。

1.设置git仓库: git init

2.添加文件到仓库：git add readme.txt

3.提交文件：git commit -m "write a txt"

## 版本控制

1.查看当前文件状态：git status

![image-20220518234748209](../../Pic/image-20220518234748209.png)

2.查看修改：git diff

3.Git中，使用HEAD指针指向当前版本，跳转到以前的或者现在版本可以使用：

- git reset HEAD^——跳转到前一个版本
- git reset HEAD~3——跳转到前3个版本
- git reset 1ab3...——跳转到指定版本

git log：Git会为每次提交记录生成独一无二的哈希值，可以使用git log命令查看过往的提交版本。
git log --pretty=oneline——简化输出

git reflog：查看执行的历史命令

### Git暂存区

​		在git init的目录下(工作区)，有一个.git文件夹，这是git仓库的版本库，用于标识git工作的内容。版本库里面分为暂存区和分支。

​		git add就是把工作区更改的文件提交到暂存区，git commit把暂存区的文件提交到正在使用的分支上。把工作区做的变更，在.git里面记录下来，这样就是git的工作原理。

![git-repo](../../Pic/0.jpeg)



4.撤销文件变更

git restore \<file\>  ——撤销工作区的变更
git restore --staged \<file\> ——撤销暂存区的变更

git rm \<file\> 删除文件，并把此次变更提交到暂存区
撤销删除，可以使用git restore --staged \<file\>

## 远程仓库

​		个人用户使用github作为远程仓库，公司内部可能使用gitlab来搭建远程仓库。

### 建立远程仓库

1.github上新建一个仓库

2.`ssh-keygen -t rsa -C "email@example.com"`  生成公私钥

3.把生成的私钥放到github账户中。

存在的问题：

- 存在几个不通主机的密钥文件

  config文件设置:

  ```shell
  Host github
      User git  
      HostName github.com
      PreferredAuthentications publickey
      IdentityFile "C:\Users\xxx\.ssh\git_rsa"
      ServerAliveInterval 300
      ServerAliveCountMax 10
  ```

- ssh-add \<rsafile\>

- ssh报错：Error connecting to agent

  ssh未启动，需要去服务上启动，找到Openssh服务，改禁用为手动或者自动

  然后 ssh-agent 启动ssh服务

但是我搞了半天还是github push时候会权限拒绝，只有ssh key命名为id_rsa才行，不知道什么原因。😍

**4.推送远程仓库**

建议本地仓库与远程仓库的连接:
git remote add origin git@github.com:xxx/xxx.git 

git push -u origin master

5.删除远程仓库

git remote rm \<name\>

删除远程仓库前使用 git remote -v 查看远程信息



























