## 1. 버추얼박스 6.0버전 설치
## 2. VMware player
## 3. vagrant 설치

### 3.1 폴더 들고 들어가서 vgrant init
### 3.2 폴더 들어가서 vagrantfile 내용 아래와 같이 변경
    
```  
Vagrant.configure("2") do |config|
  # Ansible-Node01
  config.vm.define:"node01" do |cfg|
    cfg.vm.box = "centos/7"
    cfg.vm.provider:virtualbox do |vb|
        vb.name="Node01"
        vb.customize ["modifyvm", :id, "--cpus", 1]
        vb.customize ["modifyvm", :id, "--memory", 1024]
    end
    cfg.vm.host_name="node01"
    cfg.vm.network "public_network", ip: "172.20.10.11"
    cfg.vm.network "forwarded_port", guest: 22, host: 19211, auto_correct: false, id: "ssh"
    cfg.vm.network "forwarded_port", guest: 80, host: 10080
  end
end

```
### 3.3 같은 위치에 아래 내용의 bash_ssh_conf.sh 파일 생성
  
 
```
  #! /usr/bin/env bash

now=$(date +"%m_%d_%Y")
cp /etc/ssh/sshd_config /etc/ssh/sshd_config_$now.backup
sed -i -e 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd
```

### 3.4 아래 명령어 입력
    
```
    $ vagrant plugin install vagrant-vbguest
    $ vagrant up
    # 설치 다 되면
    $ vgrant ssh
    #로 접속해서 젒속되는지 확인
```
