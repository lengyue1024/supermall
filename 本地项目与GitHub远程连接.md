fatal: remote origin already exists.报错已成功解决

结局方法：
``` bash 
git remote rm origin
```


## git实现GitHub仓库和本地仓库同步

一般我们为了使得本地创建的项目和远程仓库同步， 我们通常做法都是：
1. 在本地创建项目
2. 在GitHub创建仓库
3. clone仓库到本地
4. 把本地项目复制到仓库目录下，然后再进行提交git push



其实我们完全可以在git上远程到GitHub仓库。
也就是将本地项目与GitHub仓库创建连接关系即可。

1. 先在本地项目下，运行git init初始化为git项目
2. 远程连接仓库,建立连接
` git remote add  origin https://github.com/username/repository.git`
3. 存储到暂存区
``` bash
git add .
git commit -m "commit_message"
```
4. 提交代码到远程仓库
分为两种情况：
case1：
git push -u origin master

case2:
git pull origin master
or 
git pull origin master --allow-unrelated-histories


命令解释：
1. git push -u orgin master命令，将本地库的内容推送到远程库中去
2. origin：本地库的别名
master：分支名，GitHub默认分支名
3. git init #把当前目录变为仓库，并且建立.git隐藏文件
4. 如果你在创建仓库的时候勾选了添加LCENCE证书文件或者README等，反正只要仓库创建后不是空的，在你进行push的时候都会出现这个错误“error: src refspec master does not match any.
error: failed to push some refs to 'https://github.com/username/repository.git'' ”错误，都是因为仓库和本地文件不一致，导致本地和仓库不一致，所以需要先把仓库的pull拉取下来，让本地库和远程库同步，再去提交本地项目.这需要用到case2情况。
5. 报错信息：“fatal: refusing to merge unrelated histories” 
这种报错信息，是因为一个是本地库，一个是远端库，然后本地要去推送到远端， 远端觉得这个本地库跟自己不相干， 所以告知无法合并。
具体的方法， 一个种方法： 是 从远端库拉下来代码 ， 本地要加入的代码放到远端库下载到本地的库， 然后提交上去 ， 因为这样的话， 你基于的库就是远端的库， 这是一次update了。

第二种方法：
使用这个强制的方法

git pull origin master --allow-unrelated-histories

后面加上 --allow-unrelated-histories ， 把两段不相干的 分支进行强行合并

后面再push就可以了 。


6. 在使用git pull时,经常会遇到报错:
error: The following untracked working tree files would be overwritten by merge:
xxx
Please move or remove them before you can merge

这是因为本地有修改,与云端别人提交的修改冲突,又没有merge.

如果确定使用云端的代码,最方便的解决方法是删除本地修改,可以使用以下命令:

git clean  -d  -fx xxx文件 
eg:
git clean -d -fx .gitignore README.md 

d  -----删除未被添加到git的路径中的文件
f  -----强制运行
x  -----删除忽略文件已经对git来说不识别的文件

注意:该命令会删除本地的修改,最好先备份再使用

git clean 参数
-n 显示 将要 删除的 文件 和 目录
-f 删除 文件，-df 删除 文件 和 目录

git clean -n
git clean -df
git clean -f


其实可以那样，把目录下的给忽略掉

