# make로 STM32시리즈 펌웨어 개발하기
  - 이 문서는 makefile 유틸리티로 STM32시리즈 펌웨어 개발하는 방법을 공유합니다
  - 윈도우10 에서 리눅스와 거의 유사하게 Make기반 프로젝트를 생성해서 컴파일하는 튜토리얼 입니다.
  - 만들어진 해당 프로젝트는 Windows, MAC, Linux(우분투)모든 OS에서 컴파일이 가능합니다.
  - 영문버전 문서는 여기로 [English Version]()
  - 개발환경은 아래와 같습니다
    - OS : windows10 64bit
    - MCU : STM32L476VGT - Discovery board
    - 이외 사용 툴 : STM32CubeMX, Visual studio Code, (옵션 : Vim)
 #
 ## 0. Overview
   1. Makefile 설치
   2. GCC ARM Cross compiler 설치
   3. CubeMX에서 makefile 프로젝트 생성
   4. 구동 및 테스트
 #
 ## 1. Makefile 설치
   - CMake를 설치합니다 [여기링크](https://cmake.org/download/)에서 CMake Windosw버전을 클릭하여 다운로드 합니다
   ![img](/img/20190621-no006.png)
   - 위 사진에서 windows OS를 위한 executable installer 를 다운받으시면 됩니다
   - CMake 설치과정은 [여기링크](https://westwoodforever.blogspot.com/2013/04/cmake-windows.html)를 참고하시면 됩니다
 #
 ## 2. GCC ARM Cross compiler 설치
   - GCC ARM Cross compiler 설치를 위해 [여기링크](https://launchpad.net/gcc-arm-embedded/+download)를 눌러 GCC ARM Cross compiler 를 다운받으면 됩니다. 글 
   - 작성시점(2019-06-21) 기준 아래 이름의 파일 입니다
      ```
      gcc-arm-none-eabi-5_4-2016q3-20160926-win32.exe
      ```
   ![img](/img/20190621-no007.png)
   - 컴파일러 설치과정은 [여기링크](https://ddnemo.tistory.com/97) 를 참고하시면 됩니다
   - 컴파일러 설치가 완료되면 ARM Cross compiler 의 환경변수를 등록 해 줍니다
   > 컴파일러의 환경변수 등록이 되지 않으면, 에러가 발생하므로 꼭 진행해야 합니다!! (많은 분들이 컴파일러의 환경변수 등록을 하지않는 에러를 겪으십니다..)
   - 
   ```
   C:\Program Files (x86)\GNU Tools ARM Embedded\5.4 2016q3\bin
   ```
   - [Windows키] + R을 눌러 실행창에서 "cmd"를 입력합니다
   ```
   cmd
   ```
   ```
    arm-none-eabi-gcc -v
   ```
   ```
    C:\Users\dhkim>arm-none-eabi-gcc -v
    Using built-in specs.
    COLLECT_GCC=arm-none-eabi-gcc
    COLLECT_LTO_WRAPPER=c:/program\ files\ (x86)/gnu\ tools\ arm\ embedded/.4\ 2016q3/bin/../lib/gcc/arm-none-eabi/5.4.1/lto-wrapper.exe Target: arm-none-eabi
    
    ~ <<<<중략>>>>> ~

    usr --with-host-libstdcxx='-static-libgcc -Wl,-Bstatic,-lstdc++,-Bdynamic -lm' --with-pkgversion='GNU Tools for ARM Embedded Processors' -with-multilib-list=armv6-m,armv7-m,armv7e-m,armv7-r,armv8-m.base,armv8-m.main Thread model: single
    gcc version 5.4.1 20160919 (release) [ARM/embedded-5-branch revision 240496] (GNU Tools for ARM Embedded Processors)

   ```
  - 위와같은 메세지를 만나지 못한경우, 2번항목을 다시 반복하여 문제를 해결해야 합니다.
 #
 ## 3. CubeMX에서 makefile 프로젝트 생성
   - 
   - 
   -  
 #
 ## 4. 구동 및 테스트
   - 윈도우키+R 키를 눌러 실행창에서 "cmd"를 입력하여 커맨드 창을 불러옵니다
   ![img](/img/20190621-no001.png)
   - 커맨드 창에서 make를 입력합니다
   ![img](/img/20190621-no002.png)
      ```
      make
      ```
   - make (프로젝트 빌드)가 완료된 모습입니다
   ![img](/img/20190621-no003.png)
   - make를 한번 더 콘솔에 입력하면, 변경점 없음을 인식한 make 유틸리티가 컴파일을 다시 하지 않는것을 확인합니다.
   ![img](/img/20190621-no004.png)
   - 프로젝트를 재 컴파일 하기 위해서는 make clean 명령어를 입력합니다
   ![img](/img/20190621-no005.png)
      ```
      make clean
      ```
   


 