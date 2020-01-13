---
title: Intellij JSP - MySQL DBCP (Connection Pool)
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-13 13:27:28 +0900'
categories: JSP
---

> ## MySQL DBCP (Connection Pool)


* ### Connection Pool 설정
	* ##### context.xml
		```
		<Context>
			...

			<Resource name="jdbc/mysql" auth="Container"
									type="javax.sql.DataSource" username="root" password="1234"
									driverClassName="com.mysql.jdbc.Driver"
									url="jdbc:mysql://localhost:3306/JSP"
									maxActive="50" maxIdle="30" maxWait="1000"/>

			...
		</Context>
		```
		* ###### name - JNDI 이름
		* ###### type - resource의 반환 타입
		* ###### username - DB접속 이름
		* ###### username - DB접속 비밀번호
		* ###### url  - DB 접속 URL
		* ###### driverClassName - DBCP를 이용하기 위한 DriverClass
		* ###### maxActive - 사용이 되고 있는 최대 커넥션 개수 (-1로 설정하면 동시에 생서되는 커넥션 개수에 제한을 두지 않음)
		* ###### maxIdle - 사용되지 않는 커넥션을 커넥션 풀에 저장해 둘 수 있는 최대 수
		* ###### maxWait - 단위는 mx 단위, 동시 접속자 수가 많아져서 사용 가능한 커넥션이 없을 때 maxWait 에 지정된 시간만큼 대기
		
	* ##### web.xml
		```xml
		<?xml version="1.0" encoding="UTF-8"?>
		<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
						 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
						 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
						 version="4.0">
				...
	
				<resource-ref>
						<description>MySQL DBCP</description>
						<res-ref-name>jdbc/mysql</res-ref-name>
						<res-type>javax.sql.DataSource</res-type>
						<res-auth>Container</res-auth>
				</resource-ref>
	
				...
		</web-app>		
		```
		
* ### Connection Pool 을 이용한 Connection 가져오기
	* ##### DBConnUtils.java
		```java
		import javax.naming.Context;
		import javax.naming.InitialContext;
		import javax.sql.DataSource;
		import java.sql.Connection;

		public class DBConnUtils {

				public static Connection getDBCP() {
						Connection conn = null;
						String jndiName = "jdbc/mysql";
						try {
								Context initContext = (Context) new InitialContext().lookup("java:/comp/env");
								DataSource ds = (DataSource) initContext.lookup(jndiName);
								conn = ds.getConnection();
						} catch (Exception e) {
								e.printStackTrace();
						}

						return conn;
				}
		}
		
		```
			
			

* ###### [신입SW인력을 위한 실전 JSP Servlet]
* ###### [TOMCAT JDBC 설정방법 + SPRING DATASOURCE 설정방법]


[TOMCAT JDBC 설정방법 + SPRING DATASOURCE 설정방법]: https://mkil.tistory.com/425
[신입SW인력을 위한 실전 JSP Servlet]: https://www.youtube.com/watch?v=2_vetq6gGMo&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=53