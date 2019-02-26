# 20190225-Spring Boot @component下的依赖注入 #

## 问题背景描述 ##

最近在学习使用SpringBoot，正常的MVC（Controller层、Service层）下都可以使用@Resource或者@Autowired注解依赖注入，但是把这个依赖注入用于写的Util类/WebSocket类中，发现引入失败，解决一下这个问题。

## 解决问题方案 ##

	// 创建一个WebSocket站点
	@ServerEndpoint(value = "/webSocket/{userId}")
	@Component
	public class WebSocket {
	    // 日志
	    private static final Logger LOG = LoggerFactory.getLogger(WebSocket.class);
	
	    // 线上用户数量
	    private static Integer connectCount = 0;
	
	    // 存放每个连接对象的session
	    private static CopyOnWriteArraySet<WebSocket> webSocketSet = new CopyOnWriteArraySet<WebSocket>();
	
	    // 会话实例
	    private Session session;
	
	    // 用户ID
	    private String userId;

		@Resource
		private UserService userService; // null

运行时报错

	java.lang.reflect.InvocationTargetException: null

Springboot 的 websocket 里面使用 @Autowired 注入 service 或 bean 时，报空指针异常，service 为 null（并不是不能被注入）。

**解决方法**：将要注入的 service 改成 static，就不会为null了。

	// 创建一个WebSocket站点
	@ServerEndpoint(value = "/webSocket/{userId}")
	@Component
	public class WebSocket {
	    // 日志
	    private static final Logger LOG = LoggerFactory.getLogger(WebSocket.class);
	
	    // 线上用户数量
	    private static Integer connectCount = 0;
	
	    // 存放每个连接对象的session
	    private static CopyOnWriteArraySet<WebSocket> webSocketSet = new CopyOnWriteArraySet<WebSocket>();
	
	    // 会话实例
	    private Session session;
	
	    // 用户ID
	    private String userId;
	
	    private static UserService userService;
	
	    @Autowired
	    public void setUserService(UserService userService) {
	        WebSocket.userService = userService;
	    }


**本质原因**：spring管理的都是单例（singleton），和 websocket （多对象）相冲突。

**详细解释**：项目启动时初始化，会初始化 websocket （非用户连接的），spring 同时会为其注入 service，该对象的 service 不是 null，被成功注入。但是，由于 spring 默认管理的是单例，所以只会注入一次 service。当新用户进入聊天时，系统又会创建一个新的 websocket 对象，这时矛盾出现了：spring 管理的都是单例，不会给第二个 websocket 对象注入 service，所以导致只要是用户连接创建的 websocket 对象，都不能再注入了。

像 controller 里面有 service， service 里面有 dao。因为 controller，service ，dao 都有是单例，所以注入时不会报 null。但是 websocket 不是单例，所以使用spring注入一次后，后面的对象就不会再注入了，会报null。
