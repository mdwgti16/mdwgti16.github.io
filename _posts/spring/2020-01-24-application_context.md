---
title: Intellij Spring - 애플리케이션 컨텍스트 (Application Context)
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-24 15:27:28 +0900'
categories: Spring
---

> ## 애플리케이션 컨텍스트 (Application Context)


* ### 애플리케션 컨텍스트와 설정정보
	* ##### 빈(bean) : 스프링이 제어권을 가지고 직접 만들고 관계를 부여하는 오브젝트 = 자바빈 또는 앤터프라이즈 자바빈(EJB)에서 말하는 빈과 비슷한 오브젝트 단위의 컴포넌트
	* ##### 스프링 빈 : 스프링 컨테이너가 생성과 관계설정, 사용 등을 제어해주는 제어의 역전이 적용된 오브젝트
	* ##### 빈 팩토리 : 빈의 생성과 관계설정 같은 제어를 담당하는 IoC 오브젝트 
	* ##### 애플리케이션 컨텍스트 : 애플리케이션 전반에 걸쳐 모든 구성요소의 제어 작업을 담당하는 IoC 엔진
		* ###### 애플리케이션 컨텍스트는 ApplicationContext 인터베이스를 구현, ApplicationContext는 빈펙토리가 구현하는 BeanFactory 인터페이스를 상속했으므로 애플리케이션 컨텍스트는 일종의 빈 팩토리라고 볼 수 있다. 두 가지가 동일하다고 생각하면 된다.
	* #### 설정정보 생성
		* ###### 어노테이션을 이용
			* ###### DaoFactory.java
				```java
				import dao.*;
				import org.springframework.context.annotation.Bean;
				import org.springframework.context.annotation.Configuration;
				import org.springframework.jdbc.datasource.SimpleDriverDataSource;

				import javax.sql.DataSource;

				@Configuration
				public class DaoFactory {
						@Bean
						public UserDao userDao(){
								UserDao userDao = new UserDao();
								userDao.setConnectionMaker(connectionMaker());
								return userDao;
						}

						@Bean
						public ConnectionMaker connectionMaker() {
								return new MyConnectionMaker();
						}			
				```
				
			* ###### 어노테이션 설정정보를 사용하는 애플리케이션 컨텍스트 가져오기
				```java
				ApplicationContext context = new AnnotationConfigApplicationContext((DaoFactory.class));
				UserDao dao = context.getBean("userDao", UserDao.class);				
				```
			* ###### @Configuration : 빈 팩토리를 위한 오브젝트 설정을 담담하는 클래스라고 인식
			* ###### @Bean : 오브젝트를 만들어주는 메소드
		*  ###### xml 설정정보 이용
			*  ###### applicationContext.xml
				```xml
				<?xml version="1.0" encoding="UTF-8"?>
				<beans xmlns="http://www.springframework.org/schema/beans"
							 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
							 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

						<bean id="userDao" class="dao.UserDao">
								<property name="connectionMaker" ref="connectionMaker"/>
						</bean>

						<bean id="connectionMaker" class="dao.MyConnectionMaker"/>
				</beans>
				```
			* ##### xml 설정정보를 사용하는 애플리케이션 컨텍스트 가져오기
				```java
				ApplicationContext context = new GenericXmlApplicationContext("applicationContext.xml");
				UserDao dao = context.getBean("userDao", UserDao.class);				
				```
			* ###### @Configuration -> <beans> : 빈 설정파일
			* ###### @Bean methodName -> <bean id="methodName" : 빈의 이름
			* ###### return new BeanClass(); -> class="a.b.c... BeanClass">
		* #### setter를 이용한 프로퍼티 값 주입
			* ##### DaoFactory.java
				```java
				...
				@Configuration
				public class DaoFactory {				
						...
						@Bean
						public DataSource dataSource(){
								SimpleDriverDataSource dataSource = new SimpleDriverDataSource();
								dataSource.setDriverClass(com.mysql.jdbc.Driver.class);
								dataSource.setUrl("jdbc:mysql://localhost:3306/SPRING");
								dataSource.setUsername("root");
								dataSource.setPassword("1234");
								return dataSource;
						}
				}
				```
			* ##### applicationContext.xml
				```xml
				<?xml version="1.0" encoding="UTF-8"?>
				<beans xmlns="http://www.springframework.org/schema/beans"
							 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
							 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
						...
						<bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
								<property name="driverClass" value="com.mysql.jdbc.Driver"/>
								<property name="url" value="jdbc:mysql://localhost:3306/SPRING?useSSL=false"/>
								<property name="username" value="root"/>
								<property name="password" value="1234"/>
						</bean>
				</beans>
				```
				* ###### 해당 클래스에 setter 메소드가 있어야 된다
* ### 애플리케션 컨텍스트를 사용했을 때의 장점
	* ##### 클라이언트는 구체적인 패곹리 클래스를 알 필요가 없음
	* ##### 애플리케이션 컨텍스트는 종합 IoC 서비스를 제공해줌
	* ##### 애플리케이션 컨텍스트는 빈을 검색하는 다양한 방법을 제공함

* ###### [토비의 스프링 3.1]


[토비의 스프링 3.1]: https://book.naver.com/bookdb/book_detail.nhn?bid=7006516