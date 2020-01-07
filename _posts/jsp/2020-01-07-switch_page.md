---
title: Intellij JSP - 페이지 이동 방법
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-7 11:27:28 +0900'
categories: JSP
---

> ## 페이지 이동 방법



* ### Forward
	* ##### jsp:forward
		
		```jsp
		<jsp:forward page="result.jsp">
			<jsp:param name="id" value="qwer"/>
			<jsp:param name="pw" value="1234"/>
		</jsp:forward>
		```
			
	* ##### pageContext.forward(String s)
		* ###### request.setAttribute(String s, Object o) 메소드를 이용해 값을 전달할 수 있음
		```jsp
		<% pageContext.forward("page.jsp"); %>
		```

	* ##### RequestDispatcher
		```jsp
		<%
			RequestDispatcher rd = request.getRequestDispatcher("page,jsp");
			rd.forward(request,response);
		%>
		```
		
		
* ### Redirect
	* ##### response.sendRedirect(String s)
		```jsp
		<%
			response.sendRedirect("page.jsp");
		%>
		```

* ### Html
	* ##### Form 태그
		```html
		<form action="page.jsp">
			<label>ID : <input type="text" name="id"></label>
			<label>Password : <input type="text" name="qw"></label>
			<input type="submit" value="Submit">
		</form>
		```
			
	* ##### a href=" "
		```html
		<a href="page.jsp">Move Next Page</a>
		```
	* ##### location.href=" "
		```html
		<input type="button" value="Move Next Page" onclick="location.href='page.jsp'">
		```
	
	


* ###### [JSP - 특정페이지로 이동방법(forward/redirect)]
* ###### [페이지 이동 방법, Foward, Redirect]


[JSP - 특정페이지로 이동방법(forward/redirect)]: https://installed.tistory.com/entry/8-JSP-%ED%8A%B9%EC%A0%95%ED%8E%98%EC%9D%B4%EC%A7%80%EB%A1%9C-%EC%9D%B4%EB%8F%99%EB%B0%A9%EB%B2%95
[페이지 이동 방법, Foward, Redirect]: https://m.blog.naver.com/PostView.nhn?blogId=tkddlf4209&logNo=220539737196&proxyReferer=https%3A%2F%2Fwww.google.com%2F