

# Git分支管理

git所有分支之间彼此互不干扰，各自完成各自的工作和内容。可以在分支使用完后**合并到总分支(原分支)** 上，安全、便捷、不影响其他分支工作；

## main分支(以前为master分支)

从项目创建之初，有且唯一的分支就是主分支。如果之后再创建分支，就是一个一个的从分支，主分支被叫做`main`

## HEAD

在 Git 中，它是一个指针，指向当前所在的本地分支;

git创建新分支。即在当前位置创建一个指针，比如起名为 从分支dev，然后将HEAD指向dev。

分支创建好后的提交都是在dev分支上提交，之前的总提交master分支的提交位置停留在创建从分支dev的位置。而HEAD会跟随新创建的分支，跟随每一次提交不停的先前移动。保持HEAD指针的在提交的最前沿。
在master上新创建的dev分支会继承master分支的所有提交，通过 git log 可以看出来;

![image-20230711213308852](https://gitee.com/juzihhu/image_bed/raw/master/img/202307112133680.png)

## 提交分支

当`dev分支`工作完成，需要合并到`master分支`的时候，也只是**将master指针指向当前dev的位置**，并将HEAD指向master，这时dev分支可以直接删除，也可以不删除，删除的话也只是移除了dev指针，只留下一个master指针，对工作区没有任何的影响;

![image-20230711213517127](https://gitee.com/juzihhu/image_bed/raw/master/img/202307112135466.png)

**切换回主分支**

```shell
分支切换回主分支master
git checkout master
```

**合并分支**
当分支切换回主分支的时候，可以将dev的修改提交合并到master分支上，使用：

```shell
#合并dev到master
git merge dev
```

```shell
#这一次的合并称之为快速合并 fast-forward。只是将master的指针指向了dev最后一次提交的位置。
```

当分支切换回主分支master的时候，就可以删除dev分支使用：

```shell
# 删除dev分支
git branch -d dev
```

**小结：**

```shell
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>
```



## git commit 冲突情况

```shell
准备新的devm分支，继续我们的新分支开发：
$ git checkout -d devm
Switched to a new branch 'devm'
修改merge.txt最后一行，改为：
this is a test for merging main AND dev: by dev
在devm分支上提交：
$ git add merge.txt
$ git commit -m "AND simple"
[feature1 14096d0] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)

```

**切换到main分支：**

```shell
$ git checkout main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Git还会自动提示我们当前master分支比远程的master分支要超前1个提交。
在main分支上把merge.txt文件的最后一行改为：
this is a test for merging main and dev: by main
提交：
$ git add merge.txt 
$ git commit -m "main mergr"
[main 236c69e] main mergr
 1 file changed, 1 insertion(+), 1 deletion(-)

```

**合并冲突产生**

现在，`master（main）`分支和`feature1`分支各自都分别有新的提交，变成了这样：

![image-20230711221512295](https://gitee.com/juzihhu/image_bed/raw/master/img/202307112215134.png)

这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突

```
86159@LAPTOP-AK18IKI3 MINGW64 ~/Desktop/_post (main)
$ git merge devm
Auto-merging merge.txt
CONFLICT (content): Merge conflict in merge.txt
Automatic merge failed; fix conflicts and then commit the result.

```

`git status`查看冲突的文件

![image-20230711221926437](https://gitee.com/juzihhu/image_bed/raw/master/img/202307112220461.png)



**查看merge.txt的内容：**

```shell
$ cat merge.txt
<<<<<<< HEAD
this is a test for merging main and dev: by main
=======
this is a test for merging main AND dev: by dev
>>>>>>> devm

```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，将上面内容删除修改如下后保存：

```shell
$ cat merge.txt
this is a test for merging main and dev: by merge confog

```

**再次提交**

![image-20230711222402404](https://gitee.com/juzihhu/image_bed/raw/master/img/202307112224630.png)

**查看分支的合并情况**

```shell
git log --graph --pretty=oneline --abbrev-commit
```

![image-20230711222524588](https://gitee.com/juzihhu/image_bed/raw/master/img/202307112225693.png)



## 分支管理策略

通常，合并分支时，如果没有冲突，并且分支是单向一条线路继承下来的，git会使用 `fast forword` 模式，但是有些快速合并不能成功，但是又没有冲突时，就会触发分支管理策略，git会自动做一次新的提交。

当两个分支对工作区都进行了修改，但是修改的并不是同一个文件，而是两个不同的文件，也就是不会产生冲突。此时合并的时候，不能使用**“快速合并”**，就会弹框需要你输入一段合并说明。使用快捷键 ctrl+x 退出

合并时禁止快速合并模式

```shell
# 合并dev到master，禁止快速合并模式，同时添加说明
git merge --no-ff -m '' dev
```



## 参考链接

[git merge合并分支，解决冲突](https://blog.csdn.net/mocas_wang/article/details/106750498)

[git的分支管理（详细版）](https://blog.csdn.net/silence_pinot/article/details/111478596)

[Git 分支管理策略](https://mp.weixin.qq.com/s/hRd1UNMRutmA6MGmswweBw)

[我用四个命令概括了 Git 的所有套路](https://labuladong.gitee.io/algo/di-si-zhan-4baf4/wo-yong-si-ad48a/)

# Git 练习网址

https://learngitbranching.js.org/?locale=zh_CN

