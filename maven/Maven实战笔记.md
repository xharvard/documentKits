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



## 第四章 坐标和依赖 ##
### 4.1 坐标 ###
唯一标示maven中的构件。就跟平面坐标(x,y)唯一标示平面的一个点一样。

坐标包括：groupId, artifactId, version, packaging, classifier

### 4.2 坐标详解 ###

    <groupId>junit</groupId>
	<artifactId>junit</artifactId>
	<version>4.7</version>
    <packaging>jar</packageing>
    
    groupId: 当前maven项目隶属的实际项目   - 必须
    artifactId: 定义项目模块   - 必须
    version: 项目当前版本    - 必须
    packaging: 项目的打包方式   - 可选，默认为jar
    classifier: 定义附属构件，如javadoc, 源代码等  - 不可直接定义，通过插件完成
    最后查询的文件格式  artifactId-version.packaging  (junit-4.7.jar)

### 4.3 依赖的配置 ###
    <dependencies>
	  <dependency>
	    <groupId>...</groupId>
	    <artifactId>...</artifactId>
	    <version>...</version>
        <type>...</type>
	    <scope>...</scope>
        <optional>..</optional>
        <exclusions>
          <exclusion>
            ...
          </exclusion>
        </exclusions>
        ...
	  </dependency>
      ...
    </dependencies>

dependencies下可以包含一个或多个dependency。
每个dependency包含：

- groupId: 坐标信息
- artifactId: 坐标信息
- version: 坐标信息
- type: 依赖类型，对应于坐标的packaging。默认为jar
- scope: 依赖范围 4.4详细描述
- optional: 可选依赖范围 4.7详细描述
- exclusions: 排除传递依赖性

### 4.4 依赖范围 ###
maven在编译，测试，运行时使用的classpath应该是不同的，所有scope就是针对这种情况设计的。

比如，junit库在运行时是不需要的，但在测试时是需要的。

Maven的几种依赖范围：

- compile: 编译依赖范围，默认范围。对于编译，测试，运行三种classpath都有效。
- test: 测试依赖范围，只对于测试classpath有效。 如junit。
- provided: 已提供依赖范围，对于编译和测试classpath有效，运行无效。如servlet-api,编译，测试需要，运行时有容器提供，不需要maven重复引入。
- runtime: 运行时依赖范围，对于测试和运行classpath有效，编译无效。如JDBC驱动实现，编译时只需jdk提供的接口即可，测试和运行时才需要具体实现驱动。
- system: 系统依赖范围，与provided一致。只是该范围依赖是必须通过systemPath显示指定依赖文件的路径。不通过maven仓库解析，与本机系统绑定。谨慎使用。
- import: 导入依赖范围，不会对三种classpath产生影响。

### 4.5 传递依赖性 ###
maven会自动管理包的依赖关系，比如spring-framework-2.5.6.jar包需要依赖commmos-logging.jar包，maven自动把依赖包commmos-logging.jar下载下来管理。而且依赖范围也可以传递过来。

假设A依赖于B，B依赖于C，我们说A对于B是第一直接依赖，B对于C是第二直接依赖，A对于C是传递性依赖。

### 4.6 依赖调解 ###
假如A>B>C>X(1.0),，A>D>X(2.0)，X是A的传递性依赖，但是有2个版本的X,那么哪个版本会被maven解析呢？

- 调解第一原则：路径最近者优先。即X(2.0)会被解析。如果路径相同，则第二原则。
- 调解第二原则：POM中顺序靠前的优先。

### 4.7 可选依赖 ###
不推荐使用。

    <optional>..</optional> 设置为true，即表示为可选依赖

A>B，B>X(可选)，B>Y(可选)，这种情况X，Y将不会对A有影响。 A如果要使用X或者Y,得自己声明依赖。

### 4.8 最佳实践 ###
#### 4.8.1 排除依赖 ####
某个依赖的jar包或者文件不想要，或者想换成其他的版本的配置。

例如: A.jar依赖B.jar,B.jar依赖C-snap.jar。这个时候我觉得C-snap-2.0.jar不太稳定或者想换成其他同类型的包，可以如下配置：

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
		  <exclusions>
            <exclusion>
              <groupId>C</groupId>
	          <artifactId>C-snap</artifactId>
            </exclusion>
          </exclusions>
		  <scope>test</scope>
		</dependency>
        <dependency>
		  <groupId>C</groupId>
		  <artifactId>C-snap</artifactId>
          <version>3.0</version>
		</dependency>
      </dependencies>
	</project>

    注意：在exclusion里不用注明version，通过groupId和artifactId可以唯一确定坐标。


> A ->  B -X> C(version ?)
> 
> |
> 
>    `  -> C(version: 3.0)


#### 4.8.2 归类依赖 ####
一些库的版本号可能是相同的，可以抽取出常量来控制。(**properties**里定义，如下**sp.version**)

	<project xmlns="http://maven.apache.org/POM/4.0.0"
	  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
	  http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	  <groupId>com.mvn</groupId>
	  <artifactId>hello-world</artifactId>
	  <version>0.0.1-SNAPSHOT</version>
	  <name>Maven hello world project</name>
	  
	  <properties>
		<sp.version>2.5</sp.version>
	  </properties>
	  
	  <dependencies>
		<dependency>
		  <groupId>org.sp</groupId>
		  <artifactId>sp-core</artifactId>
		  <version>${sp.version}</version>
		</dependency>
		<dependency>
		  <groupId>org.sp</groupId>
		  <artifactId>sp-beans</artifactId>
		  <version>${sp.version}</version>
		</dependency>
	  </dependencies>
	</project>

#### 4.8.3 优化依赖 ####

    mvn dependency:list 列出所有解析过的jar包
    mvn dependency:tree 列出所有解析过的jar包树
    mvn dependency:analyze 分析项目依赖

    D:\hello-world> mvn dependency:list
    [INFO] Scanning for projects...
    [INFO]
    [INFO] ------------------------------------------------------------------------
    [INFO] Building Maven hello world project 0.0.1-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO]
    [INFO] --- maven-dependency-plugin:2.8:list (default-cli) @ hello-world ---
    [INFO]
    [INFO] The following files have been resolved:
    [INFO]    junit:junit:jar:4.7:test
    [INFO]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 0.713 s
    [INFO] Finished at: 2016-05-26T16:30:09+08:00
    [INFO] Final Memory: 12M/155M
    [INFO] ------------------------------------------------------------------------

    D:\hello-world> mvn dependency:tree
    [INFO] Scanning for projects...
    [INFO]
    [INFO] ------------------------------------------------------------------------
    [INFO] Building Maven hello world project 0.0.1-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO]
    [INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ hello-world ---
    [INFO] com.mvn:hello-world:jar:0.0.1-SNAPSHOT
    [INFO] \- junit:junit:jar:4.7:test
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 0.742 s
    [INFO] Finished at: 2016-05-26T16:26:54+08:00
    [INFO] Final Memory: 12M/155M
    [INFO] ------------------------------------------------------------------------

