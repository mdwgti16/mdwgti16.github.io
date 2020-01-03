---
title: JSP - Tomcat 로그 글자 깨짐 해결 및 영어로 변경
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-3 15:27:28 +0900'
categories: JSP
---

> ## Tomcat 로그 글자 깨짐 해결 및 영어로 변경

### 1 Tomcat 구동시 한글 로그 깨짐 해결
* ##### Tomcat 설치 폴더/conf/logging.properties에서 아래와 같이 주석처리를 한다
		...
	
		java.util.logging.ConsoleHandler.level = FINE
		java.util.logging.ConsoleHandler.formatter = org.apache.juli.OneLineFormatter
		# java.util.logging.ConsoleHandler.encoding = UTF-8 
	
* ##### Tomcat 로그가 제대로 출력되는 것을 볼수 있다

		03-Jan-2020 15:53:19.337 정보 [main] org.apache.catalina.startup.VersionLoggerListener.log 서버 버전 이름:        Apache Tomcat/9.0.30
		03-Jan-2020 15:53:19.339 정보 [main] org.apache.catalina.startup.VersionLoggerListener.log Server 빌드 시각:          Dec 7 2019 16:42:04 UTC
		03-Jan-2020 15:53:19.339 정보 [main] org.apache.catalina.startup.VersionLoggerListener.log Server 버전 번호:         9.0.30.0
		03-Jan-2020 15:53:19.339 정보 [main] org.apache.catalina.startup.VersionLoggerListener.log 운영체제 이름:               Windows 10
		03-Jan-2020 15:53:19.339 정보 [main] org.apache.catalina.startup.VersionLoggerListener.log 운영체제 버전:            10.0
		03-Jan-2020 15:53:19.339 정보 [main] org.apache.catalina.startup.VersionLoggerListener.log 아키텍처:          amd64
		...
			
### 2 Tomcat 로그 영어로 바꾸기
1. #### 프로젝트에 따로 적용
* ###### Edit Configurations ->  VM  options 에 아래를 추가
			-Duser.language=en -Duser.region=US
			
2. #### Tomcat catalina.out 파일 수정
* ###### Tomcat  설치 폴더/bin/catalina.bat에 아래를 추가
			set "JAVA_OPTS=%JAVA_OPTS% -Duser.language=en"
			
* ##### Tomcat 로그가 영어로 출력 된다
		03-Jan-2020 16:12:03.001 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version name:   Apache Tomcat/9.0.30
		03-Jan-2020 16:12:03.007 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built:          Dec 7 2019 16:42:04 UTC
		03-Jan-2020 16:12:03.007 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version number: 9.0.30.0
		03-Jan-2020 16:12:03.007 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name:               Windows 10
		03-Jan-2020 16:12:03.007 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Version:            10.0
		03-Jan-2020 16:12:03.007 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Architecture:          amd64
			


* ###### [톰캣 로그 언어 영어로 바꾸기(이클립스) - tomcat language change]	
* ###### [Tomcat 로그를 한글 대신 영문으로 남기기]

[톰캣 로그 언어 영어로 바꾸기(이클립스) - tomcat language change]: https://nhj12311.tistory.com/61
[Tomcat 로그를 한글 대신 영문으로 남기기]: https://bigboss.io/2019/08/tomcat-log-in-english/