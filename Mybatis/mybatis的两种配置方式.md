# mybatis的两种配置方式 #

首先需要引入mybatis-spring-boot-starer的pom文件

	<dependency>
		<groupId>org.mybatis.spring.boot</groupId>
		<artifactId>mybatis-spring-boot-starter</artifactId>
		<version>1.1.1</version>
	</dependency>

## 无配置文件注解版 ##

1. 添加application.properties相关配置

		mybatis.type-aliases-package=com.example.demo
		
		spring.datasource.driverClassName = com.mysql.jdbc.Driver
		spring.datasource.url = jdbc:mysql://localhost:3306/demo?useUnicode=true&characterEncoding=utf-8
		spring.datasource.username = root
		spring.datasource.password = 123456
	
	springboot会自动加载spring.datasource.*相关配置，数据源就会自动注入到sqlSessionFactory中，sqlSessionFactory会自动注入到Mapper中。

	在启动类中添加对mapper包扫描@MapperScan

		@SpringBootApplication
		@MapperScan("com.example.demo.mapper")
		public class Application {
		
			public static void main(String[] args) {
				SpringApplication.run(Application.class, args);
			}
		}

	或者直接在Mapper类上面添加注解@Mapper

2. 开发Mapper
	
		public interface UserMapper {
		
			@Select("SELECT * FROM users")
			@Results({
				@Result(property = "name", column = "name")
			})
			List<User> getAllUser();
		
			@Select("SELECT * FROM users WHERE id = #{id}")
			@Results({
				@Result(property = "name", column = "name")
			})
			User getUser(Long id);
	
			@Insert("INSERT INTO users(id,name,passwd) VALUES(#{id}, #{name}, #{passwd})")
			void insert(User user);
	
			@Update("UPDATE users SET name=#{name} WHERE id = #{id}")
			void update(User user);
		
			@Delete("DELETE FROM users WHERE id = #{id}")
			void delete(Long id);
	
		}


	> @Select 是查询类的注解，同理@Update，@Insert，@Delete等同于sql中的指令
	
	> @Results 是返回的结果集，实体属性和数据库字段名一一对应，如果实体和数据库字段名保持一致，则不需要这个属性


3. 使用

		@RunWith(SpringRunner.class)
		@SpringBootTest
		public class UserMapperTest {
		
			@Autowired
			private UserMapper UserMapper;
		
			@Test
			public void testInsert() throws Exception {
				UserMapper.insert(new User("aa", "a123456", UserSexEnum.MAN));
				UserMapper.insert(new User("bb", "b123456", UserSexEnum.WOMAN));
				UserMapper.insert(new User("cc", "b123456", UserSexEnum.WOMAN));
		
				Assert.assertEquals(3, UserMapper.getAll().size());
			}
		
			@Test
			public void testQuery() throws Exception {
				List<UserEntity> users = UserMapper.getAll();
				System.out.println(users.toString());
			}
			
			@Test
			public void testUpdate() throws Exception {
				User user = UserMapper.getOne(3l);
				System.out.println(user.toString());
				user.setNickName("neo");
				UserMapper.update(user);
				Assert.assertTrue(("neo".equals(UserMapper.getOne(3l).getNickName())));
			}
		}

## 配置文件版 ##

1. 添加application.properties相关配置

		mybatis.config-location=classpath:mybatis/mybatis-config.xml
		mybatis.mapper-locations=classpath:mybatis/mapper/*.xml

	指定了mybatis基础配置文件和实体类映射文件的地址

	mybatis-config.xml 配置

		<configuration>
			<typeAliases>
				<typeAlias alias="Integer" type="java.lang.Integer" />
				<typeAlias alias="Long" type="java.lang.Long" />
				<typeAlias alias="HashMap" type="java.util.HashMap" />
				<typeAlias alias="LinkedHashMap" type="java.util.LinkedHashMap" />
				<typeAlias alias="ArrayList" type="java.util.ArrayList" />
				<typeAlias alias="LinkedList" type="java.util.LinkedList" />
			</typeAliases>
		</configuration>

2. 添加实体类的映射文件

		<mapper namespace="com.example.demo.UserMapper" >
		    <resultMap id="BaseResultMap" type="com.example.demo.UserEntity" >
		        <id column="id" property="id" jdbcType="BIGINT" />
		        <result column="userName" property="userName" jdbcType="VARCHAR" />
		        <result column="passWord" property="passWord" jdbcType="VARCHAR" />
		        <result column="user_sex" property="userSex" javaType="com.example.demo.UserSexEnum"/>
		        <result column="nick_name" property="nickName" jdbcType="VARCHAR" />
		    </resultMap>
		    
		    <sql id="Base_Column_List" >
		        id, userName, passWord, user_sex, nick_name
		    </sql>
		
		    <select id="getAll" resultMap="BaseResultMap"  >
		       SELECT 
		       <include refid="Base_Column_List" />
			   FROM users
		    </select>
		
		    <select id="getOne" parameterType="java.lang.Long" resultMap="BaseResultMap" >
		        SELECT 
		       <include refid="Base_Column_List" />
			   FROM users
			   WHERE id = #{id}
		    </select>
		
		    <insert id="insert" parameterType="com.neo.entity.UserEntity" >
		       INSERT INTO 
		       		users
		       		(userName,passWord,user_sex) 
		       	VALUES
		       		(#{userName}, #{passWord}, #{userSex})
		    </insert>
		    
		    <update id="update" parameterType="com.neo.entity.UserEntity" >
		       UPDATE 
		       		users 
		       SET 
		       	<if test="userName != null">userName = #{userName},</if>
		       	<if test="passWord != null">passWord = #{passWord},</if>
		       	nick_name = #{nickName}
		       WHERE 
		       		id = #{id}
		    </update>
		    
		    <delete id="delete" parameterType="java.lang.Long" >
		       DELETE FROM
		       		 users 
		       WHERE 
		       		 id =#{id}
		    </delete>
		</mapper>

3. 编写dao层代码

		public interface UserMapper {
			
			List<User> getAll();
			
			User getOne(Long id);
		
			void insert(User user);
		
			void update(User user);
		
			void delete(Long id);
		
		}