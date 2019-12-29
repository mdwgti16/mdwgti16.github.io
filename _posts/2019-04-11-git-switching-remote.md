---
title: Git - Remote URL 변경
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-04-11 14:26:28 +0900'
categories: Git
---

> ## 원격 주소  변경

#### 1. 이전 remote 주소 확인
* ```ruby
	$ git remote -v
	> origin  https://github.com/USERNAME/REPOSITORY.git (fetch)
	> origin  https://github.com/USERNAME/REPOSITORY.git (push)
	```

#### 2.  새로운 주소로 변경
* ```ruby
	$ git remote set-url origin https://github.com/NewNAME/NewREPOSITORY.git (fetch)
	```
	
#### 3.  remote 주소 확인
* ```ruby
	$ git remote -v
	# Verify new remote URL
	> origin  https://github.com/NewNAME/NewREPOSITORY.git (fetch) (fetch)
	> origin  https://github.com/NewNAME/NewREPOSITORY.git (fetch) (push)
```