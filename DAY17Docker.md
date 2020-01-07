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
* 이하 dc 는 docker의 Alias
```
$ docker image ls
$ docker image pull gihyodocker/echo:latest
$ dc run -p 9000:8080 gihyodocker/echo:latest
```

새 커멘드창에서
```
$ dc container ls

```
- 도커컨테이너 종료
```
$ dc stop [도커컨테이너이름or ID]

```
