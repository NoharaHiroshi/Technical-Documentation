# Spring Boot 切换运行环境 #

在开发时，往往面临开发环境、测试环境、生产环境的切换。Spring Boot提供了简单的方法以供切换。

Spring Boot启动时会默认加载application.properties中的配置内容。 如果想修改启动时默认加载的配置文件，可以在启动命令后加上：

	# dev为环境名称
	-Dspring.profiles.active=dev

然后在application.properties 同级目录下新建文件application-dev.properties。

这样之前启动命令就可以通过指定的环境名称（dev）找到对应得application-dev.properties配置文件。

	# test为环境名称
	-Dspring.profiles.active=test

对应的配置文件就是application-test.properties