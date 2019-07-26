# 11주차 ROS(Robot Operating system) 학습정리
  - 목차
    - [Day1](https://github.com/d-h-k/MDS_DHKim_Docs/blob/master/w11_ROS.md#day1) 
    - [Day2](https://github.com/d-h-k/MDS_DHKim_Docs/blob/master/w11_ROS.md#day2) 
    - [Day3](https://github.com/d-h-k/MDS_DHKim_Docs/blob/master/w11_ROS.md#day3) 
    - [Day4](https://github.com/d-h-k/MDS_DHKim_Docs/blob/master/w11_ROS.md#day4)
    - [Day5](https://github.com/d-h-k/MDS_DHKim_Docs/blob/master/w11_ROS.md#day5)
   - 리뷰 
     ```
     sdfsdf
     ```
  #    
  ## Day1
   - ROS2.0 버전에 대하여 [잘 정리된 링크](http://snowdeer.github.io/ros2/2017/12/18/introduction-ros2/)
     - 현업에서 사용경우 극히 제한적
     - 리얼타임 지원
       - 이전버전 : 1000 ms 단위 최대 시간지연 보장
       - Softreal : 100ms 단위 최대 시간지연 보장
       - Hardreal : 10ms 단위 최대 시간지연 보장
     - 리얼타임의 종류 : Hard/soft/firm
     - non-real 타임의 결정 : jitter,지터란 일정하지 않은 시간의 지연
   - CMake : ROS에서는 주로  catkin 으로 빌드하며 이는 내부적으로 make로 구성되어짐 [강좌](http://egloos.zum.com/ttti07/v/4182865)
   - [ROS in Raspberry - 오로카](https://cafe.naver.com/ArticlePrint.nhn?clubid=25572101&articleid=14308)
   - [ROS Wiki](http://wiki.ros.org)
   - [파이썬 공부](https://wikidocs.net/book/110)
   - [로보티즈의 터틀봇 위키](https://github.com/ROBOTIS-GIT/turtlebot3)
   - 빌드 환경 정리
        ```
        0. source /opt/ros/indigo/setup.bash
                    <= 기본 설치된 ros 패키지 라이브러리 경로 설정
                    <= 교재 14페이지 : 볼드채 굵은 글씨 첫 번째 줄
                    <= 교재 7페이지 : 볼드채 굵은 글씨 맨아래 줄
                    <= ~/.bashrc 파일 -> 새로운 창을 열면 자동 적용 

        1. catkin_make <= 워크 스페이스 최상위 폴더상에서 진행
                    <= 컴파일 옵션이 동일한 코드 들을 묶음
                    <= 컴파일(빌드 환경) 을 제공하는 역할
                    <= 교재 14페이지 : 볼드채 굵은 글씨 맨 아래 줄
                    <= clean 

        2. source devel/setup.bash
                    <= 빌드 되어진 커스텀(유저 개인) 정보를 
                        환경 설정하는 스크립트 실행 명령어
                    <= 교재 15페이지 : 볼드채 굵은 글씨 첫 번째 줄

        3. rosrun PACKAGE EXECUTABLE [ARGS] 

        rosrun rospy_tutorials talker
        rosrun basics topic_publisher.py 

                    <= 노드를 실행하는 명령어 소속 패키지를 써야 함
                    <= 교재 18페이지 : 볼드채 굵은 글씨 첫 번째 줄

        4. rosrun rospy_tutorials talker 
        roscd rospy_tutorials 
                    <= /opt/ros/indigo/share/rospy_tutorials
                    <= 0.번 명령어 실행으로 경로가 환경변수에 있음 

        5. 워크 스페이스는 컴파일 옵션이 동일한 빌드 환경을 제공

        6. 패키지는 같은 분류로 묶어질 노드 를 묶는 역할
        노드는 패키지에 속하게 된다. 

        7. 노드는 단독 실행이 가능한 실행가능한 파일 => EXECUTABLE

        ```
   - ROS의 미래지향적 산업분야 : 자율주행차, 드론, 협동로봇
   - ROS 의 핵심키워드 : 협업, 시간절약(코드재사용), SW플랫폼 
   - [ROS용 모터 컨트롤러 위키](http://wiki.ros.org/Motor%20Controller%20Drivers)
   - ROS를 활용하는 방법 5가지
     1. 이미 개발된 기능 활용 : 모터드라이버, 액추에이터, 로봇시스템 등등
     2. 지도 활용 가능
     3. 데이터 분산처리
     4. 로봇의 상태와 알고리즘
     5. 위키 문서들, 생태계 기 구축됨
   - Do ROS : 로봇 프로그래밍에 관심있는 사람들
   - Do not ROS : 로봇 기구학은 다루지 않음
   - ROS의 개발 철학 5가지
     1. p2p : 작은 프로그램 여러개가 모여 하나의 프로그램을 이루므로 확장성이 좋다
     2. 도구기반 : IDE같은것은 없지만 Make파일과 GCC 컴파일러 등 기존에 구현된 툴킷을 활용
     3. 다중언어 : C/C++,Python 등등 다양한 언어로 활용 가능 하지만.... C++과 py가 대부분인듯 함 
     4. 가벼움 : 동의하지는 않지만, 가볍다고 자랑함(대조군을 정의해줬으면 좋겠다..)
     5. 무료 및 오픈소스 : 오픈소스는 스카이넷이야!
   - 괄목할만한 발전을 이루는 중
   - 노드의 본질은 POSIX 프로세스, 연결선은 TCP 이다.
   - 하나의 로봇은 오로지 하나의 Roscore만을 갖는다  
   - 오늘 사용한 주요 명령어
        ```
        cd ~/catkin_ws/src/
        catkin_create_pkg basics rospy
        chmod u+x topic_publisher.py 
        sudo powerof
        source devel/setup.bash
        roscore
        ```
   - ROS 프로그램의 이름을 바꾸는 방법 : 동일한 하드웨어를 하나의 로봇에서 두개 이상 사용할 때(ex- 왼쪽스피커, 오른쪽 스피커)
     1. 하나의 변수명만을 변경
     2. 네임스페이스 소속 변경
     3. 프로그램 자체의 변경
   - 실습 : 개발환경 구축, 예제실행, 토픽의 발생과 구독 실습 예정
  #
  ## Day2
   - fd
   ```
   sudo apt-get remove vim-common
   sudo apt-get install vim
      25  sudo apt-get remove vim-common
   26  sudo apt-get install vim
   27  ls
   28  vim
   29  sudo apt-get install build-essential
   30  gcc
   31  subl
   32* 
   33  sudo apt-get install cmake
   34  sudo apt-get install make
   35  sudo apt-get install miniterm
   36  sudo apt-get install terminator
   37  sudo apt-get install minicom
   ```
  #
  ## Day3
   - ROS
  #
  ## Day4
   - sdf
  #
  ## Day5 
   - sdf
