Spring Boot IDEA设置热部署

**使用spring-boot-devtools实现热部署**

在pom中直接引入依赖

	<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
	</dependency>
	 
设置以下两项（第一项如已设置直接设置第二项）

>1、 “File” -“Settings” -“Build,Execution,Deplyment” >-“Compiler”，选中打勾 “Build project automatically” 。

>2、 组合键：“Shift+Ctrl+Alt+/” ，选择 “Registry” ，选中打勾 “compiler.automake.allow.when.app.running” 。

之后直接正常run即可！

**使用spring-loaded实现热部署**

在Plugins中添加依赖

	<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <dependencies>
                    <!-- spring热部署 -->
                    <dependency>
                        <groupId>org.springframework</groupId>
                        <artifactId>springloaded</artifactId>
                        <version>1.2.6.RELEASE</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>

命令行窗口启动

找到pom.xml的路径，在这个路径下打开cmd窗口（win下可以通过shift快速在对应路径打开），输入启动命令

	mvn spring-boot:run

这样就可以在IDE里修改代码实现热加载了！

