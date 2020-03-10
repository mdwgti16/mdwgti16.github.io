---
title: Intellij Spring - Spring MVC + Maven 프로젝트 만들기
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-22 15:27:28 +0900'
categories: Spring
---

> ## Spring MVC + Maven 프로젝트 만들기


* ### Maven 프로젝트 생성
	* ##### Create New Project
		![](/assets/img/spring/create_project1.png)
	* ##### Maven 선택 -> java version 선택 -> Next -> 프로젝트 생성
		![](/assets/img/spring/create_project2.png)
* ### Spring MVC 추가		
	* ##### 프로젝트 우 클릭 -> Add Framework Support.. 클릭
		![](/assets/img/spring/create_project3.png)
	* ##### Spring MVC 선택 -> OK
		![](/assets/img/spring/create_project4.png)
	* ##### Artifacts 설정
		![](/assets/img/spring/create_project4-1.png)
		![](/assets/img/spring/create_project4-2.png)
	* ##### Controller 생성
		```java
		package com.ex.spring.controller;

		import org.springframework.stereotype.Controller;
		import org.springframework.web.bind.annotation.RequestMapping;

		@Controller
		public class MController {
				@RequestMapping("/index")
				public String home(){
						return "index";
				}
		}
		```
	* ##### 디렉터리 구조
		![](/assets/img/spring/create_project4-3.png)
* ### xml 파일 설정
	<details markdown="1">
	<summary><font size="4px" >web.xml</font></summary>
	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
					 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
					 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
					 version="4.0">
			<context-param>
					<param-name>contextConfigLocation</param-name>
					<param-value>/WEB-INF/applicationContext.xml</param-value>
			</context-param>
			<listener>
					<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
			</listener>
			<servlet>
					<servlet-name>dispatcher</servlet-name>
					<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
					<load-on-startup>1</load-on-startup>
			</servlet>
			<servlet-mapping>
					<servlet-name>dispatcher</servlet-name>
					<url-pattern>/</url-pattern>
			</servlet-mapping>
	</web-app>	
	```	
	</details>	

	<details markdown="1">
	<summary><font size="4px" >dispatcher-servlet.xml</font></summary>
	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
				 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
				 xmlns:context="http://www.springframework.org/schema/context"
				 xmlns:mvc="http://www.springframework.org/schema/mvc"
				 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

			<mvc:annotation-driven/>

			<mvc:resources mapping="/resources/**" location="/resources/"/>

			<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
					<property name="prefix" value="/WEB-INF/views/"/>
					<property name="suffix" value=".jsp"/>
			</bean>

			<context:component-scan base-package="com.ex.spring.controller"/>
	</beans>
	```	
	</details>		
* ### Tomcat 연동
	* ##### Add Configuration -> Add New Configuration -> Tomcat Server -> Local 선택
		![](/assets/img/spring/create_project5.png)
	* ##### Fix -> Application context 를 /로 수정
		![](/assets/img/spring/create_project6.png)
		![](/assets/img/spring/create_project7.png)
	* ##### index 화면
		![](/assets/img/spring/create_project7-1.png)
* ### MySQL 연동		
	* ##### Project Structure (Ctrl + Alt + Shift + S) -> Libraries -> New Project Library -> Java
		![](/assets/img/spring/create_project8.png)
	* ##### mysql-connector-java-5.1.48-bin.jar를 추가 -> OK
		![](/assets/img/spring/create_project9.png)
	
* ### JDBC Test
	![](/assets/img/spring/create_project11.png)
	* ##### DBTest.java
		```java
		import org.junit.jupiter.api.Test;

		import java.sql.Connection;
		import java.sql.DriverManager;
		import java.sql.PreparedStatement;
		import java.sql.SQLException;

		public class DBTest {
				@Test
				public void test() throws SQLException {
						PreparedStatement pstmt = null;
						Connection conn = getConnTest();
						String query = "CREATE TABLE USER(ID VARCHAR(20) PRIMARY KEY, PW VARCHAR(2))";
						pstmt = conn.prepareStatement(query);
						pstmt.executeUpdate();

						pstmt.close();
						conn.close();
				}

				public Connection getConnTest() {
						try {
								String dbURL = "jdbc:mysql://localhost:3306/SPRING";
								String dbID = "root";
								String dbPW = "1234";
								Class.forName("com.mysql.jdbc.Driver");
								return DriverManager.getConnection(dbURL, dbID, dbPW);
						} catch (Exception e) {
								e.printStackTrace();
						}
						return null;
				}
		}
		```
	* ##### USER 테이블이 추가 됨
			

> ###### [Spring MVC, Maven 프로젝트 설정 방법]
> ###### [Intellij에서 Spring MVC 시작하기]


[Spring MVC, Maven 프로젝트 설정 방법]: https://whitepaek.tistory.com/41
[Intellij에서 Spring MVC 시작하기]: https://nesoy.github.io/articles/2017-02/SpringMVC