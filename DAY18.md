###

## 도커 명령어
- 컨테이너 상세 정보(?)
-  docker inspect [컨데이너아이디]

- docker network create mongo-cluster

- docker-compose up

- docker run --name mysql -e MYSQL_ROOT_PASSWORD=1111 -d mysqlmaster

docker swarm init
docker swarm leave --force

docker node ls


##로컬레지스트리 등록!!
docker pull busybox
docker tag busybox:latest localhost:5000/busybox:latest
docker push localhost:5000/busybox:latest
docker pull registry:5000/busybox:latest

