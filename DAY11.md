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
