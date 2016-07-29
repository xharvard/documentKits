
## NFS ##
NFS（Network File System）即网络文件系统，是FreeBSD支持的文件系统中的一种，它允许网络中的计算机之间通过TCP/IP网络共享资源。在NFS的应用中，本地NFS的客户端应用可以透明地读写位于远端NFS服务器上的文件，就像访问本地文件一样。


## Linux Bridge ##
Linux Bridge 模拟了物理网络中网桥的感念，即将若干了服务器的端口加入到网桥中，网桥端口对端相连的设备通过发送报文给 Linux Bridge, 并通过Linux Bridge 学习报文SMAC和查找报文的DMAC转发到相应的目的地，这点和普通的二层交换机非常类似。 Linux Bridge 的端口可以是物理网卡端口也可以是虚拟的端口。

## supervisorctl ##
python写的Linux后台进程管理程序。



## 网络传输设备 ##

- 中继器（Repeater）: 物理层设备，对传输信号进行放大，以减少远距离传输导致信号衰减带来的误码率，属于对信号再生或还原的设备。
- 集线器(Hub)
- 网桥(Bridge)
- 交换器(Switch)
- 路由器(Router)


## apt-get ##
Advanced Package Tool，又名apt-get

apt-get是一条linux命令，适用于deb包管理式的操作系统，主要用于自动从互联网的软件仓库中搜索、安装、升级、卸载软件或操作系统。

使用apt-get的主流Linux系统包括Debian和Ubuntu变异版本。

apt-get命令一般需要root权限执行，所以一般跟着sudo命令。
例：sudo apt-get xxxx

- apt-get install packagename  安装
- apt-get remove packagename 卸载（保留配置文档）
- apt-get remove --purge packagename 卸载（删除配置文档)
- apt-get autoremove packagename 删除包及其依赖的软件包
- apt-get clean 安装的软件备份也删除
- apt-get upgrade 更新软件包
- apt-get dist-upgrade 将系统升级到新版本
- apt-cache search string 在软件包列表中搜索字符串
- apt-cache show packagename 获取包的相关信息

## yum ##
yum命令是在Fedora和RedHat以及SUSE中基于rpm的软件包管理器，它可以使系统管理人员交互和自动化地更细与管理RPM软件包，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

> yum(选项)(参数)

> 选项
> 
> -h：显示帮助信息； 
> 
> -y：对所有的提问都回答“yes”； 
> 
> -c：指定配置文件； 
> 
> -q：安静模式； 
> 
> -v：详细模式； 
> 
> -d：设置调试等级（0-10）；
> 
> -e：设置错误等级（0-10）；
>  
> -R：设置yum处理一个命令的最大等待时间； 
>   
> -C：完全从缓存中运行，而不去下载或者更新任何头文件。

> 参数

> install：安装rpm软件包； 

> update：更新rpm软件包； 
> 
> check-update：检查是否有可用的更新rpm软件包； 
> 
> remove：删除指定的rpm软件包；
> 
>  list：显示软件包的信息； 
>  
>  search：检查软件包的信息； 
>  
>  info：显示指定的rpm软件包的描述信息和概要信息； 
>  
>  clean：清理yum过期的缓存； 
>  
>  shell：进入yum的shell提示符； 
>  
>  resolvedep：显示rpm软件包的依赖关系；
>  
>  localinstall：安装本地的rpm软件包； 
>  
>  localupdate：显示本地rpm软件包进行更新； 
>  
>  deplist：显示rpm软件包的所有依赖关系。

