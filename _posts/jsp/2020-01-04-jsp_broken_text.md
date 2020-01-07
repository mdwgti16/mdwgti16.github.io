---
title: Intellij JSP - Get/Post/Service 한글 깨짐 해결
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-4 15:27:28 +0900'
categories: JSP
---

> ## Get/Post/Service 한글 깨짐 해결


* #### html/JSP -> JSP
	* ##### Get 방식
	1. ##### page contentType 부분에 charset=UTF-8 추가
		```jsp
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		```
	2. ##### 스크립틀릿에서 charset을 UTF-8로 지정
		```jsp
		<% response.setCharacterEncoding("utf-8"); %>
		```
		
	* ##### Post 방식
	1. ##### 지시자와 스크립틀릿 사용
		```jsp
		<%@ page contentType="text/html;charset=UTF-8" language="java" %> <%--charset=UTF-8 추가--%>
		...
		<% response.setCharacterEncoding("utf-8"); %>
		```
	2. ##### 스크립틀릿만 사용
		```jsp
		<%
			request.setCharacterEncoding("utf-8");
			response.setCharacterEncoding("utf-8");
		%>
		```
		
* #### html/JSP -> Servlet
	* ##### Get 방식
	```java
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
	// 추가
	response.setContentType("text/html;charset=utf-8"); 
	...
	}
	```
	* ##### Post 방식
	```java
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
	// 추가
	request.setCharacterEncoding("utf-8");
	response.setContentType("text/html;charset=utf-8");
	...
	}
	```
	
* #### Service
	* ##### Get 방식
	```java
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
			super.service(req, resp);
			resp.setContentType("text/html;charset=utf-8");
	...
	}
	```
	* ##### Post 방식
	```java
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
			super.service(req, resp);
			req.setCharacterEncoding("utf-8");
			resp.setContentType("text/html;charset=utf-8");
	...
	}
	```
	 
* ###### [서블릿 주요 메서드와 한글 깨짐처리]


[서블릿 주요 메서드와 한글 깨짐처리]: https://kyun2.tistory.com/42