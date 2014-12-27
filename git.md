##Git使用记录
***
* git add files 将当前work dir中文件添加到stage
* git commit 提交commit
* git reset -- files 从commit中拷贝文件到stage
* git checkout -- files 从stage中拷贝文件到work dir
* git commit -a 添加文件到stage然后执行一个commit
* git checkout HEAD -- files 从commit中拷贝文件到stage和work dir

* git diff          比较work与stage
* git diff HEAD     比较work与commit
* git diff --cached 比较commit与stage
* git diff commitid commitid 比较两个指定的提交版本
* git diff branchname 比较branchname与当前的work
* git diff commitid 比较指定提交版本与work

**以上命令同样可以用于git diffall**

**commit:**
* git commit --amend 创建一个新的提交版本,删除旧的版本

* git checkout -b newbranch 创建新的分支，并切换到新的分支

* git reset HEAD 将当前分支移动到另一个提交位置

                 加 --hard work dir和stage将更新为新位置的文件

                 加 --soft 只更新work dir文件

                 默认只更新stage文件

* git reset 使用当前默认HEAD, 参数处理同上

**上面两个命令 都可以添加 -- files 对指定文件进行回滚

* git push origin newfeature 在远端创建newfeature分支
* git push origin :newfeature 删除远端的newfeature分支(还需要主动删除本地分支)

* git merge --no-ff branch  保留分支信息的合并
