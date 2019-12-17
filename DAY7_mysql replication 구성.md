## Vgrant를 활용한 victural box centos7 서버에 mysql 클러스터 설치, 연동하기

## 1. virtual box 설치
## 2. vagrant 설치

### 3. c:사용자/work/vagrant/Vagrantfile을 다음과 같이 변경
```
#NODE1
Vagrant.configure("2") do |config|
  # Ansible-Node01
  config.vm.define:"node01" do |cfg|
  #config.ssh.username = "core"
    cfg.vm.box = "centos/7"
    cfg.vm.provider:virtualbox do |vb|
        vb.name="Node01"
        vb.customize ["modifyvm", :id, "--cpus", 1]
        vb.customize ["modifyvm", :id, "--memory", 1024]
    end
    cfg.vm.host_name="node01"
    cfg.vm.synced_folder "./data", "/home/vagrant/data"
    cfg.vm.network "public_network", ip: "192.168.56.11"
    #cfg.vm.network "hostonly", ip: "1.1.1.1"
    cfg.vm.network "private_network", ip: "1.1.1.1", virtualbox_internet: true
    cfg.vm.network "forwarded_port", guest: 22, host: 19211, auto_correct: false, id: "ssh"
    cfg.vm.network "forwarded_port", guest: 80, host: 10080
    cfg.vm.network "forwarded_port", guest: 3306, host: 13306
  end
end

#NODE2
Vagrant.configure("2") do |config|
  # Ansible-Node01
  config.vm.define:"node02" do |cfg|
    #config.ssh.username = "core"
    cfg.vm.box = "centos/7"
    cfg.vm.provider:virtualbox do |vb|
        vb.name="Node02"
        vb.customize ["modifyvm", :id, "--cpus", 1]
        vb.customize ["modifyvm", :id, "--memory", 1024]
    end
    cfg.vm.host_name="node02"
    cfg.vm.synced_folder "./data", "/home/vagrant/data"
    cfg.vm.network "public_network", ip: "192.168.56.12"
    #cfg.vm.network "hostonly", ip: "1.1.1.2"
    cfg.vm.network "private_network", ip: "1.1.1.2", virtualbox_internet: true
    cfg.vm.network "forwarded_port", guest: 22, host: 29211, auto_correct: false, id: "ssh"
    cfg.vm.network "forwarded_port", guest: 80, host: 20080
    cfg.vm.network "forwarded_port", guest: 3306, host: 23306
  end
end


```

### 4. 같은 위치에 아래 내용의 bash_ssh_conf.sh파일 생성

```
#! /usr/bin/env bash

now=$(date +"%m_%d_%Y")
cp /etc/ssh/sshd_config /etc/ssh/sshd_config_$now.backup
sed -i -e 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd
```

### 5. 윈도우 cmd 창에서 c:사용자/work/vagrant로 이동 후 아래 명령어 입력하면 virtual box에 vm두개 생김
```
$ vagrant up
```
>> 에러나면 23번 문제인지 확인 후 vagrant reload;

### 6. 각 vm에 아래 명령어로 원격접속해서 mysql설치함
```
$ vagrant ssh node01
$ sudo yum -y install http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
$ sudo yum -y install mysql-community-server
```

### 7. mysql서버 기동
```
$ sudo systemctl enable mysqld
$ sudo systemctl start mysqld
```

### 8. Mysql 로그인

```
$ sudo cat /var/log/mysqld.log 확인
    -> temporary password 확인 -> copy
$ mysql -uroot -p 
## 이러먼 root계정으로 mysql에 접속됨 exit; 하면 나가기
```

### 9. mysql 설정파일 변경 /etc/my.cnf 
>> node01, node02 모두
```
$ sudo vi /etc/cnf
```

#### 9.1 비밀번호 복잡도 
```
##password Policy

validate_password_policy=LOW
validate_password_length=4
validate_password_mixed_case_count=0
```

#### 9.2
모든 외부 IP에서 해당 mysql에 접속할 수있도록 설정
```
bind-address=0.0.0.0
```

