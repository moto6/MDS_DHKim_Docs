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
- [초보자를 위한 임베디드 리눅스 학습 가이드
](http://ebook.pldworld.com/_eBook/Embedded/embedded_guide-v1.1_html.htm)
