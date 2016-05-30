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


## 第五章 仓库 ##
### 5.1 何为maven仓库 ###
任何一个依赖，插件或者项目构建的输出，都可以称为构件。例如依赖库log4j-1.2.15.jar是一个构件，插件maven-compiler-plugin-2.0.2.jar是一个构件，项目构建完成后的输出hello-world-1.0.0-SNAPSHOT.jar也是一个构件。

maven在某个位置统一的存储所有的maven项目共享的构件，这个位置就是仓库。

### 5.2 仓库的布局 ###
根据坐标定义构件在仓库的位置。

构件对应仓库的路径：groupId/artifactId/version/artifactId-version.packageing


例如有如下jar定义：

    <groupId>junit</groupId>
	<artifactId>junit</artifactId>
	<version>4.7</version>
    <packaging>jar</packageing>

    路径： \junit\junit\4.7\junit-4.7.jar

### 5.3 仓库的分类 ###

- 本地仓库: 本地存储
- 远程仓库: maven自带的默认中心仓库，包含了绝大多数开源构件。
- 其他特殊远程仓库: 
    - 私服(局域网内架设)
    - Java.net Maven库(https://maven2-repository.java.net/)
    - JBoss Maven库(http://repository.jboss.com/maven2)

寻找构件的时候先在本地仓库查看，如果存在，直接使用；如果不存在，则去远程仓库查看，发现后下载到本地仓库使用，如果也不存在，则maven会报错。



#### 5.3.1 本地仓库 ####
执行第一条maven命令后才会创建本地仓库，默认会将~home\.m2\repository目录作为本地仓库。

如需修改仓库路径，可以修改settings.xml文件，如果只针对当前用户修改，可以把maven安装目录下的文件拷到 .m2 文件夹下再修改。

    <settings>
    <localRepository>D:\apache-maven-3.3.9\repo</localRepository>
    </settings>

一个项目只有进入本地仓库后，才能给其他maven项目使用，那么如何进入呢：

    mvn clean install   将项目的输出构件安装到本地仓库

#### 5.3.2 远程仓库 ####
其他提供构件的仓库。可以有多个。

#### 5.3.3 中央仓库 ####
maven的默认远程仓库。在maven-model-builder-3.3.9.jar中定义。

    <repositories>
      <repository>
      <id>central</id> 
      <name>Central Repository</name> 
      <url>https://repo.maven.apache.org/maven2</url> 
      <layout>default</layout> 
      <snapshots>
      <enabled>false</enabled> 
      </snapshots>
      </repository>
    </repositories>


并且这个pom也是其他所有pom继承的超级pom。后续会讲解。

#### 5.3.4 私服 ####
一种特殊的远程仓库，架设在局域网内，代理广域网上的远程仓库，供局域网内用户使用。

maven用户会去私服下载构件，如果私服上不存在，私服会去远程仓库获取，缓存到私服，提供给用户使用。

好处：

- 节省带宽
- 加速maven构建
- 部署第三方构件
- 稳定，可控
- 降低中央仓库负荷

### 5.4 远程仓库配置 ###
很多情况下，默认的中央仓库无法满足要求，需要在pom中配置其他远程仓库。通过repository元素

	<repositories>
		<repository>
			<id>jboss</id>
			<name>JBoss Repository</name>
			<url>http://repository.jboss.com/maven2</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
			<layout>default</layout>
		</repository>
	</repositories>

    releases: 设为true,表示下载发布版本
    snapshots: 设为false，表示不下载快照版本

对于releases和snapshots还有其他2个元素：

- updatePolicy: 检查更新的频率，默认daily。 其他有never， always， interval:X(每隔X分钟)
- checksumPolicy: 校验和验证，默认为warn。 其他有fail， ignore

#### 5.4.1 远程仓库认证 ####
大部分远程仓库无须认证就可以访问，有时候出于安全考虑，需要认证才可以访问。这就需在settings文件中配置。

    <servers>
    <server>
      <id>deploymentRepo</id>
      <username>repouser</username>
      <password>repopwd</password>
    </server>
    </servers>

    注：id 必须与POM中需要认证的repository的id一致。

#### 5.4.2 部署至远程仓库 ####
将项目生成的构件部署至远程仓库，供其他团队使用。

配置如下：

	<distributionManagement>
		<repository>
			<id>proj-release</id>
			<name>proj release repository</name>
			<url>http://192.168.1.99/content/repositories/proj-release</url>
		</repository>
		<snapshotRepository>
			<id>proj-snapshots</id>
			<name>proj snapshot repository</name>
			<url>http://192.168.1.99/content/repositories/proj-snapshots</url>
		</snapshotRepository>
	</distributionManagement>

    注： repository表示发布版本的仓库
        snapshotRepository表示快照版本的仓库

    执行 mvn clean deploy 命令即可。

### 5.5 快照版本 ###
maven分 发布版和快照版。

- 发布版: (如1.2)即稳定版本，可以提供给外部使用
- 快照版：(如1.2-SNAPSHOT)会不停发的更新，快照版发布时会自动加上时间戳供其他团队依赖使用。maven会自动根据时间戳的变化下载新版本到其本地，免去了手动更新的麻烦。一旦测试完善了，去掉SNAPSHOT即可变成文档版本。默认情况，maven每天检查一次更新，也可以强制检查更新： mvn clean install -U

### 5.6 从仓库解析依赖的机制 ###

1. 依赖范围是system的时候，直接从本地系统解析构件
2. 根据依赖坐标算出路径后，从本地仓库查找，如果发现，则解析成功。
3. 在本地仓库不存在的情况下，如果依赖的版本是显示的发布版本，则遍历所有远程仓库，发现后下载解析使用。
4. 如果依赖的版本是SNAPSHOT，基于更新策略取快照的最新版本，下载解析使用。

### 5.7 镜像 ###
如果仓库X可以提供仓库Y存储的所有内容，就可以认为X是Y的一个镜像。使用镜像一般是出于地理位置考虑，比如用国内的镜像可以比国外直接提供的中央仓库下载速度更快。一般会结合私服使用。

在settings.xml文件配置：

    <mirrors>
    <mirror>
      <id>CN</id>  
      <name>OSChina Central</name>
      <url>http://maven.oschina.net/content/groups/public/</url>  
      <mirrorOf>central</mirrorOf> 
    </mirror>
    </mirrors>

    mirrorOf为central，表示该配置是中央仓库的镜像


结合私服：

    <mirrors>
    <mirror>
      <id>internal-repository</id>  
      <name>internal repository manager</name>
      <url>http://10.0.0.100/maven2</url>  
      <mirrorOf>*</mirrorOf> 
    </mirror>
    </mirrors>

    mirrorOf为*，表示对于所有远程仓库的请求都会转至http://10.0.0.100/maven2私服。


### 5.8 仓库搜索服务 ###

常用的一些搜索服务:

- https://repository.sonatype.org/
- http://mvnrepository.com/
 

## 第六章 生命周期和插件 ##
### 6.1 何为生命周期 ###
Maven的生命周期就是为了对所有的构建过程进行抽象和统一。而且其本身不做任何实际工作，实际任务都由插件来完成。

Maven从大量项目和构建工具中学习和反思，总结了一套高度完善，易扩展的生命周期，包含清理，初始化，编译，测试，打包，集成测试，验证，部署，站点生成等几乎所有构建步骤。


### 6.2 生命周期详解 ###
#### 6.2.1 三套生命周期 ####

- clean: 清理项目
- default: 构建项目
- site: 建立项目站点

三套生命周期相互独立。

每个生命周期包含一些阶段，这些阶段是有顺序的，后面的阶段依赖前面的。以clean为例，它包含pre-clean,clean和post-clean。当用户调用pre-clean时，只有pre-clean阶段执行；当用户调用clean时，pre-clean和clean阶段都会执行；当用户调用post-clean时，三个阶段都会执行。

#### 6.2.2 clean生命周期 ####
1. pre-clean: 执行一些清理前需要完成的工作
2. clean: 清理上一次构建生成的文件
3. post-clean: 执行一些清理后需要完成的工作

#### 6.2.3 default生命周期 ####
核心部分。讲解部分重要的阶段。

1. validate
2. initialize
3. generate-sources
4. process-sources: 处理项目主资源文件，一般是将src/main/resources目录下的内容处理后复制到classpath目录
5. generate-resources
6. process-resources
7. compile: 编译项目主源码，编译src/main/java下的java文件到classpath目录
8. process-classes
9. generate-test-sources
10. process-test-sources: 处理项目测试资源文件，
11. generate-test-resources
12. process-test-resources
13. test-compile: 编译项目的测试代码，编译src/test/java下的java文件到classpath目录
14. process-test-classes
15. test: 使用单元测试框架运行测试，测试代码不会打包和部署
16. prepare-package
17. package: 打包，如jar等
18. pre-integration-test
19. integration-test
20. post-integration-test
21. verify
22. install: 将包安装到maven本地仓库，供其他项目使用
23. deploy: 将最终包发布到远程仓库

#### 6.2.4 site生命周期 ####

1. pre-site: 执行一些在生成项目站点之前需要完成的工作
2. site: 生成项目站点文档
3. post-site: 行一些在生成项目站点之后需要完成的工作
4. site-deploy: 将生成的项目站点发布到服务器

#### 6.2.5 命令行与生命周期 ####
maven会自动执行命令所属阶段之前的所有阶段。

    mvn clean: 执行pre-clean和clean
    mvn test: 执行validate到test的所有阶段
    mvn clean install: 执行pre-clean和clean， 再执行validate到install的所有阶段

#### 6.3 插件目标  ####
插件目标(plugin goal):对于插件本身，为了复用代码，一个插件往往提供多个任务。每一个功能就是一个插件目标。

比如 dependency:analyze, dependency:tree 和 dependency:list等。

冒号前面的是插件前缀，后面的是插件的目标。

#### 6.4 插件绑定 ####
比如 compile 阶段与 maven-compiler-plugin 这一插件绑定，用来提供编译功能。

#### 6.4.1 内置绑定 ####
为了让用户几乎不用配置就能构建maven项目，maven核心为一些主要生命周期绑定了很多插件。

- clean: 绑定maven-clean-plugin插件

        pre-clean: -
        clean: maven-clean-plugin:clean
        post-clean: -

- site: 绑定maven-site-plugin插件

        pre-site: -
        site: maven-site-plugin:clean
        post-site: -
        site-deploy: maven-site-plugin:deploy

- default: 绑定 maven-resources-plugin, maven-compiler-plugin等

        compile: maven-compiler-plugin:compile
        test: maven-surefire-plugin:test
        jar: maven-jar-plugin:jar
        等...

#### 6.4.2 自定义绑定 ####
下面的配置: 在defau生命周期的verify阶段执行maven-soure-plugin插件的jar-no-fork任务，创建源码jar包。创建一个以-sources.jar结尾的源码jar包。一般插件也会配置一个默认阶段，如果 phase 不设置，就用插件的默认阶段。

    <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-soure-plugin</artifactId>
        <version>2.1.1</version>
		<executions>
	      <execution>
	      	 <id>attach-sources</id>
			 <phase>verify</phase>
			 <goals>
				<goal>jar-no-fork</goal>
			 </goals>
			</execution>
        </executions>
	  </plugin>
    </plugins>
    </build>

    execution: 配合执行任务
    phase: 绑定阶段
    goal: 插件目标

如果多个插件绑定到同一个阶段，则按插件声明顺序执行。

### 6.5 插件配置 ###
可以进一步配置插件目标的参数。

#### 6.5.1 命令行插件配置 ####
在命令行模式下，使用-D参数，后面加key=value的形式。

例如: maven-surefire-plugin提供了一个 maven.test.skip参数，当其值为true的时候，跳过执行测试。

    mvn install -D maven.test.skip=true

参数-D是java自带的，通过命令行设置一个java系统属性。maven简单的重用了这个参数。

#### 6.5.2 POM中插件全局配置 ####
声明插件的时候全局配置，对各个阶段都有效。

    <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>2.1</version>
		<configuration>
			<source>1.6</source>
			<target>1.6</target>
		</configuration>
	  </plugin>
    </plugins>
    </build>

#### 6.5.3 POM中插件任务配置 ####
除了可以为插件配置全局参数，还可以为某个插件任务配置特定参数。

在 execution 中配置configuration。

    <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-antrun-plugin</artifactId>
        <version>1.3</version>
		<executions>
	      <execution>
	      	 <id>ant-validate</id>
			 <phase>validate</phase>
			 <goals>
				<goal>run</goal>
			 </goals>
			 <configuration>
				<tasks>
					<echo>I'm bound to validate phase!</echo>
				</tasks>
			 </configuration>
			</execution>
			<execution>
	      	 <id>ant-verify</id>
			 <phase>verify</phase>
			 <goals>
				<goal>run</goal>
			 </goals>
			 <configuration>
				<tasks>
					<echo>I'm bound to verify phase!</echo>
				</tasks>
			 </configuration>
			</execution>
        </executions>
	  </plugin>
    </plugins>
    </build>

### 6.6 获取插件信息 ###
#### 6.6.1 在线插件信息 ####

- http://maven.apache.org/plugins/index.html

#### 6.6.2 使用maven-help-plugin描述插件 ####

    mvn help:describe -D plugin=compiler

### 6.7 从命令行调用插件 ###
    mvn 目标前缀:目标

    mvn dependency:tree

### 6.8 插件解析机制 ###
#### 6.8.1 插件仓库 ####
跟普通的构件依赖类似，先从本地仓库查找，没有的话去远程仓库下载到本地仓库使用。

插件的配置方式与普通构件也一样，只是标签名字不同。用pluginRepositories和pluginRepository来声明。

	<pluginRepositories>
		<pluginRepository>
			<id>central</id>
			<name>Maven Plugin Repository</name>
			<url>http://repo1.maven.org/maven2</url>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
			<releases>
				<updatePolicy>never</updatePolicy>
			</releases>
		</pluginRepository>
	</pluginRepositories>

#### 6.8.2 插件的默认 groupId ####
如果插件是maven的官方插件，可以省略groupId，解析时自动用 org.apache.maven.plugins补齐，一般不推荐用。

#### 6.8.3 解析插件版本 ####
如果插件没有指明版本，则maven会去超级POM中查询对应的版本，如果有，则使用；如果没有，则在本地库里查询最新 release 版本使用。

#### 6.8.4 解析插件前缀 ####
在插件的元数据(maven-metadata.xml)中定义。

    <metadata>
      <plugins>
        <plugin>
          <name>Maven Clean Plugin</name>
          <prefix>clean</prefix>
          <artifactId>maven-clean-plugin</artifactId>
        </plugin>
        <plugin>
          <name>Maven compiler Plugin</name>
          <prefix>compiler</prefix>
          <artifactId>maven-compiler-plugin</artifactId>
        </plugin>
      </plugins>
    </metadata>


## 第七章 聚合与继承 ##
### 7.1 聚合 ###
将多个模块构建成一个项目。通过module声明。主POM的packaging必须声明为pom。

	<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
	http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	  <groupId>com.mvn</groupId>
	  <artifactId>hello-world</artifactId>
	  <version>0.0.1-SNAPSHOT</version>
	  <packaging>pom</packaging>
	  <name>Maven hello world project</name>
	  <module>hello-base</module>
	  <module>hello-system</module>
	</project>

目录结构
	
	hello-world
		|--hello-base
		|	|-- src
		|	|-- pom.xml
		|--hello-system
		|	|-- src
		|	|-- pom.xml
		|-- pom.xml

### 7.2 继承 ###
多个module之间会有相同的库或者插件定义,抽取重复的配置,就是 pom 的继承。

可以创建POM的父子结构,在父POM中声明一些配置供子POM继承。

以7.1为基础,创建hello-parent子目录,在其下创建父POM。

	<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
	http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	  <groupId>com.mvn</groupId>
	  <artifactId>hello-parent</artifactId>
	  <version>0.0.1-SNAPSHOT</version>
	  <packaging>pom</packaging>
	  <name>Maven hello world parent</name>
	</project>

修改hello-base模块:

	<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
	http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	  <parent>
		<groupId>com.mvn</groupId>
	    <artifactId>hello-parent</artifactId>
	    <version>0.0.1-SNAPSHOT</version>
		<relativePath>../hello-parent/pom.xml</relativePath>
	  </parent>
	  <artifactId>hello-base</artifactId>
	  <name>Maven hello base</name>
	</project>

	注: mavne 默认识别的 relativePath 是其上一层目录

修改hello-system模块: 同上

最后修改hello-world主POM:

	<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
	http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	  <groupId>com.mvn</groupId>
	  <artifactId>hello-world</artifactId>
	  <version>0.0.1-SNAPSHOT</version>
	  <packaging>pom</packaging>
	  <name>Maven hello world project</name>
	  <module>hello-parent</module>
	  <module>hello-base</module>
	  <module>hello-system</module>
	</project>


有很多可继承的元素: groupId, version, url, dependencies 等

### 7.3 依赖的继承 ###
Maven提供了dependencyManagement元素来让子模块继承父模块的依赖配置。在其元素下的依赖声明不会引入实际的依赖,仅仅约束dependencies下的依赖使用。

    <project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
	     http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.xharvard</groupId>
	<artifactId>hello-world</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>pom</packaging>

	<name>hello-world</name>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<springframework.version>2.5.6</springframework.version>
		<junit.version>4.7</junit.version>
	</properties>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-core</artifactId>
				<version>${springframework.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-beans</artifactId>
				<version>${springframework.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>
				<version>${springframework.version}</version>
			</dependency>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>${junit.version}</version>
				<scope>test</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
    </project>

	dependencyManagement只是声明, 不会真的引入。

对应的子模块可以选择性的引入需要的依赖即可。

	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
		</dependency>
	</dependencies>

	注: 不需要写 version 属性了, 自动应用父POM定义的版本。


### 7.4 插件管理 ###
类似的, maven提供pluginManagement元素用来声明插件。

### 7.5 聚合与继承的关系 ###

- 聚合: 方便快速构建项目
- 继承: 消除重复配置

通常情况, 会将两者配置在同一个POM中。既是父POM, 又是提供聚合的父模块。

### 7.6 约定优于配置 ###
mavne的核心理念。

例如: maven定义的一些约定如下, 具体可以查看超级POM.xml文件的定义。

- 源码: src/main/java/
- 编译输出: target/classes/
- 打包方式: jar
- 包输出目录: target/

当然, 这些约定也是可以修改的, 但是不推荐。 容易给其他开发者带来混乱。

	例如修改源码路径:
	<build>
		<sourceDirectory>src/java</sourceDirectory>
	</build>

### 7.7 反应堆 ###
在一个多模块的项目中, 反应堆是指所有模块组成的一个构建结构。反应堆包含了个模块之间的继承和依赖关系, 从而能够自动计算出合理的模块构建顺序。

#### 7.7.1 反应堆的构建顺序 ####
maven读取POM, 如果该POM没有依赖模块, 那么就构建该模块; 否则就先构建其依赖模块, 如果该依赖还依赖于其他模块, 则进一步先构建依赖的依赖模块。

模块间的依赖关系将反应堆构成一个有向非循环图(DGA), 如果出现循环, maven会报错。

#### 7.7.2 剪裁反应堆 ####
构建反应堆中的某些模块。

maven提供了很多命令支持: (mvn -h 查看参数)

	-am, --also-make 同时构建所以模块的依赖模块
	-amd -also-make-dependents 同时构建依赖于所列模块的模块
	-pl, --projects <arg> 构建指定的模块, 模块间用逗号分隔
	-rf -resume-from <arg> 从指定模块回复反应堆

	mvn clean install -pl hello-base
	mvn clean install -rf hello-system

## 第八章 使用Nexus创建私服 ##



