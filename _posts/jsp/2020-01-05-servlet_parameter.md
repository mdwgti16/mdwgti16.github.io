---
title: Intellij JSP - Servlet Config/ Servlet Context Init Parameter(파라미터 초기화 및 공유)
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-5 16:27:28 +0900'
categories: JSP
---

> ## Servlet Config/ Servlet Context Init Parameter(서블릿 파라미터 초기화 및 공유)

* ### Project - InitParam
	* ##### ServletInitParam.java
	
		```java
		import javax.servlet.ServletException;
		import javax.servlet.annotation.WebInitParam;
		import javax.servlet.annotation.WebServlet;
		import javax.servlet.http.HttpServlet;
		import javax.servlet.http.HttpServletRequest;
		import javax.servlet.http.HttpServletResponse;
		import java.io.IOException;
		import java.io.PrintWriter;

		public class ServletInitParam extends HttpServlet {
				protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {}

				protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
						response.setContentType("text/html; charset=utf-8");

						String id = getInitParameter("id");
						String pw = getInitParameter("pw");
						String desc = getInitParameter("desc");

						PrintWriter out = response.getWriter();
						out.println("<title>form result</title>" +
										"당신이 입력한 정보입니다.<br>" +
										"<b>ID</b> : " + id + "<br>" +
										"<b>Password</b> : " + pw + "<br>" +
										"<b>자기소개</b><br>" +
										desc);
				}
		}
		```
	
	* ##### web.xml
		```xml
		<?xml version="1.0" encoding="UTF-8"?>
		<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
						 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
						 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
						 version="4.0">

			...

			<servlet>
					<servlet-name>InitParam</servlet-name>
					<servlet-class>example.ServletInitParam</servlet-class>
			</servlet>
			<servlet-mapping>
					<servlet-name>InitParam</servlet-name>
					<url-pattern>/initParam</url-pattern>
			</servlet-mapping>

			...

		</web-app>
		```

* ### 특정 Servlet의 Parameter 초기화
	* #### web.xml 사용
		* ##### web.xml에 매핑한 Servlet 태그 안에서 파라미터 초기화
			```xml
			<servlet>
					<servlet-name>InitParam</servlet-name>
					<servlet-class>example.ServletInitParam</servlet-class>

					<!--추가-->
					<init-param>
							<param-name>id</param-name>
							<param-value>qwer</param-value>
					</init-param>
					<init-param>
							<param-name>pw</param-name>
							<param-value>1234</param-value>
					</init-param>
					<init-param>
							<param-name>desc</param-name>
							<param-value>Hello</param-value>
					</init-param>

			</servlet>
			<servlet-mapping>
					<servlet-name>InitParam</servlet-name>
					<url-pattern>/initParam</url-pattern>
			</servlet-mapping>
			```
			
		* ##### 매핑된 URL로 접근하면 초기화 시켜놓은 파라미터가 출력된 것을 볼 수 있다

			![](/assets/img/jsp/servlet_param1.png)
 
	* #### Annotation 사용
		* ##### ServletInitParam.java의 클래스 위에 아래의 어노태이션을 추가
	
			```java
			@WebServlet(name = "ServletInitParam", urlPatterns = {"/initParam"},
					initParams = {@WebInitParam(name = "id", value = "qwer"), @WebInitParam(name = "pw", value = "1234"),
									@WebInitParam(name = "desc", value = "Hello")})
			```
									
		* ##### 마찬가지로 초기화 시켜놓은 파라미터가 출력된 것을 볼 수 있다

			![](/assets/img/jsp/servlet_param1.png)
		
		
* ### Servlet Context을 사용해 모든 Servlet에 파라미터 공유
	* #### web.xml에 Context Param을 초기화
		* ##### web.xml에 아래의 코드 추가
			```xml
			<context-param>
					<param-name>id</param-name>
					<param-value>qwer</param-value>
			</context-param>
			<context-param>
					<param-name>pw</param-name>
					<param-value>asdf</param-value>
			</context-param>
			```
			
		* ##### ServletContextParam.java
			```java
			import javax.servlet.ServletException;
			import javax.servlet.annotation.WebServlet;
			import javax.servlet.http.HttpServlet;
			import javax.servlet.http.HttpServletRequest;
			import javax.servlet.http.HttpServletResponse;
			import java.io.IOException;
			import java.io.PrintWriter;

			@WebServlet(urlPatterns = {"/contextParam"})
			public class ServletContextParam extends HttpServlet {
					protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { }

					protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
							response.setContentType("text/html;charset=utf-8");
							String id = getServletContext().getInitParameter("id");
							String pw = getServletContext().getInitParameter("pw");

							PrintWriter out = response.getWriter();
							out.println("<title>form result</title>" +
											"당신이 입력한 정보입니다.<br>" +
											"<b>ID</b> : " + id + "<br>" +
											"<b>Password</b> : " + pw );
					}
			}
			```
		 
	 * ##### 결과 화면. ServletContextParam 뿐 아니라 다른 Servlet에서도 파라미터가 공유된다
		 
		![](/assets/img/jsp/servlet_param2.png)		 
* ###### [신입SW인력을 위한 실전 JSP Servlet]


[신입SW인력을 위한 실전 JSP Servlet]: https://www.youtube.com/watch?v=2Pqi-kUMwtw&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=39