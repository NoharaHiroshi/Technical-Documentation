# 20190114-Spring Boot中返回Json格式的问题 #

## 问题背景描述 ##

Json不都是键值对吗？为什么Spring boot可以直接返回字符串？

	// @RestController返回json字符串
	@RestController
	@SpringBootApplication
	@RequestMapping(value="/user")
	public class userController {
	   	@RequestMapping(value = "/helloUser", method = RequestMethod.GET)
	    public String index(@RequestParam(value = "name", defaultValue = "World") String name) {
	        return "hello " + name;
	    }	
	}	


## 解决问题方案 ##

看来是我理解错了，要回答这个问题，首先要从@RestController注解的含义

**@Controller和@RestController的区别**

在早期没有使用前后端分离的开发方法时，由后端直接渲染页面，请求url返回的结果都是html或者jsp页面，并且跳转到相应界面。

	@Controller
	@RequestMapping(value="/user")
	public class userController {
	
		//跳转到上传文件的页面
		@RequestMapping(value="/helloUser", method = RequestMethod.GET)
		public String index() {
		//跳转到 templates 目录下的 index.html
		return "index";
	}

如果我需要使用异步方式从后台更新数据怎么办呢？则需要在对应的方法上加上@ResponseBody注解，用于返回Json，Xml等格式的数据。

	@Controller
	@RequestMapping(value="/user")
	public class userController {
		@RequestMapping(value = "/testJson", method = RequestMethod.GET)
	    public @ResponseBody User testJson(){
	        User user = new User();
	        user.setName("Lands");
	        user.setId(1);
	        return user;
	    }
	}

	返回数据
	Response Headers
		Content-Type:application/json;charset=UTF-8
	{"id":1,"name":"Lands"}

到了后面，又发明了@RestController注解，它可以原封不动的返回内容


	@RestController
	@RequestMapping(value="/user")
	public class userController {	

		@RequestMapping(value = "/helloUser", method = RequestMethod.GET)
	    public String index(@RequestParam(value = "name", defaultValue = "World") String name) {
	        User user = new User();
	        user.setName(name);
	        user.setId(1);
	        return "success";
	    }
	}

	返回数据
	Response Headers
		Content-Type:text/html;charset=UTF-8
	success

但这并不是json字符串，json字符串有严格的标准，这返回的只是普通的字符串。而且通过content-type也能获知这并不是json字符串。

**那使用@RestController如何返回Json字符串呢？**

第一种：直接返回实例对象，java会自动将实例对象转化为json字符串。但这种方法实用性不高，很少有接口直接返回对象的。

	@RequestMapping(value = "/helloUser", method = RequestMethod.GET)
	    public User index(@RequestParam(value = "name", defaultValue = "World") String name) {
	        User user = new User();
	        user.setName(name);
	        user.setId(1);
	        return user;
	    }


	import com.google.gson.Gson;

	@RequestMapping(value = "/helloUser", method = RequestMethod.GET)
    public String index(@RequestParam(value = "name", defaultValue = "World") String name) {
        User user = new User();
        user.setName(name);
        user.setId(1);
        Gson gson = new Gson();
        return gson.toJson(user); // 解析json，gson.fromJson()
    }