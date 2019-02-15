# SpringBoot 整合 Mybatis 启动流程 #

> 这篇记录是我在初次整合SpringBoot 和 Mybatis 并成功实现MVC和对象持久化时写下的，用作备忘。

默认创建spring-boot项目后，会在resources目录下生成一个空的application.properties配置文件。

SpringBoot启动时，会默认加载这个配置文件application.properties，其中包含系统属性、环境变量、命令参数这些信息。这些配置信息不一定要全部都写在application.properties中，可以在application.properties里面自定义配置文件的名称和位置。（但是无论怎么配置，spring-boot都会读取加载application.properties文件）

**首先在application.properties中配置mysql和mybatis**

	# mysql配置
	spring.datasource.url =jdbc:mysql://localhost:3306/spring_boot_sql?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true
	spring.datasource.username =root
	spring.datasource.password =123456
	spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
	
	# mybatis配置
	# 别名包位置
	mybatis.type-aliases-package=com.example.demo.model
	# 实体类的映射文件位置
	mybatis.mapper-locations=classpath:mapping/*.xml

**其次在SpringBootApplication中加入@MapperScan("com.example.demo")，扫描mapper层。**

> org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userController': Injection of resource dependencies failed;
> 
> 出现这个问题，可能是mapper层没有被扫描到

我是使用的org.mybatis.generator自动生成的映射关系。在数据库中建立好表之后，运行脚本。自动生成Model，Mapper，Mapper.xml。

**准备工作完成。**

1、创建Service。

	package com.example.demo.service;
	
	import org.springframework.stereotype.Service;
	
	import javax.annotation.Resource;
	
	import com.example.demo.mapper.UserMapper;
	import com.example.demo.model.User;
	
	@Service("userService")
	public class UserService {
	
	    @Resource
	    UserMapper userMapper;
	
	    public String addNewUser(User user){
	        int result = userMapper.insert(user);
	        System.out.println(result);
	        return "addNewUser Success";
	    }
	
	    public User[] getAllUser() {
	        User[] userList = userMapper.selectAll();
	        return userList;
	    }
	
	}

2、创建Controller。

	package com.example.demo.controller;

	import java.io.File;
	import java.io.FileInputStream;
	import java.io.OutputStream;
	
	import javax.annotation.Resource;
	
	import org.springframework.boot.autoconfigure.SpringBootApplication;
	import org.springframework.web.bind.annotation.*;
	import org.springframework.web.multipart.MultipartFile;
	import com.google.gson.Gson;
	
	import javax.servlet.http.HttpServletResponse;
	
	import com.example.demo.service.UserService;
	import com.example.demo.model.User;
	
	@RestController
	@SpringBootApplication
	@RequestMapping(value="/user")
	public class UserController {
	
	    @Resource
	    UserService userService;
	
		@RequestMapping(value = "/addUser", method = RequestMethod.POST)
		@RequestParam
		public String addUser(@RequestBody User user){
		    System.out.println(user);
		    System.out.println(user.getId());
		    System.out.println(user.getName());
		    try {
		        userService.addNewUser(user);
		    } catch (Exception e) {
		        e.printStackTrace();
		        return "error";
		    }
		    return "success";
		}

之后运行main，即可正常运行。