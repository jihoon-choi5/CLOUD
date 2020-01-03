## 1. Glance 설치 
[Cirros img다운로드 링크](http://download.cirros-cloud.net/0.3.5/)
http://download.cirros-cloud.net/0.3.5/cirros-0.3.5-x86_64-disk.img

yum install -y qemu-img
qemu-img info ciroos~~~
qemu-img convert -O vmdk cirros-0.3.5-x86_64-disk.img  cirros-0.3.5-x86_64-disk.vmdk
