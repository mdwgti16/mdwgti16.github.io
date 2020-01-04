---
title: Intellij JSP - Servlet Life Cycle (생명 주기, 선처리, 후처리)
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-4 13:27:28 +0900'
categories: JSP
---

> ## Servlet Life Cycle (생명 주기, 선처리, 후처리)

* ### Project - ServletTest 
	* #### Servlet.java
		```java
		import javax.annotation.PostConstruct;
		import javax.annotation.PreDestroy;
		import javax.servlet.ServletException;
		import javax.servlet.ServletRequest;
		import javax.servlet.ServletResponse;
		import javax.servlet.annotation.WebServlet;
		import javax.servlet.http.HttpServlet;
		import javax.servlet.http.HttpServletRequest;
		import javax.servlet.http.HttpServletResponse;
		import java.io.IOException;
		import java.io.PrintWriter;

		@WebServlet(value = "/Serv")
		public class Servlet extends HttpServlet {
				@PostConstruct
				private void initPostConstruct(){
						System.out.println("initPostConstruct!!");
				}

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

				@Override
				public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
						super.service(req, res);
						System.out.println("Servlet Service");
				}

				protected void doPost(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
						PrintWriter out = response.getWriter();
						out.println("DD POST");
						System.out.println("doPost!!");
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

				@PreDestroy
				private void destroyPreDestroy(){
						System.out.println("destroyPreDestroy!!");
				}
		}
		```
	
* ### Servlet Life Cycle
	1. 	##### @PostConstruct - (선처리) 어노테이션을 사용하여 임의의 메소드 생성
	2. 	##### init() - Override 메소드
	3. 	##### doGet(),doPost()
	4. 	##### service(..., ...) - Override 메소드
	5. 	##### destroy() - Override 메소드
	6. 	##### @PreDestroy - (후처리) 어노테이션을 사용하여 임의의 메소드 생성
		
  ![](/assets/img/jsp/servlet_rifecycle.png)
	 
* ###### [신입SW인력을 위한 실전 JSP]	


[신입SW인력을 위한 실전 JSP]: https://www.youtube.com/watch?v=U6FA7oWgizc&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=38