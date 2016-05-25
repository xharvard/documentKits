## Welcome to Maven 3 ##
作成日： 2016/5/23

Apache Maven是一个软件项目管理和综合工具。基于项目对象模型（POM）的概念，Maven可以从一个中心资料片管理项目构建，报告和文件。**Maven** 当前最新版本为3.3.9。

参考网站：[http://maven.apache.org/](http://maven.apache.org/)

官方文档：[http://maven.apache.org/guides/getting-started/index.html](http://maven.apache.org/guides/getting-started/index.html)

### Maven是什么？ ###
Maven是一个项目管理和综合工具。Maven提供了开发人员构建一个完整的生命周期框架。开发团队可以自动完成项目的基础工具建设，Maven使用标准的目录结构和默认构建生命周期。

在多个开发团队环境时，Maven可以设置按标准在非常短的时间里完成配置工作。由于大部分项目的设置都很简单，并且可重复使用，Maven让开发人员的工作更轻松，同时创建报表，检查，构建和测试自动化设置。

概括地说，Maven简化和标准化项目建设过程。处理编译，分配，文档，团队协作和其他任务的无缝连接。 Maven增加可重用性并负责建立相关的任务。

### Maven历史 ###
Maven最初设计，是以简化Jakarta Turbine项目的建设。在几个项目，每个项目包含了不同的Ant构建文件。 JAR检查到CVS。

Apache组织开发Maven可以建立多个项目，发布项目信息，项目部署，在几个项目中JAR文件提供团队合作和帮助。

### Maven目标 ###
- 项目是可重复使用，易维护，更容易理解的一个综合模型。
- 插件或交互的工具，这种声明性的模式。

Maven项目的结构和内容在一个XML文件中声明，pom.xml 项目对象模型（POM），这是整个Maven系统的基本单元

Apache Maven 是一种创新的软件项目管理工具，提供了一个项目对象模型（POM）文件的新概念来管理项目的构建，相关性和文档。最强大的功能就是能够自动下载项目依赖库。

### Maven安装 ###
想要安装 Apache Maven 在Windows 系统上, 只需要下载 Maven 的 zip 文件，并将其解压到你想安装的目录，并配置 Windows 环境变量。

- JDK安装 （JAVA_HOME)
	- `JAVA_HOME=C:\Program Files\Java\jdk1.8.0_45`
- Maven安装 (M2_HOME, MAVEN_HOME,PATH)
	- `M2_HOME=D:\apache-maven-3.3.9`
	- `MAVEN_HOME=D:\apache-maven-3.3.9`
	- `PATH+=%MAVEN_HOME%\bin;`
- 验证（mvn -version)

		C:\Users\Administrator>mvn -version
		Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:47+08:00)
		Maven home: D:\apache-maven-3.3.9
		Java version: 1.8.0_45, vendor: Oracle Corporation
		Java home: C:\Program Files\Java\jdk1.8.0_45\jre
		Default locale: zh_CN, platform encoding: GBK
		OS name: "windows 7", version: "6.1", arch: "amd64", family: "dos"

### POM ###

POM代表项目对象模型。它是工作在Maven的基本单位。这是一个XML文件。它始终驻留在该项目 pom.xml 基本目录。

POM 包含了目标和插件。在执行任务或目标时，Maven 会在当前目录中读取POM，得到所需要的配置信息，然后执行目标。

部分的配置可以在POM使用如下：

- project dependencies
- plugins
- goals
- build profiles
- project version
- developers
- mailing list

创建一个POM之前，我们应该先决定项目组（groupId），它的名字（artifactId）和它的版本，用来在仓库中唯一标识项目。

    <project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.companyname.project-group</groupId>
    <artifactId>project</artifactId>
    <version>1.0</version>
    <project>

超级POM：所有的POM继承自父类（尽管明确界定）。这个基础POM被称为超级POM，并包含继承默认情况下的值。

    C:\MVN\project>mvn help:effective-pom

通过上述命令可以生成默认的超级POM:

### Maven代理配置 ###
如果要使用代理的话，找到文件 {M2_HOME}/conf/settings.xml, 并把你的代理服务器信息配置写入。

### Maven的本地资源库 ###
Maven的本地资源库是用来存储所有项目的依赖关系(插件jar和其他文件，这些文件被Maven下载)到本地文件夹。很简单，当你建立一个Maven项目，所有相关文件将被存储在你的Maven本地仓库。

默认情况下，Maven的本地资源库默认为 .m2 目录文件夹：

- Unix/Mac OS X – ~/.m2
- Windows – C:\Documents and Settings\{your-username}\.m2

通常情况下，可改变默认的 .m2 目录下的默认本地存储库文件夹到其他更有意义的名称，例如， maven-repo

找到 {M2_HOME}\conf\setting.xml, 添加 localRepository 到其它名称。

	<localRepository>D:\apache-maven-3.3.9\repo</localRepository>

### Maven的中央存储库 ###
当你建立一个 Maven 的项目，Maven 会检查你的 pom.xml 文件，以确定哪些依赖下载。首先，Maven 将从本地资源库获得 Maven 的本地资源库依赖资源，如果没有找到，然后把它会从默认的 Maven 中央存储库 http://repo1.maven.org/maven2/ 查找下载

