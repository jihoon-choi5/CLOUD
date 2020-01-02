## OpenStack CLI명령어로 관리
$ cd
$ source keystonerc_admim
or
$ . cp keystonerc_admim
이렇게 하면

$openstack ~명령어 가능

:%s/export/unset


## 수동 설치 가이드
[사이트](https://docs.openstack.org/install-guide)


[수동설치](https://docs.openstack.org/install-guide/environment-packages-rdo.html)  

$ openssl rand -hex 10

yum install -y chrony

chronyc sources

yum install centos-release-openstack-rocky
yum upgrade

## CLI 도구  설치
 yum install python-openstackclient
 yum install openstack-selinux
 
 ## DB설치
 - mysal, mariadb ...
 
  yum install mariadb mariadb-server python2-PyMySQL
 
 vi /etc/my.cnf.d/openstack.cnf
 저파일에 아래 내용 입력
 
 
 ```
 [mysqld]
bind-address = 10.0.0.11

default-storage-engine = innodb
innodb_file_per_table = on
max_connections = 4096
collation-server = utf8_general_ci
character-set-server = utf8
```

systemctl enable mariadb.service
systemctl start mariadb.service
### 보안설정
mysql_secure_installation

## Message queue

RabbitMQ, Qpid, and ZeroMQ.
#### RabbitMQ 설치
yum install rabbitmq-server
systemctl enable rabbitmq-server.service
systemctl start rabbitmq-server.service
#### 사용자 추가
rabbitmqctl add_user openstack RABBIT_PASS
#### 사용자 권한 추가
rabbitmqctl set_permissions openstack ".*" ".*" ".*"

## Memcached

yum install memcached python-memcached
vi /etc/sysconfig/memcached
아래 내용 수정
```
OPTIONS="-l 127.0.0.1,::1,controller"
```

systemctl enable memcached.service
systemctl start memcached.service

## KeyStone설치
[참조링크](https://docs.openstack.org/keystone/rocky/install/)
