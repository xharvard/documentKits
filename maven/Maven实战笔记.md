# Maven实战 #

作成日： 2016/5/25

参考网站：[http://maven.apache.org/](http://maven.apache.org/)

官方文档：[http://maven.apache.org/guides/getting-started/index.html](http://maven.apache.org/guides/getting-started/index.html)

## 第一章 Maven简介 ##
Apache Maven是一个软件项目管理和综合工具。

构建工具： make -> ant -> maven

## 第二章 Maven的安装和配置 ##

### 2.1 Windows 安装 ###
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

maven升级： 下载新版本的zip文件，修改环境变量指向新版本的文件路径。

### 2.2 Linux 安装 ###
同上类似，下载tar.gz包，解压，配置环境变量。

### 2.3 安装目录分析 ###
目录结构：

    |--bin: 包含mvn运行脚本
    |    `--mvn
    |    `--mvn.cmd
    |    `--m2.conf: classworlds的配置文件
    |--boot
    |    `--plexus-classworlds-2.5.2.jar: maven自己的类加载器
    |--conf
    |    `--setting.xml: 配置信息
    |--lib: maven运行时需要的lib库
    |--LICENSE:软件许可证
    |--NOTICE:包含的第三方库
    |--README.txt:maven简介

~/.m2 文件：配置maven默认本地仓库。 （~代表用户家目录）

    mvn help:system   命令可以列出所有系统环境变量

### 2.4 设置http代理 ###
如果要使用代理的话，找到文件 {M2_HOME}/conf/settings.xml, 并把你的代理服务器信息配置写入。

一般推荐针对每个用户配置settings.xml，做法就是把maven安装目录的settings.xml拷到自己的版本库(~/.m2)再修改。

    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>proxyuser</username>
      <password>proxypass</password>
      <host>proxy.host.net</host>
      <port>80</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>

### 2.5 安装eclipse插件 m2eclipse ###
一般eclipse都已经集成了，如果没有，可以到eclipse marketplace安装。

### 2.6 maven最佳实践 ###
#### 2.6.1 设置MAVEN_OPTS环境变量 ####
通常需要设置MAVEN_OPTS值为 -Xms128m -Xmx512m ,设置java执行的内存参数。尽量不要修改mvn.bat文件，如果升级版本需要重复修改。

#### 2.6.2 配置用户范围settings.xml ####
- {M2_HOME}/conf/settings.xml  全局配置
- ~/.m2/settings.xml   用户配置(推荐)

#### 2.6.3 不要使用IDE内嵌的maven ####
经常要使用命令行操作maven,可能会出现本地版本和IDE版本不同，容易出问题。在eclipse里选择Preferences，在里面找到maven选择，配置即可。

## 第三章 Maven使用入门 ##
### 3.1 编写POM ###
POM: Project Object Model (项目对象模型)

以hello-world项目为例，说明：
先创建hello-world文件夹，在里面新建pom.xml文件，内容如下：

	<project xmlns="http://maven.apache.org/POM/4.0.0"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
		http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	  <groupId>com.mvn</groupId>
	  <artifactId>hello-world</artifactId>
	  <version>0.0.1-SNAPSHOT</version>
	  <name>Maven hello world project</name>
	</project>

    说明：
    modelVersion: 对于maven3版本，只能是4.0.0
    groupId: 定义项目属于哪个组，比如 com.mvn
    artifactId: 定义当前maven项目在组中的唯一ID,比如 hello-world
    version: 项目的当前版本,比如 0.0.1-SNAPSHOT
    name: 更详细的项目描述

### 3.2 编写主代码 ###
代码和maven是解耦的，可以互相独立。

主代码： 位于 src/main/java目录 (maven约定),然后创建文件 com/mvn/helloworld/HelloWorld.java
  
> ..\hello-word\src\main\java\com\mvn\helloworld\HelloWorld.java   

    package com.mvn.helloworld;

	    public class HelloWorld{
		public String sayHello(){
			return "Hello Maven";
		}
		
		public static void main(String[] args){
			System.out.println(new HelloWorld().sayHello());	
		}
    }

执行 mvn clean compile 命令，maven会自动编译刚刚的java文件，在target目录下生成classes文件。
      
> ..\hello-word\target\classes\com\mvn\helloworld\HelloWorld.class

### 3.3 编写测试代码 ###
测试代码：位于 src/test/java目录 (maven约定),然后创建文件 com/mvn/helloworld/HelloWorldTest.java

> ..\hello-word\src\test\java\com\mvn\helloworld\HelloWorldTest.java

    package com.mvn.helloworld;

	import static org.junit.Assert.assertEquals;
	import org.junit.Test;

	public class HelloWorldTest{
		
		@Test
		public void testSayHello(){
			HelloWorld helloworld = new HelloWorld();
			String result = helloworld.sayHello();
			assertEquals("Hello Maven" , result);
		}
	}

pom.xml添加junit模块,maven会自动到中央仓库(http://repo1.maven.org/maven2/)下载junit-4.7.jar。

	<project xmlns="http://maven.apache.org/POM/4.0.0"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
		http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	  <groupId>com.mvn</groupId>
	  <artifactId>hello-world</artifactId>
	  <version>0.0.1-SNAPSHOT</version>
	  <name>Maven hello world project</name>
	  <dependencies>
		<dependency>
		  <groupId>junit</groupId>
		  <artifactId>junit</artifactId>
		  <version>4.7</version>
		  <scope>test</scope>
		</dependency>
      </dependencies>
	</project>


执行 mvn clean test 命令，maven会自动编译刚刚的java文件，在target目录下生成test-classes文件。并且执行测试。

>..\hello-word\target\test-classes\com\mvn\helloworld\HelloWorldTest.class
 
  
### 3.4 打包和运行 ###

执行 mvn clean package 命令，在target下生成hello-world-0.0.1-SNAPSHOT.jar

执行 mvn clean install 命令，把jar包安装到maven本地库里，可供其他项目使用

默认打包生成的jar是不能直接运行的，因为带有main方法的类信息不会添加到manifest中。(jar中的META-INF/MANIFEST.MF文件），为了生成可执行的jar,需要maven-shade-plugin插件。

添加build模块

    <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>1.2.1</version>
		<executions>
	      <execution>
			 <phase>package</phase>
			 <goals>
				<goal>shade</goal>
			 </goals>
			 <configuration>
				<transformers>
				  <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
				    <mainClass>
					  com.mvn.helloworld.HelloWorld
					</mainClass>
				  </transformer>
				</transformers>
			 </configuration>
			</execution>
        </executions>
	  </plugin>
    </plugins>
    </build>

再次执行 mvn clean install 命令，在target下会生成2个jar包:

- hello-world-0.0.1-SNAPSHOT.jar (可执行)
- original-hello-world-0.0.1-SNAPSHOT.jar (不可执行)

执行 java -jar ./target/hello-world-0.0.1-SNAPSHOT.jar, 控制台输出 Hello Maven。

### 3.5 使用Archetype生成项目骨架 ###
让maven自动生成代码框架

    mvn archetype:generate
按照提示一步步往下操作，基本跟上面手动创的流程是一样的。只不过maven简化了很多细节。



## 第四章 背景案例 ##

