# Git #

作成日： 2016/6/22

官网： [https://git-scm.com/](https://git-scm.com/)

当前版本: 2.9.0

## Q1: 差异比较 ##

git diff: 比较工作目录中当前文件和暂存区快照之间的差异

git diff --cached： 高版本可以使用 --staged。 比较暂存区的文件和上次提交之间的差异

![](http://i.imgur.com/iUMYBn8.png)

    $ echo "first line" > demo.txt
    $ git diff
    $ git diff --cached
    $ git add demo.txt
    warning: LF will be replaced by CRLF in demo.txt.
    The file will have its original line endings in your working directory.
    $ git diff
    $ git diff --cached
    diff --git a/demo.txt b/demo.txt
    new file mode 100644
    index 0000000..08fe272
    --- /dev/null
    +++ b/demo.txt
    @@ -0,0 +1 @@
    +first line
    warning: LF will be replaced by CRLF in demo.txt.
    The file will have its original line endings in your working directory.
    $ vi demo.txt
    $ git diff
    diff --git a/demo.txt b/demo.txt
    index 08fe272..f9f23b0 100644
    --- a/demo.txt
    +++ b/demo.txt
    @@ -1 +1,2 @@
     first line
    +next line
    warning: LF will be replaced by CRLF in demo.txt.
    The file will have its original line endings in your working directory.

## Q2: 跟踪远程分支 ##


## Q3： 合并与变基 ##