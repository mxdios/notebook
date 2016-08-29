步骤：

1. 去一个新的目录，存放git项目

2. 将gitlab上已有的项目克隆下来， `git clone git@git.xxxxxx.git`

3. 克隆成功，将已有项目拷贝到克隆下来的文件夹里，和.git文件并列

4. 进入存放.git和项目的文件夹，关联.git文件， git add .

5. commit到本地库， `git commit -m "commit的信息记录"`

6. 查看本地分支 `git branch `

7. 查看远程分支 `git branch -r`

7. 如果只有一个 * master 分支，创建一个dev分支  `git branch dev`

8. 切换到dev分支 `git checkout dev`

9. push到远程库  `git push origin dev`

多分支使用

1. 现在处在一个普通分支`0-testBarch`，做完修改操作，进行commit push操作

	1. `git add .`将所有修改的文件添加到git本地库里

	2. `git commit -m "push change"`进行commit操作

	3. `git push`进行push操作，这时便将在`0-testBarch`分支上的修改push到了服务器上的`0-testBarch`分支上了

2. 进行`merge`请求，在gitlab上创建merge，@项目管理者，做merge操作，将本次修改的`0-testBarch`合并到主要分支`dev`上。

3. 有新的修改工作，创建一个新的分支`1-new-barnch`，做修改操作

4. `git checkout dev`第一步首先要切换到`dev`分支上

5. `git pull`把`dev`上所有修改的代码pull下来，这一步必不可少，不然在旧代码的基础上修改，最后将乱的一塌糊涂。dev是主要分支，代码是最全最新的，每次其他分支修改完毕都要`merge`到主分支

6. `git checkout -b 1-new-barnch` 基于dev最新代码，创建一个新的本地分支并切换到该分支上

7. `git push -u origin 1-new-barnch` push该分支到远程分支上，在这个分支上做代码修改操作，修改完毕再重复上面的步骤。

8. 其他操作

	1. `git branch --delete origin 1-new-barnch` 删除远程分支

	2. `git branch -D 1-new-barnch` 删除本地分支

	3. `get merge dev`  merge dev分支的最新代码到本地,如果有冲突可以看到提示。

