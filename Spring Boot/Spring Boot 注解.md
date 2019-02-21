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