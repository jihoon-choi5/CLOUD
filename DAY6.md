## 가상서버에 MySQL 설치하기
##

```
systemctl enable mysqld
vagrant

systemctl start mysqld

mysql -uroot -p 

## mySQL 비번 설정
$ vi /etc/my.cnf파일에 아래내용 추가
```
##password Policy
#
validate_password_policy=LOW
validate_password_length=4
validate_password_mixed_case_count=1
```


## cloud_db의 모든 테이블에 user1 접속 권한 허용하기
```
$ grant all privileges on cloud_db.* to 'user1'@'%' identified by '1111';
```

## 
$ flush privileges;


  
1. Mysql 설치
$ sudo yum -y install http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
$ sudo yum -y install mysql-community-server

2. Mysql 기동 
$ sudo systemctl enable mysqld
$ sudo systemctl start mysqld

3. Mysql 로그인
$ sudo cat /var/log/mysqld.log 확인
    -> temporary password 확인 -> copy
$ mysql -uroot -p 

4. Mysql root의 3 변경 
mysql> set global validate_password_policy=LOW;
mysql> alter user 'root'@'localhost' identified by 'test1234';

5. Mysql 데이터베이스 생성
mysql> create database cloud_db default character set utf8;
mysql> create user 'user1'@'%' identified by 'test1234';
mysql> grant all privileges on cloud_db.* to 'user1'@'%' identified by 'test1234';
FLUSH PRIVILEGES;
6. Mysql 접속 테스트
Linux $ mysql -uuser1 -p 
Windows c:\> mysql -uuser1 -p -h127.0.0.1 --port 13306

7. my.cnf 파일 수정
$ sudo vi /etc/my.cnf 수정 
    [mysqld]
    server-id=1
    log_bin=mysql-log_bin

8. Mysql 서버 재시작
$ systemctl restart mysqld
$ ps -el | grep mysql

9. Master DB 상태 확인 (root 권한 필요)
mysql>  show master status 
    -> file name, postion 확인 

10. Master DB에서 replication 계정 생성
mysql> grant replication slave on *.* to 'replicate_user'@'%' identified by 'test1234'

11. Slave DB 설정 (Windows에서 실행)
mysql> create database cloud_db default character set utf8;
mysql> create user 'user3'@'%' identified by 'test1234';
(Mysql 5.7 version) mysql> grant all privileges on cloud_db.* to 'user3'@'%' identified by 'test1234';
(Mysql 8.* version) mysql> grant all privileges on cloud_db.* to 'user3'@'%' with grant option;

12. Slave DB의 환경 설정 파일 수정 (my.cnf)
    - (Windows 경우) C:\ProgramData\MySQL\MySQL Server 8.0\my.ini
    - (Windwos 경우) Mysql 재실행 
        - Service 메뉴에서 MySQL80 재시작
    [mysqld]
    server-id=3
    replicate-do-db='cloud_db'

13. Slave DB에서 Master DB 사용에 대한 설정 (root로 로그인)        
change master to\
master_host='127.0.0.1',\
master_port=13306,\
master_user='replicate_user',\
master_password='1111',\
master_log_file='mysql-bin.000001',\
master_log_pos=859;


change master to\
master_host='192.168.56.11',\
master_port=3306,\
master_user='replicate_user',\
master_password='1111',\
master_log_file='mysql-log_bin.000001',\
master_log_pos=2470;

change master to master_host='127.0.0.1', master_port=13306, master_user='replicate_user', master_password='1111', master_log_file='mysql-log_bin.000005', master_log_pos=154;
14. Slave DB 재실행

15. show slave status\G;
