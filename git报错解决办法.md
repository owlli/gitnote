# git报错解决办法



##　fatal: The remote end hung up unexpectedly

git使用http上传文件有大小限制,可以改用ssh提交,也可以调大http上传文件大小的限制

解决方法：
windows:
在 .git/config 文件中加入

```shell
[http]
postBuffer = 524288000
```

linux:

```shell
git config --global http.postBuffer 524288000

```



## git status中文乱码

```shell
lzh@lzh-ws:s5p6818_study$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	"\346\211\213\345\206\214/"
nothing added to commit but untracked files present (use "git add" to track)
```

原因(我没看懂):

> core.quotepath的作用是控制路径是否编码显示的选项。当路径中的字符大于0x80的时候，如果设置为true，转义显示；设置为false，不转义

解决办法:

执行

```shell
git config --global  core.quotepath false
```

[参考链接](https://www.cnblogs.com/jason0529/p/8962842.html)



## git pull出现fatal: refusing to merge unrelated histories

在github上创建了一个自动初始化的仓库,在本地的仓库关联这个远程仓库,向上pull时就会报这个错误

```shell
lzh@lzh-ws:x6818_btn_ctrl_lcd$ git remote add origin git@github.com:owlli/x6818_btn_ctrl_lcd
lzh@lzh-ws:x6818_btn_ctrl_lcd$ git pull
warning: no common commits
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:owlli/x6818_btn_ctrl_lcd
 * [new branch]      master     -> origin/master
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> master
lzh@lzh-ws:x6818_btn_ctrl_lcd$ git branch --set-upstream-to=origin/master masterBranch 'master' set up to track remote branch 'master' from 'origin'.
lzh@lzh-ws:x6818_btn_ctrl_lcd$ git pull
fatal: refusing to merge unrelated histories
```

原因:

> 其实这个问题是因为 两个 根本不相干的 git 库， 一个是本地库， 一个是远端库， 然后本地要去推送到远端， 远端觉得这个本地库跟自己不相干， 所以告知无法合并

解决办法:

`--allow-unrelated-histories`选项把把两段不相干的 分支进行强行合并

执行

```shell
lzh@lzh-ws:x6818_btn_ctrl_lcd$ git pull origin master --allow-unrelated-histories
From github.com:owlli/x6818_btn_ctrl_lcd
 * branch            master     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 .gitignore | 52 ++++++++++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 52 insertions(+)
 create mode 100644 .gitignore
 lzh@lzh-ws:x6818_btn_ctrl_lcd$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
lzh@lzh-ws:x6818_btn_ctrl_lcd$ git push
Counting objects: 8, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 2.57 KiB | 877.00 KiB/s, done.
Total 8 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To github.com:owlli/x6818_btn_ctrl_lcd
   5e4cea7..09dfa49  master -> master
```

