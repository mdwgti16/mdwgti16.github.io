---
title: Intellij JSP - Servlet doGet(), doPost()
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-3 17:27:28 +0900'
categories: JSP
---

> ## Servlet doGet(), doPost()

* ### ServletTest
	* #### index.jsp	
			<%@ page contentType="text/html;charset=UTF-8" language="java" %>
			<html>
				<head>
					<title>$Title$</title>
				</head>
				<body>

				<form action="/Serv" method="post">
					<input type="submit" value="DO POST">
				</form>

				<input type="button" value="DO GET" onclick="location.href='/Serv'">

				</body>
			</html>
	
	* #### Servlet.java
	
	```java
	@WebServlet(value = "/Serv")
	public class Servlet extends HttpServlet {
			protected void doPost(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
					PrintWriter out = response.getWriter();
					out.println("DD POST");
			}

			protected void doGet(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
					PrintWriter out = response.getWriter();
					out.println("DD GET");
			}
}
	```
	
* ### Servlet.java

* ### doGet()
	1. #### URL 매핑을 이용
		* ##### URL은 기본적으로 get방식이므로 URL로 접근이 가능
		
    ![](/assets/img/jsp/servlet_get_post1.png)
	2. ##### form 태그나 button 이용

    ![](/assets/img/jsp/servlet_get_post2.png)
		 
    ![](/assets/img/jsp/servlet_get_post1.png)
			


* ###### [신입SW인력을 위한 실전 JSP]	


[신입SW인력을 위한 실전 JSP]: https://www.youtube.com/watch?v=MmxzA_0Vtoo&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=36