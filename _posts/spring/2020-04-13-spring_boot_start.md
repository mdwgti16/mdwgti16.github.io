---
title: Intellij Spring Boot - 프로젝트 생성 (1)
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-4-13 22:27:28 +0900'
categories:
- Spring Boot
---

> ## Spring Boot - 프로젝트 생성 (1)

* ### Create Project 
	* ##### [Spring Initializr](http://start.spring.io/) 접속
	* ##### 프로젝트 설정후 생성
		![](/assets/img/spring_boot/start1.png)
	* ##### 프로젝트 임포트
		![](/assets/img/spring_boot/start2.png)
* ### 프로젝트 구조
	* ##### pom.xml
		```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		```
##### spring-boot-starter-web : RESTful, SpringMVC, Tomcat을 포함하고 있음
	* ##### DemoApplication.java
		```java
		@SpringBootApplication
		public class DemoApplication {

			public static void main(String[] args) {
				SpringApplication.run(DemoApplication.class, args);
			}

		}		
		```
##### @SpringBootApplication : @EnableAutoConfiguration, @ComponentScan, @Configuration 을 포함하고 있는 어노테이션

* ### Run
	* ##### /src/main/resources/index.html 추가
		```html
		<!DOCTYPE html>
		<html lang="en">
		<head>
				<meta charset="UTF-8">
				<title>Home</title>
		</head>
		<body>
		Home
		</body>
		</html>		
		```
	* ##### Run Application
	 ![](/assets/img/spring_boot/start3.png)
	* ##### 결과 화면
		![](/assets/img/spring_boot/start4.png)
	
		
		
		
> ###### [스프링부트 시작하기 (SpringBoot 프로젝트 설정 방법)]
> ###### [스프링부트(SpringBoot) @SpringBootApplication]


[스프링부트 시작하기 (SpringBoot 프로젝트 설정 방법)]: https://goddaehee.tistory.com/238?category=367461
[스프링부트(SpringBoot) @SpringBootApplication]: https://m.blog.naver.com/PostView.nhn?blogId=ish430&logNo=221340243322&proxyReferer=https:%2F%2Fwww.google.com%2F