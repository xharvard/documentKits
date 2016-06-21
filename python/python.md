# python #

作成日： 2016/6/1

参考网站：[https://www.python.org/](https://www.python.org/)



## 第一章 基础知识 ##
### 1.1 安装python ###

- windows系统: 从官网下载安装包， 一步一步安装即可。
- Linux/unix系统: 通过系统的包管理器安装即可。 (Debian Linux: apt-get install python)

还有一些其他语言实现的python版本: Jython(java) 和 IronPython(C#)

### 1.2 python解释器 ###
安装好了，在命令行输入 python 即可进入python解释器模式，可以输入代码互动。

### 1.3 基础介绍 ###

- input():  获取用户输入  如果输入字符串，必须要加""号
- raw_input():  获取用户输入, 输入原始的即可，推荐使用。
- import math: 引入math模块
- #: 表示注释
- str():  将值转换成字符串
- repr(): 与str()相反，以python的形式显示
- 长字符串:  使用三个引号代替普通引号。  '''this is long string'''
- 原始字符串: 比如\n， 会被解释器当做换行。所以必须要用\(\\n)来转义。或者使用r'' 的形式
- Unicode字符串:  u'' 将字符串以Unicode存储，特别是中文字符的时候必须这么写


## 第二章 列表和元组 ##

python有6种内建序列

- 列表
- 元组
- 字符串
- Unicode字符串
- buffer对象
- xrange对象

列表和元组的区别: 列表可以修改，元组不行。

序列定义: person = ['jone', 23, 'man'] 

当然，序列可以包含其他序列

### 2.1 序列通用操作 ###
#### 2.1.1 索引 ####
从0开始递增的编号。最后一个元素可是用-1标示，最后第二个则是-2,一次类推。

    >>> greeting = 'hello'
    >>> greeting[0]
    'h'

#### 2.1.2 分片 ####
访问一定范围内的元素，通过冒号隔开2个索引。(前面的索引包含，后面的索引不包含)

    >>> tag = '<a href="http://www.python.org">Python web site</a>'
    >>> tag[9:30]
    'http://www.python.org'
    >>> tag[32:-4]
    'python web site'

    快捷操作:
    >>> num = [1, 2, 3, 4]
    >>> num[-3:-1]
    [2, 3]
    >>> num[-3:]
    [2, 3, 4]
    >>> num[:3]
    [1, 2, 3]
    >>> num[:]
    [1, 2, 3, 4]





