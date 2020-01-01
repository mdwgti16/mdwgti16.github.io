---
title: Git - 취소, 되돌리기
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-03-25 08:26:28 +0900'
categories: Git
---

### Unstaged files  수정 되돌리기
* #### repository 내 모든 수정 되돌리기
	``` ruby
	$ cd {repository_root_dir}
	$ git checkout . 
	```
	
* #### 특정 폴더 아래의 모든 수정 되돌리기
	``` ruby
	$ git checkout {dir}
	```

* #### 특정 파일의 수정 되돌리기
	``` ruby
	$ git checkout {file_name}
	```
	
### Git add 취소
* ``` ruby
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
	renamed:    activity_main.xml -> main.xml
	modified:   MainActivity.kt
```

* #### 이때, git reset HEAD [file] 명령어를 통해 git add를 취소할 수 있습니다.
* #### 파일명이 없을 경우 전체 취소

* ``` ruby
	$ git reset HEAD MainActivity.kt
	Unstaged changes after reset:
	M	MainActivity.kt

	$ git status
	On branch master
	Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)
		renamed:    activity_main.xml -> main.xml
	Changes not staged for commit:
	(use "git add <file>..." to update what will be committed)
	(use "git checkout -- <file>..." to discard changes in working directory)
		modified:   MainActivity.kt
	```

### Git commit 취소
* #### commit 내용을 없애고 이전 상태로 원복 (--hard)
* ```ruby
	$ git reset --hard HEAD^
	```

* ####  index 전단계 add하기 전 상태, unstaged 상태 (--mixed)
*  ```ruby
	 $ git reset HEAD^
	 ```
		 
* #### index 보존 add한 상태  staged 상태  (--soft)
* ```ruby
	$ git reset --soft HEAD^
	```

### Git push 취소 
* ```ruby
	$ git reset HEAD^  #local repository에서 commit을 하나 되돌림
	$ git commit -m "..."  #되돌린 것으로 commit
	$ git push origin +master #remote repository를 강제로 revert
	```

#### 참조
##### [Git - 수정한 것 되돌리기]
##### [git add 취소하기, git commit 취소하기, git push 취소하기] 

[git add 취소하기, git commit 취소하기, git push 취소하기]: https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html
[Git - 수정한 것 되돌리기]:http://hochulshin.com/git-revert-changes/