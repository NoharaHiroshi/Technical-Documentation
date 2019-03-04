Spring Boot 注解

**@SpringBootApplication**

@SpringBootApplication： 此注解是组合注解。包含了

- @ComponentScan
- @SpringBootConfiguration
- @EnableAutoConfiguration

@ComponentScan：扫描当前包及其子包下被@Component， @Controller, @Service, @Repository等注解标记的类

@SpringBootConfiguration：继承至@Configuration，会将当前类声明的多个以@Bean注解标记的方法载入到Spring Boot容器中，并且实例名就是方法名。

@EnableAutoConfiguration：Spring Boot自动装配，通过此注解，所有符合自动配置条件的Bean加载到Spring Boot容器。

**@Controller 和 @RestController**

@RestController 是Spring4之后加入的注解，原来在@Controller中返回json需要@ResponseBody来配合，如果直接用@RestController替代@Controller就不需要再配置@ResponseBody，默认返回json格式。

而@Controller是用来创建处理http请求的对象，一般结合@RequestMapping使用。

**@RequestMapping**

一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

- value： 指定请求的实际地址
- method： 指定请求的method类型，GET、POST、PUT、DELETE等
- consumes： 指定请求参数的内容类型（Content-Type），例如application/json, text/html;
- produces: 指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回；
- params： 指定request中必须包含某些参数值时，才让该方法处理。
- headers： 指定request中必须包含某些指定的header值，才能让该方法处理请求。

**@Configuration**

@Configuration常和@Bean搭配使用，@Configuration修饰的类可以作为配置文件注册Bean。
@Configuration中所有带@Bean注解的方法都会被动态代理，调用该方法返回的都是同一个实例。
@Configuration注解本质上还是@Component，因此@ComponentScan可以扫描到@Configuration注解的类。

@Configuration 标记的类必须符合下面的要求：

- 配置类必须以类的形式提供（不能是工厂方法返回的实例），允许通过生成子类在运行时增强（cglib 动态代理）。
- 配置类不能是 final 类（没法动态代理）。
- 配置注解通常为了通过 @Bean 注解生成 Spring 容器管理的类。
- 配置类必须是非本地的（即不能在方法中声明，不能是 private）。
- 任何嵌套配置类都必须声明为static。
- @Bean方法不能创建进一步的配置类（也就是返回的bean如果带有@Configuration，也不会被特殊处理，只会作为普通的bean。

使用XML配置Bean

	<beans>
	 <bean id="b" class="springsimple.B"/>
	 <bean id="a" class="springsimple.A"/>
	</beans>

一行代码配置一个Bean。

使用@Configuration注解配置Bean

	@Configuration
	public Config {
		...	

		@Bean
		public SpringSimple springSimple(){
			return new SpringSimple();
		}		
	}

@Configuration修饰的类就相当于是XML配置文件，@Bean修饰的方法就相当于配置Bean

获取Bean

	// 获取配置类的上下文	
	ApplicationContext context = new AnnotationConfigApplicationContext(Config.class);
	// 获取配置类设置的Bean，如果在配置Bean的时候没有指定Bean的名字，则默认使用Bean修饰的方法名。
	// getBean()返回的是Object，所以需要转换类型
	SpringSimple springSimple = (SpringSimple)context.getBean("springSimple");
	
	
需要注意一点是，在其他类中依赖注入Bean时，不能使用@Resource或者@Autowired。在Controller类，Service类中可以使用@Resource或者@Autowired依赖注入。


