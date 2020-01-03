---
title: Intellij JSP - Servlet 생성 및 매핑
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-3 16:27:28 +0900'
categories: JSP
---

> ## Servlet 생성 및 매핑

### 1. Intellij Servlet 생성
* ##### 마우스 우클릭 -> New -> Create New Servlet
		
 ![](/assets/img/jsp/create_servlet1.png)
 
* ##### Create Java EE 6 annotated class 체크
			
 ![](/assets/img/jsp/create_servlet2.png)
 
### 2. Servlet 매핑
* #### Annotation을 이용한 매핑 
	```java
	@WebServlet("/Serv") // Annotation을 이용한 매핑 (= @WebServlet(urlPatterns = "/Serv") = @WebServlet(value = "/Serv"))
	public class Servlet extends HttpServlet {
			protected void doPost(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {

			}

			protected void doGet(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
					PrintWriter out = response.getWriter();
					out.println("Hello World");
			}
	}
	```

* #### web.xml을 이용한 매핑
		<servlet>
				<servlet-name>ServletTest</servlet-name>
				<servlet-class>example.Servlet</servlet-class>
		</servlet>
		<servlet-mapping>
				<servlet-name>ServletTest</servlet-name>
				<url-pattern>/Serv</url-pattern>
		</servlet-mapping>
		
* #### 결과 화면
			
   ![](/assets/img/jsp/servlet_mapping1.png)
			


* ###### [신입SW인력을 위한 실전 JSP]	


[신입SW인력을 위한 실전 JSP]: https://www.youtube.com/watch?v=MmxzA_0Vtoo&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=36