Maven中心储存库网站已经改版本，目录浏览可能不再使用。这将直接被重定向到 http://search.maven.org/。

### Maven远程存储库 ###
在Maven中，当你声明的库不存在于本地存储库中，也不存在于Maven中心储存库，该过程将停止并将错误消息输出到 Maven 控制台。

> pom.xml
>
    <dependency>
        <groupId>org.jvnet.localizer</groupId>
        <artifactId>localizer</artifactId>
        <version>1.8</version>
    </dependency>

当你建立这个 Maven 项目，它将依赖找不到失败并输出错误消息。

告诉 Maven 来获得 Java.net 的依赖，你需要声明远程仓库在 pom.xml 文件这样：

> pom.xml
> 
    <repositories>
	<repository>
	    <id>java.net</id>
	    <url>https://maven.java.net/content/repositories/public/</url>
	</repository>
    </repositories>

现在，Maven的依赖库查询顺序更改为：

1. 在 Maven 本地资源库中搜索，如果没有找到，进入第 2 步，否则退出。
1. 在 Maven 中央存储库搜索，如果没有找到，进入第 3 步，否则退出。
1. 在java.net Maven的远程存储库搜索，如果没有找到，提示错误信息，否则退出。  

### Maven工作流程 ###
假设你想使用 Log4j 作为项目的日志。这里你要做什么？

#### 1，传统方式 ####
1. 访问 http://logging.apache.org/log4j/
1. 下载 Log4 j的 jar 库
1. 复制 jar 到项目类路径
1. 手动将其包含到项目的依赖
1. 所有的管理需要一切由自己做

如果有 Log4j 版本升级，则需要重复上述步骤一次。

#### 2. Maven的方式 ####
1. 你需要知道 log4j 的 Maven 坐标，例如：
        
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.14</version>

2. 它会自动下载 log4j 的1.2.14 版本库。如果“version”标签被忽略，它会自动升级库时当有新的版本时。

3. 声明 Maven 的坐标转换成 pom.xml 文件。

        <dependencies>
        <dependency>
	    <groupId>log4j</groupId>
	    <artifactId>log4j</artifactId>
	    <version>1.2.14</version>
        </dependency>
        </dependencies>

4. 当 Maven 编译或构建，log4j 的 jar 会自动下载，并把它放到 Maven 本地存储库
5. 所有由 Maven 管理

#### 3.对上述说明 ####
当建立一个Maven的项目，pom.xml文件将被解析，如果看到 log4j 的 Maven 坐标，然后 Maven 按此顺序搜索 log4j 库：

1. 在 Maven 的本地仓库搜索 log4j 
2. 在 Maven 中央存储库搜索 log4j
3. 在 Maven 远程仓库搜索 log4j(如果在 pom.xml 中定义)

Maven 依赖库管理是一个非常好的工具，为您节省了大量的工作。

如何找到 Maven 坐标？
访问 Maven 中心储存库，搜索下载您想要的jar。

### 定制库到Maven ###
- 要使用的 jar 不存在于 Maven 的中心储存库中。
- 您创建了一个自定义的 jar ，而另一个 Maven 项目需要使用。

例如，kaptcha，它是一个流行的第三方Java库，它被用来生成 “验证码” 的图片，以阻止垃圾邮件，但它不在 Maven 的中央仓库中。

#### *操作步骤* ####

1. mvn安装

    下载 “jsch”(生成秘钥对的jar包)，将其解压缩并将 jsch-0.1.50.jar 复制到其他地方，比如：D盘。发出下面的命令：
    
    `mvn install:install-file -Dfile=D:\jsch-0.1.50.jar -DgroupId=com.jcraft -DartifactId=jsch -Dversion=0.1.50 -Dpackaging=jar`    

        [INFO] Scanning for projects...
        [INFO]
        [INFO] ------------------------------------------------------------------------
        [INFO] Building Maven Stub Project (No POM) 1
        [INFO] ------------------------------------------------------------------------
        [INFO]
        [INFO] --- maven-install-plugin:2.4:install-file (default-cli) @ standalone-pom---
        [INFO] Installing D:\jsch-0.1.50.jar to D:\apache-maven-3.3.9\repo\com\jcraft\js
        ch\0.1.50\jsch-0.1.50.jar
        [INFO] Installing C:\Users\xuhaihua\AppData\Local\Temp\mvninstall814494459576652
        3683.pom to D:\apache-maven-3.3.9\repo\com\jcraft\jsch\0.1.50\jsch-0.1.50.pom
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time: 0.353 s
        [INFO] Finished at: 2016-05-23T13:46:50+08:00
        [INFO] Final Memory: 7M/123M
        [INFO] ------------------------------------------------------------------------    

2. pom.xml

    安装完毕后，就在 pom.xml 中声明 kaptcha 的坐标。
    
        <dependency>
            <groupId>com.jcraft</groupId>
            <artifactId>jsch</artifactId>
            <version>0.1.50</version>
        </dependency>

### Maven创建项目 ###
1. 从 Maven 模板创建一个项目

    `mvn archetype:generate -DgroupId=com.test -DartifactId=demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

