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

				<form action="/Serv" method="get">
					<input type="submit" value="DO GET">
				</form>

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
	
* ### doGet()
	*	#### URL 매핑을 이용
		* ##### URL 매핑은 은 기본적으로 get 방식이므로 매핑된 URL로 접근하게 되면 doGet() 메소드가 호출된다
		
   ![](/assets/img/jsp/servlet_get_post1.png)	
	 
	* #### form 태그를 이용 (method="get") 
		* ##### DO GET 클릭

    ![](/assets/img/jsp/servlet_get_post2.png)
		 
    ![](/assets/img/jsp/servlet_get_post1_1.png)
			
* ### doPost()
	* ##### URL 매핑으로 접근하면 get 방식을 이용하므로 form태그를 이용해 호출해야 한다
	
	* ##### DO POST 클릭

   ![](/assets/img/jsp/servlet_get_post2.png)
 
	* ##### URL은 같지만 form 태그의 method="post" 이었기 때문에 doPost()가 호출된다

   ![](/assets/img/jsp/servlet_get_post3.png)
	 
* ###### [신입SW인력을 위한 실전 JSP]	


[신입SW인력을 위한 실전 JSP]: https://www.youtube.com/watch?v=6D1hOSyHJTg&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=37