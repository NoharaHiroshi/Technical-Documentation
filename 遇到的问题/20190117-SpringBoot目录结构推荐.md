# 20190117-Spring Boot 目录结构推荐 #

## 问题背景描述 ##

刚开始使用SpringBoot，十分茫然，不知道会有哪些目录，哪些内容，它们分别代表什么，又有什么样的作用。 因此，梳理一下。

## 解决问题方案 ##

SpringBoot的入口是入口类，是使用了@SpringBootApplication注解的类，在官方文档汇总，**入口类推荐与其他代写的包放在同一级，会自动扫描。**

**一、代码层的结构**

根目录：com.springboot

1、工程启动类(ApplicationServer.java)置于com.springboot.build包下

2、实体类(domain)置于com.springboot.domain

3、数据访问层(Dao)置于com.springboot.repository

4、数据服务层(Service)置于com,springboot.service,数据服务的实现接口(serviceImpl)至于com.springboot.service.impl

5、前端控制器(Controller)置于com.springboot.controller

6、工具类(utils)置于com.springboot.utils

7、常量接口类(constant)置于com.springboot.constant

8、配置信息类(config)置于com.springboot.config

9、数据传输类(vo)置于com.springboot.vo

**二、资源文件的结构**

根目录:src/main/resources

1.配置文件(.properties/.json等)置于config文件夹下

2.国际化(i18n))置于i18n文件夹下

3.spring.xml置于META-INF/spring文件夹下

4.页面以及js/css/image等置于static文件夹下的各自文件下


> 这是我从网上摘抄下来的，结合IDE生成的目录整合一下。

大致上看了下，SpringBoot项目结构的内容大致包含

代码层（体系类）：

-   实体层（Model/Domain/...）
- 	数据访问层/持久化层（Dao/Mapper/...）
- 	数据服务层（Service）
- 	前端控制器（Controller）

代码层（工具类）：

- 工具类（utils）
- 常量接口类（constant
- 配置信息类（config）

资源文件层（src/main/resource）：

