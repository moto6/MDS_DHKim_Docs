프로세스의 상태천이
sleep -> Weakup -> Ready -> Run

펨웨어 레벨은 가상 주소가 없었어요
디바이스 드라이버는 특정 주소로 다이렉트 접근 불가능- 커널 패닉상태로 빠짐
리눅스 커널 메모리 보호기능, 특정주소를 다이렉트로 접근 불가능 에러발생
OS야 1:1 맵핑된 주소를 알려주겠니???? 


이번주 디바이스 드라이버는 위 아래를 모두 아우를 수 있기 때문에
디바이스 드라이버는 커널에 속해있기 때문에 
```shell
root@ubuntu-vm /
# ls
bin    dev   initrd.img  media  proc  selinux  tmp  vmlinuz
boot   etc   lib         mnt    root  srv      usr
cdrom  home  lost+found  opt    sbin  sys      var
root@ubuntu-vm /
# cd m
media/ mnt/   
root@ubuntu-vm /
# cd m
media/ mnt/   
root@ubuntu-vm /
# cd mnt/hgfs/Data/
root@ubuntu-vm /mnt/hgfs/Data
# ls
arm-2010q1-202-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2
kernel-mds2450-3.0.22-20140306.tar.gz
rootfs_20140218.tar.gz
zImage
root@ubuntu-vm /mnt/hgfs/Data
# cp arm-2010q1-202-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2 /root
root@ubuntu-vm /mnt/hgfs/Data
# cp kernel-mds2450-3.0.22-20140306.tar.gz /root
root@ubuntu-vm /mnt/hgfs/Data
# cp rootfs_20140218.tar.gz /root
root@ubuntu-vm /mnt/hgfs/Data
# ^C
root@ubuntu-vm /mnt/hgfs/Data
# ^C
root@ubuntu-vm /mnt/hgfs/Data
# 
root@ubuntu-vm ~
# ls
arm-2010q1                                                       문서
arm-2010q1-202-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2  바탕화면
kernel-mds2450-3.0.22-20140306.tar.gz                            비디오
rootfs_20140218.tar.gz                                           사진
공개                                                             음악
다운로드                                                         템플릿
root@ubuntu-vm ~
# tar xvjf arm-2010q1-202-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2 
```