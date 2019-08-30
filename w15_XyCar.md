# 16주차 강의 : XyCAR 활용 자율주행 모형차 실습 
 ## 목차
  - 1일차
    - 지난주 리뷰
    - 팀별 프로젝트 플랜소개
    - 소프트웨어 설치 : Jetpack4.2.1+ROS Melodic
    - turtlebot3_Fake
  - 2일차
    - 자이트론 대표님 교육 -> 소개및 사용법 실제모형
  - 3일차
    - AI AutoCAR 기반 카메라 차선인식 기반 주행
    - Turtlebot3_fake2
  - 4일차 
    - TensorRT_ROS 차의 형태의 가제보 + 라이다 시뮬레
 ## 1일차
  - 강사님의 SSD Training export [Link!](https://github.com/katebrighteyes/ssd_traing_export)
  - [나의 깃허브 Mirror](https://github.com/d-h-k/ssd_traing_export?organization=d-h-k&organization=d-h-k)
 ## 1일차 수업
  - Jetson nano Deepstream 설치방법
    - [링크](https://github.com/katebrighteyes/JetsonDeepStream/blob/master/ssdufftest/readme_for_nano_deepstream.txt)
    - 위 링크는 자비에 혹은 TX1,2 또한 마찬가지 
    - UFF 사용법은 공통! - 모든 엔비디아 플랫폼 기반 
    - 나노 TX2는 10.0 자비에만 10.1
    - 나노에 ROS melodic 설치 방법
        ```
        sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
        ```
        ```
        sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
        ```
        ```
        sudo apt update
        ```
        ```
        sudo apt install ros-melodic-desktop
        ```
        
        run

    - 식당 서빙로봇 : 흥미, 재미, 정리할수 있고, 포트폴리오 가치있는? BM 도 고려



        gi

