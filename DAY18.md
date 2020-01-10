###

## 도커 명령어
- 컨테이너 상세 정보(?)
-  docker inspect [컨데이너아이디]

- docker network create mongo-cluster

- docker-compose up

- docker run --name mysql -e MYSQL_ROOT_PASSWORD=1111 -d mysqlmaster

## 도커 swarm 구성

docker swarm init
docker swarm leave --force

docker node ls


## 로컬레지스트리 등록!!
docker pull busybox
docker tag busybox:latest localhost:5000/busybox:latest
docker push localhost:5000/busybox:latest
docker pull registry:5000/busybox:latest

apt-get update
apt-get install -y vim


https://github.com/PYC1080/TIL/blob/master/docker/%EB%8F%84%EC%BB%A4%2C%20%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4%202%EC%9D%BC%EC%B0%A8.md
## 도커 swarm 상태일때
docker service ~명령어 사용가능

docker stack deploy -c "file path" [stack name]
