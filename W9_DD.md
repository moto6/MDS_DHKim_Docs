 # Week9 - Linux Device Driver
   #### 들어가기 전에 : 이 글은 제가 학습한 내용을 "이렇게 이해했습니다" 로 정리하는 문서이지 강좌글이 아닙니다. 
 > 목차
 >> - Day1 : 
 >> - Day2
 >> - Day3
 >> - Day4
 >> - Day5
 #
 ## Day1    
  - 프로세스의 상태천이 : sleep -> Weakup -> Ready -> Run
  - 펨웨어 레벨은 가상 주소가 없었어요
  - 디바이스 드라이버는 특정 주소로 다이렉트 접근 불가능- 커널 패닉상태로 빠짐
  - 리눅스 커널 메모리 보호기능, 특정주소를 다이렉트로 접근 불가능 에러발생
  - OS야 1:1 맵핑된 주소를 알려주겠니???? 
  - 이번주 디바이스 드라이버는 위 아래를 모두 아우를 수 있기 때문에
  - 디바이스 드라이버는 커널에 속해있기 때문에 커널 컴파일을 해야 함
  - 루트파일시스템은 : /root의 특정 Path로 이동, 압축풀기 옵션
      - xvzf : tar파일
      - xvjf : bin파일
  - 디바이스 드라이버에 대한 간단 특징들 
     - 내용이 매우 방대함 - 뒷부분 못나갈수 있음
     - 구인이 많은부분 - 일이 빡세긴 한데 잘하면 인정 받을 수 있음. 거의 모든 임베디드 분야에 통용되는 곳임
     - 예를들어 사람의 마음을 찍는 프로젝트의 경우, hw->os포팅->dd구현의 순서로 프로젝트가 진행 됨
     - 리눅스 프로젝트의 프레임 혹은 틀임. 많이 만들면 만들수록 실력이 향상 됨
     - 함수포인터 변수와 void 형 포인터 변수 등판!
     - 상위 영역(App영역, User영역)에서는 하드웨어를 파일로 추상화하여 바라 봄
     - File의 동작에 관하여 4가지 동작
       - Open, Read, Write, Close
     - 저수준 파일 입출력에 관하여 : 파일 디스크립터를 둘러 싼 설정들..
     - 개발단계에서는 NAND에 다운로드 안하고, 파일시스템은 NFS로 Host에 위치 시킴
     - 함수에 Static 키워드 설정 : 해당 파일 내에서만 사용하고 접근할 수 있는 함수라는것을 명시적으로 설정 함
     - 커널은 일반 어플리케이션 영역과 다른 함수군을 사용 함 - printk..
  - 리눅스에 관한 몇가지 포인트 복습
     - 리눅스는 무료가 아니며, 제품출시는 유료이다. 단 학습은 무료
     - 커널은 OS 그 자체로 설명할 수 있다.
     - 선점형/비선점형 OS는 커널과 스케쥴링이 다르다
     - 쓰레드는 프로세스에 종속적
     - 기아상태 : 프로그램이 CPU에 컴퓨팅 자원할당을 요구하나, 제공해 주지 못하는 상태를 기아상태라고 명명 함
  - 리눅스 커널의 몇가지 기능에 관하여
     - 프로세스 관리 : SM, MSGQ, Pipi(np, unp)
     - 메모리 관리 : 모든 프로세스에 대한 가상 주소 영역을 구축한다
     - 파일시스템 관리 : 리눅스/유닉스의 철학 : Everything is file
     - 디바이스 제어 : 프로세스 메모리를 제외한 거의모든 대부분의 활동은 DD가 관리함
     - 네트워크 관리
  - 커널의 관점에서 관측 리눅스 구조
     1. fork함수를 유저가 호출
     2. fork의 함수 원형은 어디에 존재하는지 찾음 -> 커널에존재
     3. 디테일하게 공부해야 할 부분이 커널
     4. DD또한 커널영역에 속함
  - 리눅스 커널 2.6버전에서의 변화
    - Overview
        1. 커널코어 및 디바이스 드라이버 구조에 많은 변화
        2. 모듈의 구현 방식의 변화 : .ko
        3. 2.6은 전버전인 .4에 비해서 대대적인 터닝 포인트임
        4. 커널코드 공부 시 C언어로 구성된 객체지향 코드를 확인할 수 있음
        5. 클래스 객체지향은 결국 증복된 코드를 막아 생산성의 향상이 목적임
    - 메모리 부분 변화에 관하여
        1. MMU 없는 시스템의 지원
        2. Sleep하지 않는 메모리의 할당
        3. mempol기능 : 사전할당 후 이용하는
    - 인터럽트 핸들러 : 매우매우 복잡하고 어렵다
        - 그중 하나를 한다면 Request IRQ 만 알면 됨.
    - 디바이스 넘버링
        - Major넘버와 Minor 넘버가 존재
    - 커널모듈 적재/제거
        1. insmod
        2. rmmod
  - 리눅스 디바이스 드라이버의 3가지 : 캐릭터DD, 블럭DD, 네트워크DD
  - 실습 내용  
    ```s
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
    ```
  - tar 명령어로 압축파일 풀기
    ```s
    root@ubuntu-vm ~
    # tar xvjf arm-2010q1-202-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2 
    ```
  - 커널 압축풀어서 디렉토리 위치 시키기
    ```s
    root@ubuntu-vm ~
    # cd kernel-mds2450-3.0.22 -> 폴더 이동
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # ls -> 디렉토리 파일 리스트 출력
    COPYING   (~생략~)   samples  usr
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # pwd -> 디렉토리 경로 출력
    /root/kernel-mds2450-3.0.22
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make mds2450_defconfig -> MDS2450보드 설정 파일 셋팅 ★필수★필수★
    HOSTCC  scripts/basic/fixdep
    HOSTCC  scripts/kconfig/conf.o
    HOSTCC  scripts/kconfig/zconf.tab.o
    HOSTLD  scripts/kconfig/conf
    #
    # configuration written to .config
    #
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make clean -> 명령어로 파일 정리
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    ```
  - 커널 컴파일 시도 -> 에러 발생
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make zImage
    make: /project/toolchain/arm-2010q1/bin/arm-none-linux-gnueabi-gcc: 명령을 찾지 못했음
    HOSTCC  scripts/basic/fixdep
    HOSTCC  scripts/kconfig/conf.o
    HOSTCC  scripts/kconfig/zconf.tab.o
    HOSTLD  scripts/kconfig/conf
    scripts/kconfig/conf --silentoldconfig Kconfig
    make: /project/toolchain/arm-2010q1/bin/arm-none-linux-gnueabi-gcc: 명령을 찾지 못했음
    CHK     include/linux/version.h
    CHK     include/generated/utsrelease.h
    make[1]: `include/generated/mach-types.h'는 이미 갱신되었습니다.
    CC      kernel/bounds.s
    /bin/sh: /project/toolchain/arm-2010q1/bin/arm-none-linux-gnueabi-gcc: not found
    make[1]: *** [kernel/bounds.s] 오류 127
    make: *** [prepare0] 오류 2
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # 
    ```
  - 에러 보는 눈을 키워여되 크로스컴파일러 아구가 안맞아요 감으로 느끼는 연습을 하시란말이야
  - 고치는법 : gedit Makefile 명령어로 메이크파일 변경 -> 196라인
    ```
    CROSS_COMPILE	?= $(CONFIG_CROSS_COMPILE:"%"=%)
    ```
  - 위의 파일 내용을 아래와 같이 변경
    ```
    #CROSS_COMPILE	?= $(CONFIG_CROSS_COMPILE:"%"=%)
    CROSS_COMPILE	?= arm-none-linux-gnueabi-
    ```
  - ubuntu 컴파일러 설치과정
    1. 컴파일러(arm-linux-noni-eabi)를 압축해제하여 풀어놓기
    2. 컴파일러 경로는 /root/arm2010q1
    3. "source /etc/environment" 명령어 실행 : 환경변수 다시 로드
    4. 확인과정 
       - "arm-"입력후 tab키 눌르면 컴파일러 항목이 자동으로 달라붙어야 함
  - 컴파일러 설정 오류 시 확인&반복해야 함
  - TFTP 설정 진행
    ```s
    root@ubuntu-vm ~
    # tar xvzf kernel-mds2450-3.0.22-20140306.tar.gz 
    ```
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make mds2450_defconfig
    ```
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make clean
    ```
    - zImage: 임베디드 환경에서의 리눅스 커널 실행파일 이미지
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    # cd /etc/xinetd.d
    root@ubuntu-vm /etc/xinetd.d
    # gedit tftpd
    ```
    - /etc/xinetd.d 파일 내용
    ```s
    service tftp
    {
        protocol = udp
        port = 69
        socket_type = dgram
        wait = yes
        user = nobody
        server = /usr/sbin/in.tftpd
        serve_args = /tftpboot
        disable = no
    }
    ```
    - tftpboot 폴더 생성, 권한 부여, 네트워크 재부팅을 차례로 실행    
    ```s
    root@ubuntu-vm /etc/xinetd.d
    # mkdir /tftpboot
    root@ubuntu-vm /etc/xinetd.d
    # chmod 777 /tftpboot
    root@ubuntu-vm /etc/xinetd.d
    # /etc/init.d/xinetd restart
    * Stopping internet superserver xinetd                                  [ OK ] 
    * Starting internet superserver xinetd                                  [ OK ] 
    root@ubuntu-vm /etc/xinetd.d
    # netstat -au
    Active Internet connections (servers and established)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State      
    udp        0      0 *:57255                 *:*                      
    (중략)
    udp        0      0 *:tftp                  *:*                      
    (중략)
    udp6       0      0 [::]:mdns               [::]:*                            
    root@ubuntu-vm /etc/xinetd.d
    ```
 - 위에서 tftp 가 보이지 않는다면 분명히 오타, 아래 사항으로 확인가능
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    # cp zImage /tftpboot/
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    # ifconfig eth1 up 192.168.20.90 up
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    # cp zImage /tftpboot/
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    ```
 - 이미지 업로드가 안될때는 IP 주소 확인 -> 추후에 static IP 변경 가능
    ```s
    # ifconfig eth1 up 192.168.20.90 up
    ```
 - 리눅스 커널 업로드
    ```s
    tftpboot 30008000 zImage
    ```
 - MDS2450 보드에서 프롬프트 명령
    ```s
    bootm 30008000
    ```
 - MDS2450 보드에서 "printenv"명령어로 보드 셋팅 출력
    ```s
    bootargs=root=/dev/nfs rw nfsroot=192.168.20.90:/nfs/rootfs ip=192.168.20.246:192.168.20.90:192.168.20.1:255.255.255.0::eth0:off console=ttySAC1,115200n81
    stdin=serial
    stdout=serial
    stderr=serial
    ```
 - host 환경설정 NFS 설치
    ```s
    root@ubuntu-vm /etc
    # pwd
    /etc
    root@ubuntu-vm /etc
    # gedit exports 
    ```
 - gedit으로 실행시킨 exports 파일 내용 적는다
    ```s
    /nfs/rootfs *(rw,sync,no_root_squash,no_all_squash)
    ```
 - NFS를 위한 커널 서비스 재 실행
    ```s
    root@ubuntu-vm /etc
    # gedit exports 
    root@ubuntu-vm /etc
    # /etc/init.d/nfs-kernel-server restart
    ```
 - 이후 부팅과정
    ```s
    [teraterm]
    tftp 300800 zImage
    [host ubuntu]
    # ifconfig eth1 192.168.20.90 up
    ->다운이 실행됨
    [teraterm]
    bootm 30008000
    ->다운이 실행됨 혹시 rootfs를 못찾고 있다면
    [host ubuntu]
    # ifconfig eth1 192.168.20.90 up
    ```
 - 보드에 커널(zImage와, rootfs가 모두 업로드 되면 아래 화면을 만난다)
    ```s
    Welcome to MDS2450
    mds2450 login: root
    ```
 - 로그인 비밀번호 : root
 - 위 과정으로 리눅스 부팅 완료!! 커널 확인을 위해서 아래의 명령어 입력
    ```s
    # uname -a
    ```
 - 확인과정 (안되는 경우를 대비하여) - [host ubuntu]
    ```s
    root@ubuntu-vm /etc
    # gedit exports 
    ```
    ```s
    /nfs/rootfs *(rw,sync,no_root_squash,no_all_squash)
    ```
 - 네트워크 시스템 재부팅
    ```s
    root@ubuntu-vm /etc
    # /etc/init.d/nfs-kernel-server restart
    ```
 - 아래와 같은 메시지 확인
    ```s
    root@ubuntu-vm /etc
    # /etc/init.d/nfs-kernel-server restart
    * Stopping NFS kernel daemon                                          [ OK ] 
    * Unexporting directories for NFS kernel daemon...                    [ OK ] 
    * Exporting directories for NFS kernel daemon...                             exportfs: /etc/exports [1]: Neither 'subtree_check' or 'no_subtree_check' specified for export "*:/nfs/rootfs".
    Assuming default behaviour ('no_subtree_check').
    NOTE: this default has changed since nfs-utils version 1.0.x          [ OK ]
    * Starting NFS kernel daemon 
    ```
    ```s
    root@ubuntu-vm /nfs/rootfs
    # ls
    aaa  dev  home  linuxrc  opt   root  sbin  tmp  var
    bin  etc  lib   mnt      proc  run   sys   usr
    root@ubuntu-vm /nfs/rootfs
    # mkdir /root/module
    root@ubuntu-vm /nfs/rootfs
    # ls
    aaa  dev  home  linuxrc  opt   root  sbin  tmp  var
    bin  etc  lib   mnt      proc  run   sys   usr
    root@ubuntu-vm /nfs/rootfs
    # cd /root/module/
    root@ubuntu-vm ~/module
    # ls
    root@ubuntu-vm ~/module
    # pwd
    /root/module
    root@ubuntu-vm ~/module
    # cp /mnt/hgfs/Data/13_module.zip /root/module/
    root@ubuntu-vm ~/module
    # ls
    13_module.zip
    root@ubuntu-vm ~/module
    # ls
    13_module.zip
    root@ubuntu-vm ~/module
    # unzip 13_module.zip 
    Archive:  13_module.zip
    inflating: .hello.ko.cmd           
    inflating: .hello.mod.o.cmd        
    inflating: .hello.o.cmd            
    inflating: .hello_param.ko.cmd     
    inflating: .hello_param.mod.o.cmd  
    inflating: .hello_param.o.cmd      
    inflating: .tmp_versions/hello.mod  
    inflating: .tmp_versions/hello_param.mod  
    inflating: hello.c                 
    inflating: hello.ko                
    inflating: hello.mod.c             
    inflating: hello.mod.o             
    inflating: hello.o                 
    inflating: hello_param.c           
    inflating: hello_param.ko          
    inflating: hello_param.mod.c       
    inflating: hello_param.mod.o       
    inflating: hello_param.o           
    inflating: Makefile                
    extracting: Module.symvers          
    inflating: modules.order           
    root@ubuntu-vm ~/module
    # ls
    13_module.zip   hello.c      hello.mod.o    hello_param.ko     hello_param.o
    Makefile        hello.ko     hello.o        hello_param.mod.c  modules.order
    Module.symvers  hello.mod.c  hello_param.c  hello_param.mod.o
    root@ubuntu-vm ~/module
    # make
    make -C /root/work/embedded/linux-3.12.14 SUBDIRS=/root/module modules
    make: *** /root/work/embedded/linux-3.12.14: 그런 파일이나 디렉터리가 없습니다.  멈춤.
    make: *** [all] 오류 2
    root@ubuntu-vm ~/module
    # make clean
    make -C /root/work/embedded/linux-3.12.14 SUBDIRS=/root/module clean
    make: *** /root/work/embedded/linux-3.12.14: 그런 파일이나 디렉터리가 없습니다.  멈춤.
    make: *** [clean] 오류 2
    root@ubuntu-vm ~/module
    # gedit Makefile 
    ```
 - 커널 디렉토리 확인
    ```
    KDIR	:= /root/kernel-mds2450-3.0.22
    ```
 - target board 에서 작업 
     - 커널모듈 Insert
        ```
        # insmod hello.ko
        Hello, world 4.
        ```
     - 커널 모듈 Remove
        ```
        # rmmod hello.ko
        Goodbye, world 4.
        ```
 - hello.ko 모듈 타겟보드의 nfs로 복사하기
    ```
    root@ubuntu-vm ~/module
    # cp hello.ko /nfs/rootfs/home/default
    ```
 - host 개발PC에서 xx.ko 커널모듈
    ```
    root@ubuntu-vm ~/module
    # pwd
    /root/module
    root@ubuntu-vm ~/module
    # cp myk.ko /nfs/rootfs/home/default/
    ```
 - 기본 커널모듈 소스
    ```c
    #include <linux/module.h>       /* Needed by all modules */
    #include <linux/kernel.h>       /* Needed for KERN_INFO */
    #include <linux/init.h>         /* Needed for the macros */

    #define DRIVER_AUTHOR   "DongKyu, Kim <dongkyu@mdstec.com>"
    #define DRIVER_DESC             "A sample driver"


    static int __init init_hello_4(void)
    {
            printk(KERN_INFO "Hello, world 4.\n");
            return 0;
    }

    static int __init dhkim_km_hello(void)
    {
            printk(KERN_INFO "Hello MDS kernal World!! From:DHKim\n");
            return 0;
    }

    static int __exit dhkim_km_gobye(void)
    {
            printk(KERN_INFO "byebye kernal World~~ From:DHKim\n");
            return 0;
    }

    static void __exit cleanup_hello_4(void)
    {
            printk(KERN_INFO "Goodbye, world 4.\n");
    }

    module_init(dhkim_km_hello);
    module_exit(dhkim_km_gobye);
    /* Get rid of taint message by declaring code as GPL. */
    MODULE_LICENSE("GPL");
    MODULE_AUTHOR(DRIVER_AUTHOR);           /* Who wrote this module? */
    MODULE_DESCRIPTION(DRIVER_DESC);        /* What does this module do */
    MODULE_SUPPORTED_DEVICE("testdevice");
    ```

  ## 1일차 정리
   - KDIR :커널의 위치
   - 커널 모듈이 있어야지만 커널모듈(너컬 오브젝트) 생성이 가능 
   - 커널모듈은 커널이 돌고있는 와중에 인서트/리무브가 가능
   - 실습 진행을 원활하게 하기 위한 몇가지 
      1. 리셋
      2. bootcmd 에다가 환경설정으로 자동으로 nfs까지 전부 딱 정합이 될 수 있는 환경 조성(setenv, bootcmd 프롬프트 명령어 활용)
      3. NFS환경 구축
      4. Hello kernal world 메시지 출력되게
      5. 커널(zImage) 의 위치는 /root/kernel-mds2450-3.0.22/arch/arm/boot
   - NFS(network file system)필요성 정리 
      1. 임베디드 리눅스 시스템  개발시, Filesystem을 계속해서 접근/변경해야 하는데, 매번 Target의 Nand Flash를 계속해서 쓰면 시간이 낭비됨
      2. Host PC의 파일시스템을 공유해서 타겟보드의 루트파일시스템을 제공하는 서비스
      3. Host PC에서 작업한 내용을 PC내부에서 통신없이 Target과 공유하는 nfs폴더로 이동하면, Target의 Filesystem에서 자동으로 반영됨.(매우매우 편리함)
   - TFTP - [위키 설명](https://ko.wikipedia.org/wiki/TFTP) : Trivial 된 FTP프로토콜로써, 간소화된 프로토콜이라 적은 용량과 사이즈로 구현이 간단하여 임베디드 시스템에서 파일 전송용으로 많이 사용됨
   - FTP - [위키설명](https://ko.wikipedia.org/wiki/%ED%8C%8C%EC%9D%BC_%EC%A0%84%EC%86%A1_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C) : 파일 전송 프로토콜로써 널리 사용됨, 이더넷 통신 기반이다.
   - [NFS 셋팅 가이드 - 1](https://blog.naver.com/meelong0/140025104036)
   - [NFS 셋팅 가이드 - 2](http://forum.falinux.com/zbxe/index.php?mid=lecture_tip&page=118&document_srl=462347)
   - [초보자를 위한 임베디드 리눅스 학습 가이드](http://ebook.pldworld.com/_eBook/Embedded/embedded_guide-v1.1_html.htm)
 #
 ## Day 2 
  - 리눅스를 인공위성쯤에서 큰그림을 보고 내려다봐라
  - 어떤 역할이고 전체 시스템에서 어디쯤 위치하고 있으며, 어떤기능을 하는지 큰그림에서 작은  
  - 대부분의 경우는 DD를 짜고, DD를 활용한 어플리케이션을 짜는 경우가 많음. 현업에 가면 DD와 APP이 별도가 아닌 하나의 개념으로 봐야 함
  - 
  ```s
  root@ubuntu-vm /proc/1
  # mknod /dev/led c 240 0
  ```
  - mknod : 특수 장치 파일로 커널에 등록
  - /dev/led : 특수장치파일 생성위치
  - c : 캐릭터 디바이스 
  - 240 : Major number
  - 0 : Minor number
  - 커널은 제작되어있는데 디바이스드라이버는 그때그때
    - 이럴때를 위해서 함수포인터를 등록해서 
    - 함수포인터 변수, void형 포인터 변수는 투성이로 나온다!!
  - DD가 빡센이유
    - DMA살리기 -> 펌웨어 DD는 1비트의 예술, 오동작 혹은 Halt일어날 수 있음
    - Timer Oneshot, AutoReload ... 
    - 삼성 SDS다니시다가 과정에 들어오신분, 연세 60살-> 서버관리 서버확장 외국에서 성대나오고 영어가 기가막혀
    - 삼성 보드 메뉴얼은 2450 보드 후진 영어 DD는-> 삽질이 많이 들어가는 분야, 해야될께많아
    - 리눅스 디바이스 드라이버 유영창 저 절판, 단계별로 올라가기 좋은 책
    - 
  - 비특권모드에서 특권모드로는 못넘어와(23p)
    - 

```cpp
#include <linux/module.h>
#include <linux/init.h>
#include <linux/major.h>
#include <linux/fs.h>
#include <linux/cdev.h>

MODULE_LICENSE("GPL");

static int sk_major = 0, sk_minor = 0;
static int result;
static dev_t sk_dev;

static struct cdev sk_cdev;

static int sk_register_cdev(void);

/* TODO: Define Prototype of functions */
static int sk_open(struct inode *inode, struct file *filp);
static int sk_release(struct inode *inode, struct file *filp);

/* TODO: Implementation of functions */
static int sk_open(struct inode *inode, struct file *filp)
{
    printk("Device has been opened...\n");

    /* H/W Initalization */

    //MOD_INC_USE_COUNT;  /* for kernel 2.4 */

    return 0;
}

static int sk_release(struct inode *inode, struct file *filp)
{
    printk("Device has been closed...\n");

    return 0;
}

struct file_operations sk_fops = {
    .open       = sk_open,
    .release    = sk_release,
};

static int __init sk_init(void)
{
    printk("SK Module is up... \n");

        if((result = sk_register_cdev()) < 0)
        {
                return result;
        }

    return 0;
}

static void __exit sk_exit(void)
{
    printk("The module is down...\n");
        cdev_del(&sk_cdev);
        unregister_chrdev_region(sk_dev, 1);
}

static int sk_register_cdev(void)
{
        int error;

        /* allocation device number */
        if(sk_major) {
                sk_dev = MKDEV(sk_major, sk_minor);
                error = register_chrdev_region(sk_dev, 1, "sk");
                // sk_major에 0일때 ->> 
        } else {
                error = alloc_chrdev_region(&sk_dev, sk_minor, 1, "sk");
                sk_major = MAJOR(sk_dev);
                // sk_major에 0이 아닌 다른 함수로 설저정일때 ->> 
        }

        if(error < 0) {
                printk(KERN_WARNING "sk: can't get major %d\n", sk_major);
                return result;
        }
        printk("major number=%d\n", sk_major);

        /* register chrdev */
        cdev_init(&sk_cdev, &sk_fops);
        sk_cdev.owner = THIS_MODULE;// THis 포인터
        sk_cdev.ops = &sk_fops; // 주소를 넘겨 줘
        error = cdev_add(&sk_cdev, sk_dev, 1);// 최종적으로 이렇게 넘겨 줘, 캐릭터 디바이스 에드

        if(error)
                printk(KERN_NOTICE "sk Register Error %d\n", error);

        return 0;
}



module_init(sk_init);
module_exit(sk_exit);
```




```cpp
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <fcntl.h>

int main(void)
{
    int fd;

    fd = open("/dev/SK", O_RDWR);//dev sk pointing, mknod 필요!!
    printf("fd = %d\n", fd);

    if (fd<0) {
        perror("/dev/SK error");
        exit(-1);
    }
    else
        printf("SK has been detected...\n");

    getchar();
    close(fd);

    return 0;
}
~        
```
-GCC 로 컴파일하지말고 arm용 크로스 컴파일러로 

```
root@ubuntu-vm ~/open
# ls
02_open_release.zip  Module.symvers   modules.order  sk.ko     sk.mod.o  sk_app    sk_app_open
Makefile             Modules.symvers  sk.c           sk.mod.c  sk.o      sk_app.c
root@ubuntu-vm ~/open
# file sk_app
sk_app: ELF 32-bit LSB executable, ARM, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.16, not stripped
root@ubuntu-vm ~/open
# cp sk_app /nfs/rootfs/workSpace/
root@ubuntu-vm ~/open
# 

```


-  주목 놓치면 안되!!
```s
# ls
csk.ko   sk_app
# ./sk_app
fd = -1
/dev/SK error: No such file or directory
```
- 위 실행에서 에러가 난 이유 : 파일이 없어서 , 장치 파일로 등록이 안되있어서 오픈이 안되있다. 장치파일을 다 이런식으로  연결되어 있어
- 아래는 해결
```s
# cd /workSpace/
# ls
sk.ko   sk_app
# insmod sk.ko
SK Module is up...
major number=251
# mknod /dev/SK c 251 0
# ./sk
sk.ko   sk_app
# ./sk_app
Device has been opened...
fd = 3
SK has been detected...
```
- mmknod란 : 

- write함수 추가하려면
```cpp
struct file_operations sk_fops = {
    .open       = sk_open,
    .release    = sk_release,
    .write      = (여기에 입력)
};
```
- write함수 프레임만 맞게 짜기 시작해야됭
- /dev/SK :  디바이스 드라이버 이름 > 만드는법
```s
mknod /dev/SK c 251 0
```
- 디바이스 드라이버는 대문자로 꼭 만들어야됭

- 디바이스 드라이버를 만드는데 핑요한 구조체 : dev_t
```cpp
static dev_t sk_dev;
```
> 강사님이 핵심
 >> - 이쪽은 모듈 요기는 스트럭쳐
  - 장치 파일로 바꿔야 특수장치파일로 만들어줘야지 어플리케이션에서 열 수 있다.
  - 오픈을 하면서 파일 디스크립터를 
  - SK : 스켈렉톤, 뼈대라는 의미
  - /dev/SK 특수 장치 파일
   ```cpp
   static int sk_write(struct file *filp, const char *buf, size_t count, loff_t *f_pos)
   {
         char data[11];

         copy_from_user(data, buf, count);
         printk("data >>>>> = %s\n", data);

         return count;
   }

   static int sk_read(struct file *filp, char *buf, size_t count, loff_t *f_pos)
   {
         char data[20] = "this is read func...";

         copy_to_user(buf, data, count);

         return 0;
   }
   ```
  - copy_to_user : 커널에서 유저(APP)단으로
  - copy_from_user : 
  - 디바이스 이름 : sk
  - 0이면 커널이 알아서 dev num 지정해주는것, !0 이면, 
   ```cpp
   retn = read(fd, buf, 20); //APP에서의 syscall
   ```
   ```
   retn = write(fd, buf, 10);//APP에서의 syscall
   ```
   ```
   디바이스 트리란? : 커널 4.x 부터 도입된 개념으로 기존의 D.D를 좀더 쉽게 접근하고, 생성해주는 스크립트 언어
   디바이스 트리 공부를 위해서는 : 하드웨어는 라즈베리파이3, 문서는 아래 링크 참고
   ```
- [라즈베리파이 디바이스트리](https://wikidocs.net/3205)
### 과제 1
  - 과제1 : 어플리케이션 계층에서 일차원 문자배열을 선언하고 그 주소를 read() 함수를 이용하여 커널영역으로 넘겨주면, 커널영역에서 대문자를 할당하여 copy_to_user/copy_from_user 를 사용하여 다시 응용계층으로 넘겨주면 최종적으로 어플단에서 대문자를 찍는 프로그램을 작성하시고, copy_from_user 도 적용 해보세요(소문자 찍기)
 - 과제 못풀음 삽질하다가
 - 구조체 과제(조별과제)
   ```s
   copy_to_user()/copy_from_user() 사용
   ```

  - 1차과제 test_mydrv.c
   ```cpp
   /* test_mydrv.c */
   //유저 영역의 어플리케이션 코드
   #include <stdio.h>
   #include <stdlib.h>
   #include <sys/types.h>
   #include <fcntl.h>

   #define MAX_BUFFER 26

   char buf_in[MAX_BUFFER], buf_out[MAX_BUFFER];
   //입력버퍼 출력버퍼 공간 생성

   int main()
   {
   int fd,i;
   
   fd = open("/dev/mydrv",O_RDWR);
   //DD오픈, mydrv_open() 함수 호출 됨

   read(fd,buf_in,MAX_BUFFER);
   //DD에서의 mydrv_read() 함수 호출됨
   // open()에서 얻은 fd, 입력 버퍼 주소, 버퍼 크기
   printf("buf_in = %s\n",buf_in);
   //버퍼에 어떤 값이 채워져서 buf_in이 됨

   for(i = 0;i < MAX_BUFFER;i++)
      buf_out[i] = 'a' + i;
   write(fd,buf_out,MAX_BUFFER);
   //쓰기버퍼 , 파일디스크립터, 버퍼주소, 버퍼 최대길이
   
   close(fd);
   //디바이스 드라이버 닫기
   
   return (0);
   }

   ```
   ```cpp
   /*
   mydrv.c - kernel 3.0 skeleton device driver
                  copy_to_user()
   */
   // 커널 영역의 디바이스 드라이버 코드
   #include <linux/module.h>
   #include <linux/moduleparam.h>
   #include <linux/init.h>
   #include <linux/kernel.h>   /* printk() */
   #include <linux/slab.h>   /* kmalloc() */
   #include <linux/fs.h>       /* everything... */
   #include <linux/errno.h>    /* error codes */
   #include <linux/types.h>    /* size_t */
   #include <asm/uaccess.h>
   #include <linux/kdev_t.h>
   #include <linux/cdev.h>
   #include <linux/device.h>

   #define DEVICE_NAME "mydrv"
   static int mydrv_major = 240;//디바이스 메이저 넘버 명시적할당
   module_param(mydrv_major, int, 0);//모듈 파라미터.,,,>?


   static int mydrv_open(struct inode *inode, struct file *file)
   {//DD의 오픈함수 
   printk("mydrv opened !!\n");
   return 0;
   }

   static int mydrv_release(struct inode *inode, struct file *file)
   {//DD의 클로즈 함수
   printk("mydrv released !!\n");
   return 0;
   }

   static ssize_t mydrv_read(struct file *filp, char __user *buf, size_t count,loff_t *f_pos)
   {//리드함수, 파일포인터, 유저버퍼 함수포인터, 버퍼사이즈, pos받음, USER영역의 함수와 1:1매칭 되지 않음
   char *k_buf;
   int i;
   // 리드함수니까 유저 영역에서 read함수가 호출 된 상황
   k_buf = kmalloc(count,GFP_KERNEL);//멜록으로 공간 할당
   for(i = 0 ;i < count;i++) {
         k_buf[i] = 'A' + i;//넘겨받은 버퍼 주소 위치에다가 차례로 쓰기
   }
   if(copy_to_user(buf,k_buf,count)) {
      return -EFAULT;//에러 핸들링
   }
   printk("mydrv_read is invoked\n");//커널 메시지 출력
   kfree(k_buf);// 할당받은 공간 해제
   return 0;

   }

   static ssize_t mydrv_write(struct file *filp,const char __user *buf, size_t count,
                              loff_t *f_pos)
   {
   char *k_buf;
   //유저 영역에서 Write함수를 호출한경우 DD에서 Write함수가 호출 되는것을 확인 함.
   k_buf = kmalloc(count,GFP_KERNEL);//커널 맬럭으로 임시 저장공간 할당받음
   if(copy_from_user(k_buf,buf,count)) {
      return -EFAULT;//에러 핸들링,copy_from_user() 함수가 0이 아닌 수를 리턴하면 해당 케이스로 진입하여 에러를 반환
   }
   printk("k_buf = %s\n",k_buf);
   printk("mydrv_write is invoked\n");
   kfree(k_buf);
   return 0;
   }


   /* Set up the cdev structure for a device. */
   static void mydrv_setup_cdev(struct cdev *dev, int minor,
         struct file_operations *fops)
   {
      int err, devno = MKDEV(mydrv_major, minor);//devno 번호에 
      
      cdev_init(dev, fops);
      dev->owner = THIS_MODULE;
      dev->ops = fops;
      err = cdev_add (dev, devno, 1);
      
      if (err)
         printk (KERN_NOTICE "Error %d adding mydrv%d", err, minor);
   }


   static struct file_operations mydrv_fops = {
      .owner    = THIS_MODULE,
         .read	  = mydrv_read,
      .write    = mydrv_write,
      .open     = mydrv_open,
      .release  = mydrv_release,
   };

   #define MAX_MYDRV_DEV 1

   static struct cdev MydrvDevs[MAX_MYDRV_DEV];

   static int mydrv_init(void)
   {// DD 이니셜라이즈 함수
      int result;
      dev_t dev = MKDEV(mydrv_major, 0);//MKDEV()는 시스템 함수
      //dev_t 구조체 변수 이름 dev에다가 MKDEV() 리턴 값 대입
      
      /* Figure out our device number. */
      if (mydrv_major)//mydrv_major!=0 인경우, 디바이스넘버 명시한경우 In case
         result = register_chrdev_region(dev, 1, DEVICE_NAME);//시스템 함수 호출
      else {//디바이스 넘버 명시하지 않은경우 
         result = alloc_chrdev_region(&dev,0, 1, DEVICE_NAME);//시스템 함수 호출
         mydrv_major = MAJOR(dev);//시스템 함수 호출해서 번호 할당
      }
      if (result < 0) {//에러 핸들링
         printk(KERN_WARNING "mydrv: unable to get major %d\n", mydrv_major);
         return result;
      }
      if (mydrv_major == 0)
         mydrv_major = result;

      mydrv_setup_cdev(MydrvDevs,0, &mydrv_fops);//이 함수는 User함수
      printk("mydrv_init done\n");	
      return 0;
   }

   static void mydrv_exit(void)
   {
      cdev_del(MydrvDevs);
      unregister_chrdev_region(MKDEV(mydrv_major, 0), 1);
      printk("mydrv_exit done\n");
   }

   module_init(mydrv_init);
   module_exit(mydrv_exit);

   MODULE_LICENSE("Dual BSD/GPL");
   ```

### 과제2
 - 과제2 : 응용프로그램이 보내준 아래 구조체 형식의 데이터를 드라이버가 받아서 출력시키고 드라이버는 같은 구조체 형식으로 또 다른 데이터를 응용프로그램에게 보내주고 응용프로그램이 출력시키는 코드를 구현하세요
   ```cpp
   /*  구조체 포맷  */
   typedef struct
   {
      int age;  //나이 :35
      char name[30];// 이름 : HONG KILDONG
      char address[20]; // 주소 : SUWON CITY
      int  phone_number; // 전화번호 : 1234
      char depart[20]; // 부서 : mds
   } __attribute__ ((packed)) mydrv_data;

   //   sprintf(k_buf->name,"HONG KILDONG");
   ```
 - copy to user 
    1. APP 영역에서의 write함수 호출
    2. DD에서 -> struct file operations 에서 등록 : wrtie = sk_write
    3. 안적었음.. 뭦;
 - attriibute Packed : 구조체 peding bytes 제거하는 예약 키워드  
 - [리눅스 공부 - 임베디드 홀릭님 카페북](https://cafe.naver.com/lazydigital/book5089864/8630)

#
## 3일차
 - DD프로그래밍 핵심
   1. ㅇㅇ
   2. 
   3. 디바이스드라이버는 커널 프로그래밍 -> DD는 조금만 잘못해도 커널 패닉이 
   4. 커널패닉은 매우 위험, 커널 프로그래밍은 
 - 강조 점 fileops. 함수포인터가 늘어나고 있어요
 - volatile 의미 두가지
   1. 최적화 하지마라
   2. 캐시에서 읽지 말고 메모리에서 읽어와라

   ```s
   # cat /proc/devices
   Character devices:
   1 mem
   2 pty
   3 ttyp
   4 /dev/vc/0
   4 tty
   4 ttyS
   5 /dev/tty
   5 /dev/console
   5 /dev/ptmx
   7 vcs
   10 misc
   13 input
   21 sg
   29 fb
   67 mds2450-kscan
   81 video4linux
   86 ch
   90 mtd
   108 ppp
   116 alsa
   128 ptm
   136 pts
   180 usb
   188 ttyUSB
   189 usb_device
   204 ttySAC
   216 rfcomm
   251 sk
   252 BaseRemoteCtl
   253 usbmon
   254 rtc

   Block devices:
   1 ramdisk
   259 blkext
   7 loop
   8 sd
   31 mtdblock
   65 sd
   66 sd
   67 sd
   68 sd
   69 sd
   70 sd
   71 sd
   128 sd
   129 sd
   130 sd
   131 sd
   132 sd
   133 sd
   134 sd
   135 sd
   179 mmc
   #
   ```
- 3일차 : ictol_mydrv.h
```cpp
#ifndef _IOCTL_MYDRV_H_
#define _IOCTL_MYDRV_H_
//헤더파일용 선언

#define IOCTL_MAGIC    254
//매직넘버..?

typedef struct
{
	unsigned char data[26];	
} __attribute__ ((packed)) ioctl_buf;
//구조체가 담고있는것 캐릭터배열 26개짜리 왜 근데 계속 26개냐


#define IOCTL_MYDRV_TEST           _IO(  IOCTL_MAGIC, 0 )
#define IOCTL_MYDRV_READ           _IOR( IOCTL_MAGIC, 1 , ioctl_buf )
#define IOCTL_MYDRV_WRITE          _IOW( IOCTL_MAGIC, 2 , ioctl_buf )
#define IOCTL_MYDRV_WRITE_READ     _IOWR( IOCTL_MAGIC, 3 , ioctl_buf )

#define IOCTL_MAXNR                   4

#endif // _IOCTL_MYDRV_H_
```
- 위 파일은 디바이스드라이버를 짤대 정말 많이 등장 강사님 시간많이투자
- 어려울껀 없고 금방 하세요
- 3개를 봐야함  
  1. 
  2. 
  3. 
- 매직넘버는 알아서 줘도됨
```cpp

  ioctl(fd,IOCTL_MYDRV_TEST);
  
  ioctl(fd, IOCTL_MYDRV_READ, buf_in );
```
```cpp

```
  - 어바웃 포팅
     - 창호형이 리눅스 포팅을 하기로 했어
     - 삼성에서 chip과 보드를 같이 출시를 해 (데모)
     - 왠만큼 소스와 회로 오픈해줘 - 업체에서 가져다가 모디파이해서 만들면 됭

-
- 오늘의 버그 증상 : DD가 첫회만 잘 돈다.. 첫번째 부팅 후 정상동작 확인한 다음에 모듈을 내렸다가 다시 올리려고 하면 해당 메시지 발생함.
```s
# insmod hello.ko
Hello DD world !!
IRQ_EINT3 failed to request external interrupt.
insmod: can't insert 'hello.ko': unknown symbol in module, or unknown parameter
#
```
- APP 코드
   ```cpp
   /* test_mydrv.c */

   #include <stdio.h>
   #include <stdlib.h>
   #include <sys/types.h>
   #include <fcntl.h>

   #define MAX_BUFFER 26
   char buf_in[MAX_BUFFER], buf_out[MAX_BUFFER];

   int main()
   {
   int fd,i,mode;
   
   fd = open("/dev/mydrv",O_RDWR);
   printf("fd = %d\n",fd);

   if(fd == -1) {
      printf("you are fail to open div files, if you success ...\n");
      printf("mknod /dev/mydrv c 240 0\n");
      exit(1);
   }

   printf("APP : Read Func\n");
   read(fd,buf_in,MAX_BUFFER);
   printf("APP : Read() -> buf_in = %s\n",buf_in);
   

   printf("APP : Write Func\n");
   for(i = 0;i < MAX_BUFFER;i++)
      buf_out[i] = 'a' + i;
   write(fd,buf_out,MAX_BUFFER);
   printf("APP : END Defalut DEMO\n");


   printf("APP : start while demo\n");
   printf("APP : \n");
   strcpy(buf_out,"I'm Application\n");
   write(fd,buf_out,MAX_BUFFER);

   close(fd);
   return (0);
   }

   ```
   - DD 코드
   ```cpp
   #include <linux/module.h>	/* Needed by all modules */
   #include <linux/kernel.h>	/* Needed for KERN_INFO	*/
   #include <linux/init.h>		/* Needed for the macros */
   #include <linux/kdev_t.h>
   #include <linux/cdev.h>
   #include <linux/errno.h>
   #include <linux/slab.h>
   #include <linux/delay.h>
   #include <linux/interrupt.h>
   #include <linux/device.h>
   #include <linux/fs.h>       /* everything... */
   #include <linux/types.h>    /* size_t */

   #include <asm/io.h>
   #include <asm/irq.h>
   #include <asm/uaccess.h>

   #include <mach/gpio.h>
   #include <mach/regs-gpio.h>
   #include <plat/gpio-cfg.h>


   #define	DRIVER_AUTHOR	"DHKim of southkorea"
   #define	DRIVER_DESC		"DHKim sample DD"
   #define DRV_NAME        "mydrv"
   #define DEVICE_NAME 	"mydrv"
   #define MAX_MYDRV_DEV 1

   static int mydrv_major = 240;
   module_param(mydrv_major, int, 0);


   //from NCH.. jubjub!
   static char DDin_buf[50];
   static char DDout_buf[50];

   //
   static void mydrv_setup_cdev(struct cdev *dev, int minor,struct file_operations *fops);

   //
   static int mydrv_open(struct inode *inode, struct file *file)
   {
   printk("DHKim DD.drv opened !!\n");
   return 0;
   }

   static int mydrv_release(struct inode *inode, struct file *file)
   {
   printk("DHKim D.drv released !!\n");
   return 0;
   }


   static ssize_t mydrv_read(struct file *filp, char __user *buf, size_t count,
                  loff_t *f_pos)
   {
   char *k_buf;
   int i;
      printk("read in func\n");

   k_buf = kmalloc(count,GFP_KERNEL);
   for(i = 0 ;i < count;i++)
         k_buf[i] = 'A' + i;
   
   if(copy_to_user(buf,k_buf,count)) {
      return -EFAULT;
   }
   printk("read is invoked in kerneland HEAP\n");
   
   if(copy_to_user(buf, DDout_buf, count)) {
      return -EFAULT;
   }
   printk("success cpy2u() ->> DDout_buf");

   kfree(k_buf);
   return 0;

   }

   static ssize_t mydrv_write(struct file *filp,const char __user *buf, size_t count,
                              loff_t *f_pos)
   {
   char *k_buf;
   printk("write in func\n");

   k_buf = kmalloc(count,GFP_KERNEL);
   if(copy_from_user(k_buf,buf,count)) {
      return -EFAULT;
   }
   printk("k_buf = %s\n",k_buf);
   printk("write is invoked\n");
   
   if(copy_from_user(DDin_buf, buf, count)) {
      return -EFAULT;
   }
   printk("copy_from_user() func success ->>DDin_buf");
   kfree(k_buf);
   return 0;
   }


   static irqreturn_t keyinterrupt_func1(int irq, void *dev_id, struct pt_regs *resgs)
   {
         printk("Key 0 pressed!!!(%d)\n",irq);
         printk("KDD : %s",DDin_buf);
         return IRQ_HANDLED;
   }

   static irqreturn_t keyinterrupt_func2(int irq, void *dev_id, struct pt_regs *resgs)
   {
         printk("Key 1 pressed!!!(%d)\n",irq);
         return IRQ_HANDLED;
   }

   static irqreturn_t keyinterrupt_func_mls(int irq, void *dev_id, struct pt_regs *resgs)
   {		//GPG2,3,4,5,6 =>> 5 keys
         //printk("Key (%d) pressed!!!\n",irq);
         switch(irq)
         {
            case 18 ://in case of SW4,9
               printk("in case of SW4,9\n");
               break;
            case 19 ://in case of SW5,10
               printk("in case of SW5,10\n");
               break;
            case 48 ://in case of SW6,11
               printk("in case of SW6,11\n");
               break;
            case 49 ://in case of SW7,12
               printk("in case of SW7,12\n");
               break;
            case 50 ://in case of SW8,13
               printk("in case of SW8,13\n");
               break;
         }


         return IRQ_HANDLED;
   }
   /*
   //how to solv this 10 keys problem
   1.GPF7,GPG0 set 0(Low) -> every 10 key recognize btn
   2.if btn is pushh ->> through in case of inerrput
   3.for ex case 18, ->> 
   4.1. up 5 keys
   rGPFDAT &= ~(0x1<<7);   //Clear GPF7
   rGPGDAT |= (0x1);       //Set GPG0
   4.2 down 5 key
   rGPFDAT |= (0x1<<7);    //Set GPF7
   rGPGDAT &= ~(0x1);      //Clear GPG0
   5. each row(colum?), set port GPIO, NOT EXINT
   6. Read GPIO (decision Making) up or down
   7. save exect key 
   8. recover every key interrupt mode
   9. return exect key (swq about no.7)
   */
   static void mydrv_setup_cdev(struct cdev *dev, int minor,
         struct file_operations *fops)
   {
      int err, devno = MKDEV(mydrv_major, minor);
      
      cdev_init(dev, fops);
      dev->owner = THIS_MODULE;
      dev->ops = fops;
      err = cdev_add (dev, devno, 1);
      
      if (err)
         printk (KERN_NOTICE "Error %d adding mydrv%d", err, minor);
   }


   /***********************************************
   ************************************************
   ************************************************
   ***********************************************/
   static struct file_operations mydrv_fops = {
      .owner   = THIS_MODULE,
         .open    = mydrv_open,
      .read	 = mydrv_read,
      .write   = mydrv_write,
      .release = mydrv_release,
   };


   static struct cdev MydrvDevs[MAX_MYDRV_DEV];

   static int __init init(void)
   {
      int result;
      int ret;
      dev_t dev = MKDEV(mydrv_major, 0);
      //=section of inerterupt=======================================================================
      printk(KERN_INFO "Hello DD world !!\n");
            ///*
         // set Interrupt mode
         s3c_gpio_cfgpin(S3C2410_GPF(0), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(1), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(2), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(3), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(4), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(5), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(6), S3C_GPIO_SFN(2));

         if(request_irq(IRQ_EINT0,(void *)keyinterrupt_func1,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL))
         {
                  printk("IRQ_EINT0 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT1, (void *)keyinterrupt_func2,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT1 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT2, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT2 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT3, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT3 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT4, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT4 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT5, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT5 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT6, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT6 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         printk(KERN_INFO "%s successfully intq loaded\n", DRV_NAME);
         //*/
      //=section of inerterupt=======================================================================
      printk("This Module major number : 240\n");
      if (mydrv_major)
         result = register_chrdev_region(dev, 1, DEVICE_NAME);
      else {
         result = alloc_chrdev_region(&dev,0, 1, DEVICE_NAME);
         mydrv_major = MAJOR(dev);
      }
      if (result < 0) {
         printk(KERN_WARNING "mydrv: unable to get major %d\n", mydrv_major);
         return result;
      }
      if (mydrv_major == 0)
         mydrv_major = result;

      mydrv_setup_cdev(MydrvDevs,0, &mydrv_fops);
      printk("Hello.ko module init done\n");	

      return 0;
   }

   static void __exit cleanup(void)
   {
      cdev_del(MydrvDevs);
      unregister_chrdev_region(MKDEV(mydrv_major, 0), 1);
      printk("hello.ko DD module exit done\n");
      //=====================================================
      ///*
      printk(KERN_INFO "Goodbye, world 4.\n");
      free_irq(IRQ_EINT0, NULL);
      free_irq(IRQ_EINT1, NULL);
      free_irq(IRQ_EINT2, NULL);
      //free_irq(IRQ_EINT3, NULL);
      //free_irq(IRQ_EINT4, NULL);
      //free_irq(IRQ_EINT5, NULL);
      printk(KERN_INFO "%s successfully removed\n", DRV_NAME);
      //*/
   }

   module_init(init);
   module_exit(cleanup);
   /* Get rid of taint message by declaring code as GPL. */
   MODULE_LICENSE("GPL");
   MODULE_AUTHOR(DRIVER_AUTHOR);		/* Who wrote this module? */
   MODULE_DESCRIPTION(DRIVER_DESC);	/* What does this module do */
   MODULE_SUPPORTED_DEVICE("testdevice");

   ```
