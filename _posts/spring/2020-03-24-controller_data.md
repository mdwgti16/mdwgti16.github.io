---
title: Intellij Spring - Controller의 데이터 처리
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-3-24 10:27:28 +0900'
categories: Spring
---

> ## Controller의 데이터 처리

<details markdown="1">
<summary><font size="4px" >User.java</font></summary>
```java
public class User {
    private String id;
    private String password;
    private String name;
    private String email;

    public User() {
    }

    public User(String id, String password, String name, String email) {
        this.id = id;
        this.password = password;
        this.name = name;
        this.email = email;
    }

    public String getId() {
        return id;
    }

    public String getPassword() {
        return password;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }

    public void setId(String id) {
        this.id = id;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "User{" +
                "id='" + id + '\'' +
                ", password='" + password + '\'' +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}
```
</details>

<details markdown="1">
<summary><font size="4px" >home.jsp</font></summary>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```xml
<html>
<head>
    <title>Data 처리</title>
</head>
<body>
<form action="/value">
    <label>아이디 : <input type="text" name="id"></label>
    <label>비밀번호 : <input type="password" name="password"></label>
    <label>이름 : <input type="text" name="name"></label>
    <label>이메일 : <input type="text" name="email"></label>
    <input type="submit" value="전송">
</form>

</body>
</html>
```
</details>



* ### HttpServletRequest 
	* ##### HomeController.java
		```java
		@Controller
		public class HomeController {
				...
				
				@RequestMapping("/HttpServletRequest")
				public String getData(HttpServletRequest request) {
						String id = request.getParameter("id");
						String password = request.getParameter("password");
						String name = request.getParameter("name");
						String email = request.getParameter("email");
						User user = new User(id,password,name,email);
						System.out.println(user.toString());
						return "home";
				}
		}
		```

* ### RequestParam 
	* ##### HomeController.java
		```java
		@Controller
		public class HomeController {
				...
				
				@RequestMapping("/requestParam")
				public String getData(@RequestParam("id") String id,
								 @RequestParam("password") String password,
								 @RequestParam("name") String name,
								 @RequestParam("email") String email) {
						User user = new User(id,password,name,email);
						System.out.println(user.toString());
						return "home";
				}
		}
		```

* ### Command Object 
	* ##### HomeController.java
		```java
		@Controller
		public class HomeController {
				...
				
				@RequestMapping("/commandObject")
				public String getData(User user) {
						System.out.println(user.toString());
						return "command";
				}
		}
		```
	* ##### command.jsp
		```xml
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<html>
		<head>
				<title>Data 처리</title>
		</head>
		<body>
		${user.id}<br/>
		${user.password}<br/>
		${user.name}<br/>
		${user.email}<br/>
		</body>
		</html>		
		```
###### 커맨드 객체를 이용하면 파라미터를 커맨드 객체로 바로 받을 수 있고 view에 넘길때 model에 넣지 않아도 자동으로 전달된다.
		
* ### ModelAttribute
	* ##### view에서 사용할 커맨드 객체 이름 변경
		```java
		@Controller
		public class HomeController {
				...
				
				@RequestMapping("/commandObject")
				public String getData(@ModelAttribute("u") User user) {
						System.out.println(user.toString());
						return "command";
				}
		}
		```
		
		<details markdown="1">
		<summary><font size="4px" >command.jsp</font></summary>
		```xml
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<html>
		<head>
				<title>Data 처리</title>
		</head>
		<body>
		${u.id}<br/>
		${u.password}<br/>
		${u.name}<br/>
		${u.email}<br/>
		</body>
		</html>		
		```
		</details>
		
	* ##### view에서 사용 가능한 메소드 지정
		```java
		@Controller
		public class HomeController {
				...
				
				@ModelAttribute("serverTime")
				public String getDate(){
						SimpleDateFormat sf = new SimpleDateFormat ( "yyyy년 MM월dd일 HH시mm분ss초");
						return sf.format(System.currentTimeMillis());
				}
				
				@RequestMapping("/commandObject")
				public String getData(@ModelAttribute("u") User user) {
						System.out.println(user.toString());
						return "command";
				}
		}
		```
		<details markdown="1">
		<summary><font size="4px" >command.jsp</font></summary>
		```xml
		<%@ page contentType="text/html;charset=UTF-8" language="java" %>
		<html>
		<head>
				<title>Data 처리</title>
		</head>
		<body>
		${u.id}<br/>
		${u.password}<br/>
		${u.name}<br/>
		${u.email}<br/>
		${serverTime}
		</body>
		</html>	
		```
		</details>
		

> ###### [자바 스프링 프레임워크(renew ver.) - 신입 프로그래머를 위한 강좌]


[자바 스프링 프레임워크(renew ver.) - 신입 프로그래머를 위한 강좌]: https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew/