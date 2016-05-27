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
