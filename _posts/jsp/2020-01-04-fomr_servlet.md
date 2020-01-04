---
title: Intellij JSP - Html Form 태그로 Servlet에 Parameter 넘기기
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-4 16:27:28 +0900'
categories: JSP
---

> ## Get/Post/Service 한글 깨짐 해결


* #### Project - FormTest
	* ##### rform.html
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
									border: 1px solid #444444;
							}

							.c {
									text-align: center;
							}
					</style>
			</head>
			<body>

			<p class="c"> 가입할 ID와 Password 및 자기소개를 입력하세요.</p>
			<form action="/form" method="post" name="textform">
					<table>
							<tr>
									<td>ID :</td>
									<td><input type="text" name="id"></td>
							</tr>
						
							<tr>
									<td>Password :</td>
									<td><input type="password" name="pw"></td>
							</tr>
						
							<tr>
									<td>자기소개<br/></td>
									<td><textarea name="desc" cols="50" fows="4"></textarea><br/></td>
							</tr>

							<tr>
									<td>소속국가</td>
									<td>
											<select name="na" size="3" multiple>
													<option selected>Korea</option>
													<option>USA</option>
													<option>Canada</option>
											</select>
									</td>
							</tr>

							<tr>
									<td>관심분야</td>
									<td><input type="checkbox" name="interested" value="엔터테인먼트">엔터테인먼트<br/>
											<input type="checkbox" name="interested" value="컴퓨터/인터넷">컴퓨터/인터넷<br/>
											<input type="checkbox" name="interested" value="경제/비지니스">경제/비지니스<br/>
											<input type="checkbox" name="interested" value="스포츠/건강">스포츠/건강<br/>
											<input type="checkbox" name="interested" value="여행/관광">여행/관광<br/>
									</td>
							</tr>
							<tr>
									<td>결혼여부</td>
									<td>
											<input type="radio" name="m_status" value="미혼" checked>미혼
											<input type="radio" name="m_status" value="기혼">기혼
									</td>
							</tr>

					</table>
					<br/>
					<div class="c">
							<input type="submit" value="전송" name="submitbtn">
							<input type="reset" value="초기화" name="resetbtn">
					</div>
			</form>
			</body>
			</html>
			
	* ##### RForm.java
		```java
		import javax.servlet.ServletException;
		import javax.servlet.annotation.WebServlet;
		import javax.servlet.http.HttpServlet;
		import javax.servlet.http.HttpServletRequest;
		import javax.servlet.http.HttpServletResponse;
		import java.io.IOException;
		import java.io.PrintWriter;

		@WebServlet(name = "RForm", value = "/form")
		public class RForm extends HttpServlet {
				protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
						request.setCharacterEncoding("utf-8");
						response.setContentType("text/html;charset=utf-8");
						String id = request.getParameter("id");
						String pw = request.getParameter("pw");
						String desc = request.getParameter("desc");
						String na = request.getParameter("na");

						String[] interested = request.getParameterValues("interested");
						String m_status = request.getParameter("m_status");

						PrintWriter out = response.getWriter();
						out.println("<title>form result</title>" +
										"당신이 입력한 정보입니다.<br>" +
										"<b>ID</b> : " + id + "<br>" +
										"<b>Password</b> : " + pw + "<br>" +
										"<b>자기소개</b><br>" +
										desc + "<br>" + "<br>" +

										"당신의 관심분야와 결혼여부는 다음과 같습니다.<br>"
						);
						for (String e : interested) {
								out.println(e + "<br>");
						}
						out.println(m_status + "<br>" + "<br>");
						out.println("당신의 소속국가는 다음과 같습니다.<br>" + na);
				}

				protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { }
		}
		```
		
* #### Parameter Names
	* ###### 이름 = id
	* ###### 비밀번호 = pw
	* ###### 자기소개 = desc
	* ###### 국가 = na
	* ###### 흥미 = interested
	* ###### 결혼상태 = m_status

* #### /rform.html

 ![](/assets/img/jsp/form_servlet1.png)
 
* #### /form
	
 ![](/assets/img/jsp/form_servlet2.png)
	 
* ###### [신입SW인력을 위한 실전 JSP Servlet]


[신입SW인력을 위한 실전 JSP Servlet]: https://www.youtube.com/watch?v=2Pqi-kUMwtw&list=PLieE0qnqO2kTyzAlsvxzoulHVISvO8zA9&index=39