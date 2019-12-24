cd /etc/yum.repos.d/
yum search docker >>원하는 프로그램패키지 있는지 확인

## yum repository 추가 방법 >> 하고 yum list해야함

### 방법1
 mkdir /pkg
  436  umount /dev/sr0
  437  mount /dev/sr0 /pkg
  438  df -h /pkg
  439  yum-config-manager --add-repo="레보지토리 URL"
  440  pwd
  441  ls
  442  cat pkg.repo
  443  yum list
  

### 방법2
cd /etc/yum.repos.d/
여기다가 xxxx.repo 파일 만들어서 아래 형식으로 내용 넣기

```

  2 [pkg]
  3 name=added from: file:///pkg
  4 baseurl=file:///pkg
  5 enabled=1
  6
~

```

### 방법3 package로 받기(yum install)




grep -B 패턴 5 패턴 들어간 전5줄봄
grep -A 패턴 5 >> 패턴 들어간 뒤 5줄 봄

------
#우분투
### 
apt-cache search
apt-cache show

apt-get install/remove/upgrade



iptables -L
iptables -F >>룰셋 제거 해보기

## daemon 형 프로세스(서비스) >> 메모리에 상주하는 서비스

inetd - telnet
## server 형 프로세스(서비스) >>

## job Scheduling 
1. cron: 주기적인 작업처리
2. at: 한번 예약해서
3. batch: 특정시간 예약할 수없고, CPU가 한가할때


## ssh 공개키 개인키 만들기
ssh-keygen

ssh-copy-id student@172.20.0.129

ssh student@172.20.0.129
하면 인증 없이 접속 가능