### 10. mysql서버 재기동
>> node01, node02 모두
```
$ sudo systemctl restart mysql;
```
여기서 에러나먼 my.cnf파일 설정 다시보기

### 11. Mysql root 비번 변경
>> node01, node02 모두
```
mysql> alter user 'root'@'localhost' identified by '1111';
mysql> create user 'root'@'%' identified by '1111';
```

### 12. mysql 데이터베이스 생성
>> node01
``` sql
mysql> create database cloud_db default character set utf8;
mysql> create user 'clustermanager'@'1.1.1.2' identified by '1111';
mysql> grant all privileges on cloud_db.* to 'clustermanager'@'1.1.1.2' identified by '1111';
mysql> flush privileges;
```

>> node02
``` sql
mysql> create database cloud_db default character set utf8;
```

### 13. master/slave 설정 /etc/my.cnf에 추가하기

### 13.1 master 서버 (node01)
```
$ sudo vi /etc/my.cnf
```
아래 내용 추가
```
server-id=1
log_bin=mysql-log_bin
```

### 13.2 slave 서버 (node02)
```
$ sudo vi /etc/my.cnf
```
아래 내용 추가
```
server-id=2
replicate-do-db='cloud_db'
```
>> slave서버에 cloud_db데이터베이스가 안 생겨있으면 에러남!

### 14. mysql재시작
```
$ systemctl restart mysqld
$ ps -el | grep mysql
```

### 15. master 서버상태 확인(node01)
```sql
mysql>  show master status 

```
>>    -> file name, postion 확인 

### 16. master서버에서 slave에 replication 권한 계정 생성
```
mysql> grant replication slave on cloud_db.* to 'clustermanager'@'1.1.1.2' identified by '1111'
```

### 17. Slave DB 설정
```
mysql> create database cloud_db default character set utf8;
mysql> create user 'clustermanager1'@'%' identified by '1111';
(Mysql 5.7 version) mysql> grant all privileges on cloud_db.* to 'clustermanager1'@'%' identified by '1111';
(Mysql 8.* version) mysql> grant all privileges on cloud_db.* to 'clustermanager1'@'%' with grant option;

```

### 18. Slave DB에서 Master DB 사용에 대한 설정 (root로 로그인) 
```
change master to\
master_host='1.1.1.1',\
master_user='clustermanager',\
master_password='1111',\
master_log_file='mysql-bin.000002',\
master_log_pos=4967;
```

### 19.  Slave DB 재실행
```
$ systemctl restart mysqld (or Windows 서비스 재실행)
mysql> show slave status\G;
    - Slave_IO_Running: Yes
    - Slave_SQL_Running: Yes 

    ! Error>
            mysql> STOP SLAVE;
            mysql> SET GLOBAL SQL_SLAVE_SKIP_COUNTER=1;
            mysql> START SLAVE;
            mysql> show slave status \G;
```

### 20. master서버의 cloud_db덤프해서 slave의 cloud_db에 옮기기
>만약 master와 slave에 동일 database, table구성이 안 되어있으면 에러남!
>> node01서버에서
```
$ mysqldump -uroot -p cloud-db > cloud_db_master.sql
```
cloud_db_master.sql 파일을 node02서버에 옮김
>> node02서버에서 cloud_db_master.sql 덤프 풀기
```
mysql -uroot -p cloud-db < cloud_db_master.sql
```

### 21. Master에 clustermanager으로 로그인
```
mysql> use cloud_db;
mysql> create table , insert ...
```
데이터 업로드. 수정 등 해보고

### 22. Slave에 clustermanager1으로 로그인    

```
mysql> use cloud_db;
mysql> select ...
```
>> 자동으로 업데이트 되는거 확인하기.

## 23. 번외 vagrant Vagrantfile 공유폴더 설정
```
cfg.vm.synced_folder "./data", "/home/vagrant/data"
```
윈도우 ~/data폴더와 vm 서버 /home/vagrant/data/폴더가 공유됨!
윈도우 ~/data 폴더가 없다면 만들어야함!;
