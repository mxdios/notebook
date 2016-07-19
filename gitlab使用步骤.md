步骤：

1. 去一个新的目录，存放git项目
2. 将gitlab上已有的项目克隆下来， git clone git@git.debug.live:apt/codemake-ios.git
3. 克隆成功，将已有项目拷贝到克隆下来的文件夹里，和.git文件并列
4. 进入存放.git和项目的文件夹，关联.git文件， git add .
5. commit到本地库， git commit -m "commit的信息记录"
6. 查看远程库上的分支 git branch 
7. 如果只有一个 * master 分支，创建一个dev分支  git branch dev
8. 切换到dev分支 git checkout dev
9. push到远程库  git push origin dev