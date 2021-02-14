---
title: Spring Boot - Mybatis 빠르게 연동하기
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2021-02-14 20:27:28 +0900'
categories:
- Spring Boot
---

> ## Spring Boot - Mybatis 빠르게 연동하기

* ### Dependency 설정 
	pom.xml
	```
	<dependency>
		<groupId>org.mybatis.spring.boot</groupId>
		<artifactId>mybatis-spring-boot-starter</artifactId>
		<version>2.1.4</version>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-data-jdbc</artifactId>
	</dependency>
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<scope>runtime</scope>
	</dependency>
	<dependency>
		<groupId>org.mariadb.jdbc</groupId>
		<artifactId>mariadb-java-client</artifactId>
		<scope>runtime</scope>
	</dependency>
	```
	
* ### Hikari Datasource 환경 설정
	application.yml
	```
	spring:
	datasource:
		hikari:
		driver-class-name: com.mysql.cj.jdbc.Driver
		jdbc-url: jdbc:mysql://localhost:3306/testdb?useSSL=false&useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
		username: root
		password: 1234
		maximum-pool-size: 50
		max-lifetime: 30000
		idle-timeout: 28000
		connection-timeout: 0
		transaction-isolation: TRANSACTION_READ_UNCOMMITTED
	```
* ### DataSuorce 설정
	MariaDataSourceConfiguration.java
	```
	@Configuration
	public class MariaDataSourceConfiguration {

		@Bean
		@Qualifier("hikariConfig")
		@ConfigurationProperties(prefix="spring.datasource.hikari")
		public HikariConfig hikariConfig() {
			return new HikariConfig();
		}

		@Bean
		@Primary
		@Qualifier("dataSource")
		public DataSource dataSource() throws Exception {
			return new HikariDataSource(hikariConfig());
		}
	}
	```
* ### MyBatis 설정	
	MyBatisConfiguration.java
	```
	@Configuration
	public class MyBatisConfiguration {
		@Primary
		@Bean(name = "sqlSessionFactory")
		public SqlSessionFactory sqlSessionFactory(
				@Qualifier("dataSource") DataSource dataSource, ApplicationContext applicationContext) throws Exception {
			SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
			sqlSessionFactoryBean.setDataSource(dataSource);
			sqlSessionFactoryBean.setConfigLocation(applicationContext.getResource("classpath:mybatis/mybatis-config.xml"));
			sqlSessionFactoryBean.setMapperLocations(applicationContext.getResources("classpath:mybatis/**/*.xml"));
			return sqlSessionFactoryBean.getObject();
		}

		@Primary
		@Bean(name = "sqlSessionTemplate")
		public SqlSessionTemplate sqlSessionTemplate(
				@Qualifier("sqlSessionFactory") SqlSessionFactory sqlSessionFactory) {
			return new SqlSessionTemplate(sqlSessionFactory);
		}
	}
	```
	mybatis-config.xml
	```
	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">

	<configuration>

		<settings>
			<setting name="mapUnderscoreToCamelCase" value="true"/>
		</settings>

		<typeAliases>
			<typeAlias alias="userDto" type="com.example.dto.UserDto"/>
		</typeAliases>

		<typeHandlers>
		</typeHandlers>

	</configuration>
	```	
	##### 공식 문서를 보면 datasource를 감지해서 SqlSessionFactory을 만들고, 만들어진 SqlSessionFactory로 SqlSessionTemplate을 자동으로 만든다고 한다. 따라서 특별한 설정이 필요하지 않을 경우 위의 MyBatisConfiguration.class는 없어도 문제가 없다.

* ### DTO 생성
	UserDto.class
	```
	@Data
	public class UserDto {
		String id;
		String password;
		String name;
		String email;
	}
	```	
* ### 세 가지 사용 방법
	* ### Mapper 사용
		UserMapper.class
		```
		@Mapper
		public interface UserMapper {
			@Select("SELECT * FROM user WHERE id = #{id}")
			UserDto findById(@Param("id") String id);
		}
		```	
	* ### SqlSessionFactory 사용
		UserDaoMapper.class
		```
		@Mapper
		public interface UserDaoMapper {
			UserDto selectUserById(String id);
		}
		```	
		user-dao-mapper.xml
		```
		<?xml version="1.0" encoding="UTF-8"?>
		<!DOCTYPE mapper
				PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
				"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

		<mapper namespace="com.example.dao.UserDaoMapper">
			<select id="selectUserById" parameterType="String" resultType="UserDto">
				SELECT *
				FROM USER
				WHERE id = #{value}
			</select>
		</mapper>
		```
	* ### SqlSessionTemplate 사용
		UserDao.class
		```
		@Component
		public class UserDao {
			private final SqlSession sqlSession;

			public UserDao(@Qualifier("sqlSessionTemplate") SqlSession sqlSession) {
				this.sqlSession = sqlSession;
			}

			public UserDto selectUserById(String id){
				return this.sqlSession.selectOne("com.example.dao.UserDao.selectUserById", id);
			}
		}
		```
		user-dao.xml
		```
		<?xml version="1.0" encoding="UTF-8"?>
		<!DOCTYPE mapper
				PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
				"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

		<mapper namespace="com.example.dao.UserDao">
			<select id="selectUserById" parameterType="String" resultType="UserDto">
				SELECT *
				FROM USER
				WHERE id = #{value}
			</select>
		</mapper>
		```
* ### 실행 및 결과
	TestMyBatis.class
	```
	@SpringBootApplication
	@Slf4j
	public class TestMyBatisApplication implements CommandLineRunner {

		private final UserMapper userMapper;
		private final UserDao userDao;

		public TestMyBatisApplication(UserMapper userMapper, UserDao userDao) {
			this.userMapper = userMapper;
			this.userDao = userDao;
		}

		@Autowired
		private UserDaoMapper userDaoMapper;

		public static void main(String[] args) {
			SpringApplication.run(TestMyBatisApplication.class, args);

		}

		@Override
		public void run(String... args) throws Exception {
			log.info("result -> {}",this.userMapper.findById("abc"));
			log.info("result -> {}",this.userDao.selectUserById("abc"));
			log.info("result -> {}",this.userDaoMapper.selectUserById("abc"));
		}
	}
	```
	
	result log
	```
 	result -> UserDto(id=abc, password=qwer, name=1234, email=zxcv)
	result -> UserDto(id=abc, password=qwer, name=1234, email=zxcv)
 	result -> UserDto(id=abc, password=qwer, name=1234, email=zxcv)
	```
> ###### [Introduction What is MyBatis-Spring-Boot-Starter?]
> ###### [MyBatis @Mapper 인터페이스는 어떻게 스프링 빈으로 와이어될 수 있을까?]
[Introduction
What is MyBatis-Spring-Boot-Starter?]: http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/
[MyBatis @Mapper 인터페이스는 어떻게 스프링 빈으로 와이어될 수 있을까?]: http://wiki.plateer.com/pages/viewpage.action?pageId=7767258