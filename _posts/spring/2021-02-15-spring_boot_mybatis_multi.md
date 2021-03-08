---
title: Spring Boot - Mybatis Multiple Datasource 연동
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2021-02-15 22:49:28 +0900'
categories:
- Spring Boot
---

> ## Spring Boot - Mybatis Multiple Datasource 연동
#### 기본적인 MyBatis 연동 방법은 [Spring Boot - Mybatis 연동](https://mdwgti16.github.io/spring%20boot/spring_boot_mybatis/) 참고

* ### Multiple Hikari Datasource 환경 설정
	application.yml
	```
    spring:
      datasource:
        hikari:
            primary:
              driver-class-name: com.mysql.cj.jdbc.Driver
              jdbc-url: jdbc:mysql://localhost:3306/testdb?useSSL=false&    useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
              username: root
              password: 1234
              maximum-pool-size: 50
              max-lifetime: 30000
              idle-timeout: 28000
              connection-timeout: 0
              transaction-isolation: TRANSACTION_READ_UNCOMMITTED
            secondary:
              driver-class-name: com.mysql.cj.jdbc.Driver
              jdbc-url: jdbc:mysql://localhost:3306/testdb2?useSSL=false&    useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
              username: root
              password: 1234
              maximum-pool-size: 50
              max-lifetime: 30000
              idle-timeout: 28000
              connection-timeout: 0
              transaction-isolation: TRANSACTION_READ_UNCOMMITTED
    ```
* ### Multiple DataSuorce 설정
	MultipleMariaDataSourceConfiguration.java
	```
	@Configuration
	public class MultipleMariaDataSourceConfiguration {

		@Bean
		@Primary
		@Qualifier("primaryHikariConfig")
		@ConfigurationProperties(prefix="spring.datasource.hikari.primary")
		public HikariConfig primaryHikariConfig() {
			return new HikariConfig();
		}

		@Bean
		@Primary
		@Qualifier("primaryDataSource")
		public DataSource primaryDataSource() throws Exception {
			return new HikariDataSource(primaryHikariConfig());
		}
		@Bean
		@Qualifier("secondaryHikariConfig")
		@ConfigurationProperties(prefix="spring.datasource.hikari.secondary")
		public HikariConfig secondaryHikariConfig() {
			return new HikariConfig();
		}

		@Bean
		@Qualifier("secondaryDataSource")
		public DataSource secondaryDataSource() throws Exception {
			return new HikariDataSource(secondaryHikariConfig());
		}
	}
	```
	##### DataSource가 2개 이상일 경우 기본으로 사용 할 DataSource를 @Primary 어노테이션으로 지정 해야 됨.
* ### MyBatis 설정	
	PrimaryMyBatisConfiguration.java
	```
	@Configuration
	public class MyBatisPrimaryConfiguration {
		@Primary
		@Bean(name = "primarySqlSessionFactory")
		public SqlSessionFactory sqlSessionFactory(
				@Qualifier("primaryDataSource") DataSource dataSource, ApplicationContext applicationContext) throws Exception {
			SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
			sqlSessionFactoryBean.setDataSource(dataSource);
			sqlSessionFactoryBean.setConfigLocation(applicationContext.getResource("classpath:mybatis/mybatis-config.xml"));
			sqlSessionFactoryBean.setMapperLocations(applicationContext.getResources("classpath:mybatis/mapper/**/*.xml"));

			return sqlSessionFactoryBean.getObject();
		}

		@Primary
		@Bean(name = "primarySqlSessionTemplate")
		public SqlSessionTemplate sqlSessionTemplate(
				@Qualifier("primarySqlSessionFactory") SqlSessionFactory sqlSessionFactory) {
			return new SqlSessionTemplate(sqlSessionFactory);
		}
	}	
	```
	SecondaryMyBatisConfiguration.class
	```
	@Configuration
	public class SecondaryMyBatisConfiguration {
		@Bean(name = "secondarySqlSessionFactory")
		public SqlSessionFactory sqlSessionFactory(
				@Qualifier("secondaryDataSource") DataSource dataSource, ApplicationContext applicationContext) throws Exception {
			SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
			sqlSessionFactoryBean.setDataSource(dataSource);
			sqlSessionFactoryBean.setConfigLocation(applicationContext.getResource("classpath:mybatis/mybatis-config.xml"));
			sqlSessionFactoryBean.setMapperLocations(applicationContext.getResources("classpath:mybatis/mapper/**/*.xml"));

			return sqlSessionFactoryBean.getObject();
		}

		@Bean(name = "secondarySqlSessionTemplate")
		public SqlSessionTemplate sqlSessionTemplate(
				@Qualifier("secondarySqlSessionFactory") SqlSessionFactory sqlSessionFactory) {
			return new SqlSessionTemplate(sqlSessionFactory);
		}
	}
	```	
	##### DataSource와 마찬가지로 SqlSessionFactory와 SqlSessionTemplate이 2개 이상일 경우 기본으로 사용 할 Bean을 @Primary 어노테이션으로 지정 해야 됨.

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

			public UserDao(@Qualifier("secondarySqlSessionTemplate") SqlSession sqlSession) {
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
	result -> null
 	result -> UserDto(id=abc, password=qwer, name=1234, email=zxcv)
	```
> ###### [Spring Boot 2 Multiple DataSource](https://gigas-blog.tistory.com/122)