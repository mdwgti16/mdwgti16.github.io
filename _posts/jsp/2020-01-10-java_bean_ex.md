---
title: Intellij JSP - JavaBean Example (자바빈 저장을 이용한 로그인 예제)
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-10 16:27:28 +0900'
categories: JSP
---

> ## JavaBean Example (자바빈 저장을 이용한 로그인 예제)



* ### Project - JavaBeanLoginEx
	* ##### User.java
		```java
		// 객체 직렬화를 위해선 Serializable를 구현해야 함
		public class User implements Serializable {
				String name;
				String id;
				String pw;
				String email;
				
				public User() {
				}

				public User(String name, String id, String pw, String email) {
						this.name = name;
						this.id = id;
						this.pw = pw;
						this.email = email;
				}

				// getter, setter 생략
		}
		```
		
	* ##### login.jsp
		```jsp
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<html>
		<head>
				<title>Title</title>
		</head>
		<body>
		<form action="login_process.jsp" method="post">
				<table>
						<tr>
								<td>ID :</td>
								<td><input type="text" name="id"></td>
						</tr>

						<tr>
								<td>Password :</td>
								<td><input type="password" name="pw"></td>
						</tr>
				</table>
				<input type="submit" value="로그인"> <input type="button" value="회원 가입" onclick="location.href='join.html'">
		</form>
		</body>
		</html>
		```
		
	* ##### join.html
		```html
		<!DOCTYPE html>
		<html lang="en">
		<head>
				<meta charset="UTF-8">
				<title>textform</title>
				<style>
						table {
								/*width: 100%;*/
								border: 1px solid #444444;
								margin-left: auto;
								margin-right: auto;
						}

						td {
								/*width: 100%;*/
								/*border: 1px solid #444444;*/
						}

						.c {
								text-align: center;
						}

				</style>
		</head>
		<body>

		<h2 class="c">회원 가입</h2>
		<form action="join_process.jsp" method="post" name="textform">
				<table>

						<tr>
								<td>이름<br/></td>
								<td><input type="text" name="name"></td>
						</tr>

						<tr>
								<td>ID :</td>
								<td><input type="text" name="id"></td>
						</tr>

						<tr>
								<td>Password :</td>
								<td><input type="password" name="pw"></td>
						</tr>

						<tr>
								<td>이메일<br/></td>
								<td><input type="text" name="email"></td>
						</tr>
				</table>
				<br/>
				<div class="c">
						<input type="submit" value="회원 가입">
						<input type="reset" value="초기화">
				</div>
		</form>
		</body>
		</html>
		```
		
	* ##### join_process.jsp
		```jsp
		<%@ page import="java.io.FileOutputStream" %>
		<%@ page import="java.io.ObjectOutputStream" %>
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<jsp:useBean id="user" class="beans.User" scope="page"/>
		<jsp:setProperty name="user" property="*"/>
		<html>
		<head>
		</head>
		<body>
		<%
				String realPath = application.getRealPath("/users/user_" + user.getId());
				FileOutputStream fos = new FileOutputStream(realPath);
				ObjectOutputStream oos = new ObjectOutputStream(fos);
				oos.writeObject(user);
				oos.close();
		%>

		<h2>회원 가입 처리 완료</h2><br/>
		이름 : <jsp:getProperty name="user" property="name"/><br/>
		ID : <jsp:getProperty name="user" property="id"/><br/>
		PW : <jsp:getProperty name="user" property="pw"/><br/>
		Email : <jsp:getProperty name="user" property="email"/><br/><br/>
		<button onclick="location.href='login.jsp'">Login Page</button>

		</body>
		</html>
		```
	
	* ##### login_process.jsp
		```jsp
		<%@ page import="beans.User" %>
		<%@ page import="java.io.FileInputStream" %>
		<%@ page import="java.io.ObjectInputStream" %><%--
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<%
				String id = request.getParameter("id");
				String pw = request.getParameter("pw");

				String storedName = null;
				String storedId = null;
				String storedPw = null;
				String storedEmail = null;

				String realPath = application.getRealPath("/users/user_" + id);
				try {
						FileInputStream fis = new FileInputStream(realPath);
						if (fis != null) {
								ObjectInputStream ois = new ObjectInputStream(fis);
								User mUser = (User) ois.readObject();
								storedName = mUser.getName();
								storedId = mUser.getId();
								storedPw = mUser.getPw();
								storedEmail = mUser.getEmail();
								System.out.println(storedId + storedPw + " " + id + pw);
								ois.close();
						}


						if (id.equals(storedId) && pw.equals(storedPw)) {
								System.out.println("IN");
								out.print("<html><head><title>로그인 처리</title></head><body>" +
												"<h2>가입한 정보입니다.</h2><br>" +
												"<hr><br>" +
												"이름 : " + storedName + "<br>" +
												"ID : " + storedId + "<br>" +
												"Email : " + storedEmail + "<br></body></html>");
						} else if (id.equals(storedId)) {
		%>
		<script>
				alert("비밀번호가 다릅니다.");
				history.back();
		</script>
		<%
		} else {
		%>
		<script>
				alert("일치하는 ID의 회원정보가 없습니다.");
				history.back();
		</script>
		<%
				} } catch (Exception e) {
				System.out.println(e.getMessage());
		%>
		<script>
				alert("일치하는 ID의 회원정보가 없습니다.");
				history.back();
		</script>
		<% } %>	
		```
	* ##### 로그인 화면
		![](/assets/img/jsp/javabean1.png)
	* ##### 회원가입 화면
		![](/assets/img/jsp/javabean2.png)
	* ##### 회원가입 완료
		![](/assets/img/jsp/javabean3.png)
	* ##### 로그인 완료
		![](/assets/img/jsp/javabean4.png)
	* ##### 로그인 실패 ( 비밀번호 불일치)
		![](/assets/img/jsp/javabean5.png)
	* ##### 로그인 실패 (없는 아이디)
		![](/assets/img/jsp/javabean6.png)
			

	
	
* ###### [Servlet을 포함한 JSP 2.1 웹 프로그래밍 입문에서 완성까지 ]

[Servlet을 포함한 JSP 2.1 웹 프로그래밍 입문에서 완성까지 ]: https://book.naver.com/bookdb/book_detail.nhn?bid=6468787