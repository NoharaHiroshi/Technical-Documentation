# Maven介绍 #

## Maven是什么？ ##

Maven是基于项目对象模型(POM project object model)，可以通过**一小段描述信息（配置）**来管理项目的构建，报告和文档的**软件项目管理工具**

可以理解到，Maven是通过配置文件的方式来管理项目。

## Maven有什么作用？ ##

假如你正在Eclipse下开发两个Java项目，姑且把它们称为A、B，其中A项目中的一些功能依赖于B项目中的某些类，那么如何维系这种依赖关系的呢？

很简单，这不就是跟我们之前写程序时一样吗，需要用哪个项目中的哪些类，也就是用别人写好了的功能代码，导入jar包即可。所以这里也如此，可以将B项目打成jar包，然后在A项目的Library下导入B的jar文件，这样，A项目就可以调用B项目中的某些类了。

这样做几种缺陷

1、如果在开发过程中，发现B中的bug，则必须将B项目修改好，并重新将B打包并对A项目进行重编译操作

2、在完成A项目的开发后，为了保证A的正常运行，就需要依赖B(就像在使用某个jar包时必须依赖另外一个jar一样)，两种解决方案，第一种，选择将B打包入A中，第二种，将B也发布出去，等别人需要用A时，告诉开发者，想要用A就必须在导入Bjar包。两个都很麻烦，前者可能造成资源的浪费(比如，开发者可能正在开发依赖B的其它项目，B已经存储到本地了，在导入A的jar包的话，就有了两个B的jar)，后者是我们常遇到的，找各种jar包，非常麻烦。

Maven的目的就是为了解决复杂的依赖问题，通过配置的方式合理叙述项目间的依赖关系。

## Maven怎么使用？ ##

通俗点讲，就是通过pom.xml文件的配置获取jar包，而不用手动去添加jar包。

怎么通过pom.xml的配置就可以获取到jar包呢？pom.xml配置文件从何而来？等等类似问题我们需要搞清楚。

如果需要使用pom.xml来获取jar包，那么首先该项目就必须为maven项目，maven项目可以这样去想，就是在java项目和web项目的上面包裹了一层maven，本质上java项目还是java项目，web项目还是web项目，但是包裹了maven之后，就可以使用maven提供的一些功能了(通过pom.xml添加jar包)。

这里有一个pom.xml的简单例子，可以先了解一下。

	<!-- 依赖列表 -->
    <dependencies>
        <!-- 依赖项 -->
        <dependency>
            <!-- 包名 -->
            <groupId>org.springframework.boot</groupId>
            <!-- 项目名 -->
            <artifactId>spring-boot-starter-data-jpa</artifactId>
            <!-- 版本号 -->
			<version>1.3.2</version>
            <!-- 通常通过包名，项目名，版本号可以定位一个jar包 -->
        </dependency>
	</dependencies>

通过groupId，artifactId，version就可以定位一个jar包，那么如何获取jar包呢？这就又涉及到了仓库的概念。

maven根据配置来从仓库中获取jar包，仓库分为：本地仓库，第三方仓库（私服），中央仓库。

1. 本地仓库：Maven会将工程中依赖的构件(Jar包)从远程下载到本机一个目录下管理，每个电脑默认的仓库是在 $user.home/.m2/repository下，一般我们会修改本地仓库位置，自己创建一个文件夹，在从网上下载一个拥有相对完整的所有jar包的结合，都丢到本地仓库中，然后每次写项目，直接从本地仓库里拿就行了。修改本地库位置：在$MAVEN_HOME/conf/setting.xml文件中修改。

2. 第三方仓库，又称为内部中心仓库，也称为私服：一般是由公司自己设立的，只为本公司内部共享使用。它既可以作为公司内部构件协作和存档，也可作为公用类库镜像缓存，减少在外部访问和下载的频率。（使用私服为了减少对中央仓库的访问，私服可以使用的是局域网，中央仓库必须使用外网，也就是一般公司都会创建这种第三方仓库，保证项目开发时，项目所需用的jar都从该仓库中拿，每个人的版本就都一样。注意：连接私服，需要单独配置。如果没有配置私服，默认不使用。

3. Maven内置了远程公用仓库：http://repo1.maven.org/maven2，这个公共仓库是由Maven自己维护，里面有大量的常用类库，并包含了世界上大部分流行的开源项目构件。目前是以java为主，工程依赖的jar包如果本地仓库没有，默认从中央仓库下载。

## maven java项目结构目录 ##

	simple
		---pom.xml　　　　核心配置，项目根下
		---src
			---main　　　　　　
				---java　　　　java源码目录
	　　      　　	---resources　  java配置文件目录
	　　　　　　　---test
	　　　　　　　　　	---java　　　　测试源码目录
	　　　　　　　　　	---resources　  测试配置目录
	target 		编译目录，src/main/java下的源代码就会编译成.class文件放入target目录中

## maven web项目结构目录 ##

	pom.xml                 核心配置
	src/main/java                java源码
	src/main/resources            java配置
	src/main/webapp         web项目中 WebRoot目录
		|-- WEB-INF
       	|-- web.xml
	src/test                测试
	target                  输出目录


## maven命令操作 ##

	编译：mvn compile　　--src/main/java目录java源码编译生成class （target目录下）
	
	测试：mvn test　　　　--src/test/java 目录编译
	
	清理：mvn clean　　　 --删除target目录，也就是将class文件等删除
	
	打包：mvn package　　--生成压缩文件：java项目#jar包；web项目#war包，也是放在target目录下
	
	安装：mvn install　　　--将压缩文件(jar或者war)上传到本地仓库
	
	部署|发布：mvn deploy　　--将压缩文件上传私服