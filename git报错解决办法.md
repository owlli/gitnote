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

执行

```shell
git config --global  core.quotepath false
```

原因

> core.quotepath的作用是控制路径是否编码显示的选项。当路径中的字符大于0x80的时候，如果设置为true，转义显示；设置为false，不转义

[参考链接](https://www.cnblogs.com/jason0529/p/8962842.html)

