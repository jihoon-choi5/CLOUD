## 1. 윈도우에 도커 설치
1. [여기서](https://hub.docker.com/) 회원가입 후 도커 다운로드 한다
2. setting 에 shard c드라이브 클릭 . apply
3. sing in 
$ docker login
4. powershell사용 추천

## 2. 파워쉘에서 작업
$ docker login
$ docker version

## 3. 도커 기본
- 도커 이미지 : 도커 컨테이너를 구성하는 탬플릿
- 도커 컨테이너 : 도커 이미지를 기반으로 생성, 파일시스템과 애플리케이션이 구체화 되어있는 상태

## 4. 도커 기본 실습
> 도커/쿠버네티스를 활용한 컨테이너 개발 실전 입문 예제
* 이하 doc 는 docker의 Alias
```
$ docker image ls
$ docker image pull gihyodocker/echo:latest
$ doc run --name myweb1 -d -p 9000:8080 gihyodocker/echo:latest
```
- -d: 데몬
- -p:포트
- --rm: 컨테이너 stop하면 컨테이너 자동으로 삭제
새 커멘드창에서
```
$ doc container ls

```
- 도커컨테이너 종료
```
$ doc stop [도커컨테이너이름or ID]

```
- 전현재 모두 프로세스 보기
```
$ doc ps -a
```
- 컨테이너 삭제xc
```
$ doc container rm [ID]
$ doc stop [도커컨테이너이름or ID] && doc rm [ID]
전체삭제
$ doc container prune
$ doc container stop $(docker ps -q)
$ doc container rm $(docker ps -qa)
```

- 로그 보기
```
$ doc logs [컨테이너ID]

```
# 실습
## 1.간단한 웹페이지 생성(Docker없이 구동)
prerequest nodejs 설치
/myweb폴더생성
폴더밑에 **package.json**, index.js파일 만들어서 아래 내용 입력
- package.json
```json
{
    "dependencies": {
        "express": "*"
    },
    "scripts": {
        "start": "node index.js"
    }
}

```
- index.js
```scirpt
const express = require('express');
const app = express();

app.get('/', (req, res)=>{
    res.send("Hi, there!")
});

app.listen(8080, () => {
    console.log("Listening on port 8080")
});

```

```
npm install
npm start
```
브라우저에 http://localhost:8080접속해보기

## 2. Docker로 간단한 웹페이지 생성

### 2.1 Dockerfile생성(아래 내용)
```
FROM alpine
RUN npm install
CMD ["npm", "start"]

```

### FROM >node 사용가능한 이미지
### run -> npm install 실행
### cmd -> npm start

```
$ docker image build --no-cache -t test/simpleweb:latest .
$ docker run -d test/simpleweb:latest

$ docker push test/simpleweb:latest
$ docker pull eoe04/simpleweb:latest
$ docker exec -it web(컨테이너이름) hostname
$ docker exec -it web(컨테이너이름) sh

$ docker search --limit 5 mysql
