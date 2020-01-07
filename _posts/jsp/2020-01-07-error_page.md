---
title: Intellij JSP - 에외 처리
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-7 15:27:28 +0900'
categories: JSP
---

> ## 에외 처리



* ### Java Exception 
	* ##### make_error.jsp
		```jsp
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<html>
		<head>
		</head>
		<body>
		<%
				int num = 4/0;
		%>
		</body>
		</html>
		```
	* ##### 에러 발생 화면
		![](/assets/img/jsp/error1.png)
	
	* ##### 에러 처리
	```jsp
	<%
		 // try-catch를 추가
			try {
					int num = 4 / 0;
			} catch (Exception e) {
					out.print(e.getMessage());
			}
	%>
	```
	
	* ##### 예외 처리 결과
	![](/assets/img/jsp/error2.png)

* ### Page 지시자
	* ##### make_error_p.jsp
		* ###### <%@ page errorPage="error_page.jsp" %> - 에러 페이지 지정
		
		```jsp
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<%--Error Page 추가--%>
		<%@ page errorPage="error_page.jsp" %>
		<html>
		<head>
		</head>
		<body>
		<%
				int num = 4 / 0;
		%>
		</body>
		</html>
		```
		
	* ##### error_page.jsp
		* ###### 에러 페이지는 isErrorPage의 값을 "true" 로 변경
			
		```jsp
		<%@ page contentType="text/html;charset=UTF-8" language="java" isErrorPage="true" %>
		<html>
		<head>
				<title>Error Page</title>
		</head>
		<body>
		<%
				out.print(exception.getMessage());
		%>
		</body>
		</html>
		```
		
		
	* ##### 예외 처리 결과
	![](/assets/img/jsp/error3.png)
			
* ### web.xml
	* ##### web.xml에 error page 추가
		```xml
		<?xml version="1.0" encoding="UTF-8"?>
		<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
						 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
						 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
						 version="4.0">
				...

				<!--추가-->
				<error-page>
						<error-code>404</error-code>
						<location>/error/error_404.jsp</location>
				</error-page>
				<error-page>
						<error-code>500</error-code>
						<location>/error/error_500.jsp</location>
				</error-page>

				...
		</web-app>
		```
	
	* ##### error.jsp
		```jsp
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<html>
		<head>
		</head>
		<body>
		<%
				int num = 4 / 0;
		%>
		</body>
		</html>
		```
			
	* ##### error_404.jsp
		```jsp
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<%@ page isErrorPage="true" %>
		<html>
		<head>
				<title>404 Error Page</title>
		</head>
		<body>
		<h2>404 Error Page</h2>
		</body>
		</html>
		```

	* ##### error_500.jsp
		```jsp
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<%@ page isErrorPage="true" %>
		<html>
		<head>
				<title>500 Error Page</title>
		</head>
		<body>
		<h2>500 Error Page</h2>
		</body>
		</html>
		```
			
	* ##### 예외 처리 결과
		![](/assets/img/jsp/error4.png)
			
	* ##### 요청한 페이지가 없을 때
		![](/assets/img/jsp/error5.png)
	
	* ##### 에러 페이지에서 예외처리를 못하고 에러가 날 경우 아래의 코드 추가
		```jsp
		<% response.setStatus(200); %>
		```
			
		
				
* ###### [신입SW인력을 위한 실전 JSP Servlet]

				
[신입SW인력을 위한 실전 JSP Servlet]: https://www.youtube.com/watch?v=JXHceuYcytw&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=47