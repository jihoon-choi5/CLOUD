## 1. nodejs 프로젝트 생성
	- myproject or firstmall..
```
  $ express --session --ejs --css stylus 프로젝트명
	$ cd 프로젝트명
	$ npm intall
	
  ```
	
	
	
	
## 2. nodejs프로젝트<->git연동
-연동
  ```
	$ git init, 
	$ git add .
	$ git remote add origin url
	$ git commit -m "aaa" 
	$ git push 
```
	
	
	
## 3. heroju에 app 생성(instance)
	- dashboard or CLI에서 생성
		)heroku apps:create ...
	- myherokuapp...
	
	heroku ps:scale web=0
	
## 4. heroku <-> git hub 연동
	- dashboard의 setting 메뉴
  ```
	heroku git:remote -a eunii
  ```
