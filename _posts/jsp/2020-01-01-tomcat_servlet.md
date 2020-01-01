---
title: JSP - Tomcat 환경에서 JSP, Servlet
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 9:27:28 +0900'
categories: JSP
---

> ## JDK, Tomcat 설치

### 1 JSP 생성
* ##### Tomcat이 설치된 폴더를 찾아 그 하위 폴더인 webapps 폴더 내에 jspTest라는 폴더를 만든다
	* ###### webapps 폴더 내에 새로운 폴더 생성 
		* ###### = 새로운 웹 어플리케이션 생성
		* ###### = 새로운 Context 생성 (ServletContext)
* ##### jspTest 폴더 안에 hellowolrd.jsp 파일을 생성하고 다음 코드를 타이핑 한다
	```jsp
	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
		<head>
			<title>title</title>
		</head>
		<body>
			<% out.println("Hello World"); %> <br/>
			<% out.println("안녕하세요"); %>
		</body>
	</html>
	```
* ##### Tomcat 실행 (cmd 창 열고 startup.bat)
			
* ##### 결과 화면
			
 ![](/assets/img/jsp/tomcat-jsp/jsptest.png)

* ##### 실행하는 과정이 간단하게 보이지만 JSP 파일은 실행이 될 때 일단 Servlet인 Java 소스 파일로 변환되고 다시 class 파일로 커마일 된 후 이 클래스 파일이 JSP/Servlet 컨테이너인 Tomcat 내에서 실행되어 그 결과가 최종적으로 웹 브라우저로 전달된다.
	* ###### tomcat 설치 폴더\work\Catalina\localhost\jspTest\org\apache\jsp\index_jsp.java
	* ###### tomcat 설치 폴더\work\Catalina\localhost\jspTest\org\apache\jsp\index_jsp.class
			
### 2 Servlet  생성
* ##### Servlet java 파일 생성 -> class 파일로 컴파일 -> 서블릿 등록 및 URL 매핑 -> 실행
			
* ##### Servlet은 Java 코드로 작성을 하기 때문에 컴파일 과정이 필요하다. 별도의 CLASSPATH 환경을 설정 할 필요 없는 sjc.bat 파일을 생성한다
	* ###### sjc.bat 파일을 생성후 편집기를 이용해 다음의 내용을 타이핑 한다
		* ###### javac -cp %CATALINA_HOME%\lib\servlet-api.jar -d ..\classes %1
	
	* ###### 실행 경로에 상관없이 실행 시키기 위해 "jdk 설치 폴더/bin/" 안에 작성한 sjc.bat 파일을 넣어준다
			
* ##### Tomcat 설치 폴더/webapps/jspTest 폴더 내에 WEB-INF 폴더를 만들고 그 안에 classes 폴더와 java_sources폴더를 생성한다

* ##### java_sources 폴더 내에 HelloWorldServlet.java 파일을 생성한 후 다음의 코드를 타이핑 한다
	```java
	import javax.servlet.*;
	import javax.servlet.http.*;
	import java.io.*;

	public class HelloWorldServlet extends HttpServlet {
		public void init() {
			System.out.println("Init!!!");
		}	

		public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
				System.out.println("doGet!!!");
			response.setContentType("text/html");
			PrintWriter out = response.getWriter();
			out.println("<html><body bgcolor=\"yellow\">Hello Servlet!</body></html>");
		}	

		public void destroy() {
			System.out.println("destroy!!!");
		}
	}
	```			

* ##### cmd 창을 열고 다음의 명령어를 실행시킨다
	* ###### 현재 디렉토리 변경 	
		* ###### `cd 'Tomcat 설치 폴더'\webapps\jspTest\WEB-INF\java_sources`
	
	* ###### HelloWorldServlet.java 를 class 파일로 컴파일
		* ###### sjc HelloWorldServlet.java
	
 * ###### 컴파일 완료 (Tomcat 설치 폴더\webapps\jspTest\WEB-INF\classes\HelloWorldServlet.class)
			
 ![](/assets/img/jsp/tomcat-jsp/sjc.png)
			
* ##### web.xml 에 Servlet 등록 및 URL 매핑
	* ###### Tomcat 설치 폴더\webapps\jspTest\WEB-INF\ 폴더 안에 web.xml 파일을 생성후 다음의 내용을 타이핑 한다
		```xml
		<?xml version="1.0" encoding="UTF-8"?>
		<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
			<servlet>
				<servlet-name>helloServlet</servlet-name>
				<servlet-class>HelloWorldServlet</servlet-class>
			</servlet>
			<servlet-mapping>
				<servlet-name>helloServlet</servlet-name>
				<url-pattern>/helloServlet</url-pattern>
			</servlet-mapping>
		</web-app>
		```
		* ###### servlet-name 매핑이 되어야 하기 때문에 같아야함
		* ###### servlet-class는 HelloWorldServlet.java의 클래스 이름
		* ###### url-pattern는 검색될 url 이름
			
* ##### 실행 결과
			
 ![](/assets/img/jsp/tomcat-jsp/servlettest.png)			

	
* ###### [Servlet을 포함한 JSP 2.1 웹 프로그래밍 입문에서 완성까지 ]

[Servlet을 포함한 JSP 2.1 웹 프로그래밍 입문에서 완성까지 ]: https://book.naver.com/bookdb/book_detail.nhn?bid=6468787