---
title: Intellij JSP - File Upload (파일 업로드)
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-13 14:27:28 +0900'
categories: JSP
---

> ## > File Upload (파일 업로드)


* ### com.oreilly.servlet 라이브러리 다운로드
	* ##### [cos](http://www.servlets.com/cos/) 에서 라이브러리를 다운 받고 압축을 푼다
		![](/assets/img/jsp/file_upload1.png)
		
* ### Intellij에 라이브러리 추가
	* ##### Project Structure (Shift + Ctrl + Alt + S) -> Libraries -> New Project Library -> Java
		![](/assets/img/jsp/file_upload2.png)
	* ##### 다운 받은 cos 라이브러리의 cos 폴더/lib/cos.jar 파일을 추가
		![](/assets/img/jsp/file_upload3.png)
		
* ### File Upload Example
	* ##### file_upload.html
		```html
		<!DOCTYPE html>
		<html lang="en">
		<head>
				<meta charset="UTF-8">
				<title>File Upload</title>
		</head>
		<body>
		<form action="file_upload.jsp" enctype="multipart/form-data" method="post">
				파일 : <input type="file" name="upfile"><br/>
				<input type="submit" value="Upload">
		</form>

		</body>
		</html>	
		```
	* ##### file_upload.jsp
		```jsp
		<%@ page import="com.oreilly.servlet.MultipartRequest" %>
		<%@ page import="com.oreilly.servlet.multipart.DefaultFileRenamePolicy" %>
		<%@ page import="java.util.Enumeration" %>
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<%
				String savePath = application.getRealPath("./");
				String file = "";
				String oriFile = "";
				int sizeLimit = 5 * 1024 * 1024;
				System.out.println(savePath);

				MultipartRequest multi = new MultipartRequest(request, savePath, sizeLimit, "UTF-8", new DefaultFileRenamePolicy());
				Enumeration files = multi.getFileNames();
				String name = (String) files.nextElement();

				file = multi.getFilesystemName(name);
				oriFile = multi.getOriginalFileName(name);
		%>

		<html>
		<head>
				<title>File Upload</title>
		</head>
		<body>
		<h2>파일 업로드 성공</h2><br/>
		파일 저장 위치 : <%= savePath%><br/>
		파일 저장 이름 : <%= file%><br/>
		파일 원본 이름 : <%= oriFile%>
		</body>
		</html>
		```

	* ##### 파일 업로드 결과
		![](/assets/img/jsp/file_upload4.png)
		
* ###### [신입SW인력을 위한 실전 JSP Servlet]


[신입SW인력을 위한 실전 JSP Servlet]: https://www.youtube.com/watch?v=5kgThHLRb_k&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=56