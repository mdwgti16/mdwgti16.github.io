---
title: Intellij JSP - Servlet Context Listener(서블릿 리스너)
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-6 14:27:28 +0900'
categories: JSP
---

> ## Servlet Context Listener(서블릿 리스너)

* ### Project - ContextListener
	* ##### Servlet.java
	
		```java
		import javax.servlet.ServletException;
		import javax.servlet.annotation.WebServlet;
		import javax.servlet.http.HttpServlet;
		import javax.servlet.http.HttpServletRequest;
		import javax.servlet.http.HttpServletResponse;
		import java.io.IOException;
		import java.io.PrintWriter;

		@WebServlet(value = "/Serv")
		public class Servlet extends HttpServlet {

				@Override
				public void init() throws ServletException {
						super.init();
						System.out.println("init!!");
				}

				@Override
				protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
						super.service(req, resp);
						System.out.println("HttpServlet Service");
				}


				protected void doPost(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
				}

				protected void doGet(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
						PrintWriter out = response.getWriter();
						out.println("DD GET");
						System.out.println("doGet!!");
				}

				@Override
				public void destroy() {
						super.destroy();
						System.out.println("destroy!!");
				}
		}
		```

1. ### Servlet Context Listener 생성
	* #### 마우스 우클릭 -> New -> Create New Listener
		![](/assets/img/jsp/listener2.png)
		
	* #### ServletContextListener 제외하고 나머지 implements 제거
		```java
		import javax.servlet.ServletContextEvent;
		import javax.servlet.ServletContextListener;
		import javax.servlet.annotation.WebListener;

		@WebListener()
		public class ContextListener implements ServletContextListener{
				public ContextListener() {}

				public void contextInitialized(ServletContextEvent sce) {
						System.out.println("contextInitialized");
				}

				public void contextDestroyed(ServletContextEvent sce) {
						System.out.println("contextDestroyed");
				}
		}
		```
		
2. ### web.xml에 Context Listener 등록
	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
					 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
					 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
					 version="4.0">
			...

			<!-- 추가 -->
			<listener>
					<listener-class>example.ContextListener</listener-class>
			</listener>

			...
	</web-app>
	```
			
* ### 로그로 동작여부를 확인 할 수 있다
	![](/assets/img/jsp/listener1.png)
		 
* ###### [신입SW인력을 위한 실전 JSP Servlet]


[신입SW인력을 위한 실전 JSP Servlet]: https://www.youtube.com/watch?v=nb0ACztuQR0&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=40