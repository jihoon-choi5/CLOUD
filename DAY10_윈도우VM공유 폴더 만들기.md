##윈도우 서버 Vm서버 공유폴더 만들기
1 윈도우에 공유할 폴더 만들기
2. vmware setting - option - 공유폴더 경로 설정
3. vm 에서 다음 명령어 입력
vmhgfs-fuse /mnt
cd mnt
cd 공유 폴더명
$ mount -o loot 파일명  /centos
