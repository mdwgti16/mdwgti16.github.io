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
* ### Tomcat 연동
	* ##### Add Configuration -> Add New Configuration -> Tomcat Server -> Local 선택
		![](/assets/img/spring/create_project5.png)
	* ##### Fix -> Application context 를 /로 수정
		![](/assets/img/spring/create_project6.png)
		![](/assets/img/spring/create_project7.png)
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
		![](/assets/img/spring/create_project12.png)

			

> ###### [Spring MVC, Maven 프로젝트 설정 방법]



[Spring MVC, Maven 프로젝트 설정 방법]: https://whitepaek.tistory.com/41