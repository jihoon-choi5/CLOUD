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

mysql -u root -p
CREATE DATABASE keystone;

MariaDB [(none)]> GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'localhost' \
IDENTIFIED BY 'KEYSTONE_DBPASS';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON keystone.* TO 'keystone'@'%' \
IDENTIFIED BY 'KEYSTONE_DBPASS';


yum install openstack-keystone httpd mod_wsgi

vi /etc/keystone/keystone.conf

```
[database]
# ...
connection = mysql+pymysql://keystone:KEYSTONE_DBPASS@controller/keystone

[token]
# ...
provider = fernet
```

su -s /bin/sh -c "keystone-manage db_sync" keystone
keystone-manage fernet_setup --keystone-user keystone --keystone-group keystone
keystone-manage credential_setup --keystone-user keystone --keystone-group keystone

```
keystone-manage bootstrap --bootstrap-password ADMIN_PASS \
  --bootstrap-admin-url http://controller:5000/v3/ \
  --bootstrap-internal-url http://controller:5000/v3/ \
  --bootstrap-public-url http://controller:5000/v3/ \
  --bootstrap-region-id RegionOne
  
```
  
vi /etc/httpd/conf/httpd.conf
```
ServerName controller
```
ln -s /usr/share/keystone/wsgi-keystone.conf /etc/httpd/conf.d/

$ export OS_USERNAME=admin
$ export OS_PASSWORD=ADMIN_PASS
$ export OS_PROJECT_NAME=admin
$ export OS_USER_DOMAIN_NAME=Default
$ export OS_PROJECT_DOMAIN_NAME=Default
$ export OS_AUTH_URL=http://controller:5000/v3
$ export OS_IDENTITY_API_VERSION=3


openstack domain create --description "An Example Domain" example

**중요**
openstack project create --domain default \
  --description "Service Project" service
  
 openstack project create --domain default \
  --description "Demo Project" myproject
  
  openstack user create --domain default \
  --password 1111 myuser
