
# Week2_Embedded C Langguage
 - 이번주에는 임베디드 특화 C언어를 실습합니다.
 - 교보재로는 MDS2450 보드 []
 - Processor : SAMSUNG S3C2450
## Index
1. 
2. 

## MDS2450 보드로 실습환경 셋팅하는법 
 ### MDS2450 보드를 실습하기 위한 구축은 5단계로 나뉩니다. 
 1. VM(가상머신 환경 구축) : 호스트, 게스트간 공유폴더 설정
 2. Teraterm(Serial 터미널 프로그램) 설치 : RS232및 USART 를 이용, Bootloader, Prompt 환경 적응
 3. Linux(게스트, Ubuntu 10)에서 컴파일 환경 셋팅 : Make, ARM컴파일러 설치
 4. TFTP 를 활용한 xx.bin 파일 전송(binary)
 5. MDS2450 보드에서의 동작 확인 

### 환경 설정간 알아야 할 요소들
 [ 크로스 컴파일 환경, Ubuntu10 ]
 - 명령어 : pwd,cd (cd /:루트로 이동) 
 - root 는 수퍼유저임
 - FTP : file transfer protocol : 개발환경 PC에서 Target 보드로 컴파일 완료된 파일을 전송할 때 쓰는 규격임 ->> IP Addresss + File name만 가지고 처리함
 - 작업장 특성상 전송을 편하기 위해서 USB to LAN 케이블을 사용하며, 이를 윈도우즈에서 리눅스로 제어권 권한 변경을 해 주어야 함
 
 [ MDS2450 : Target board ]
 - printenv명령어 : 프롬프트의 설정된 기본값들을 화면에 표시함
 - tftp 30000000 MDS2450.bin : tftp포멧으로 전송 함
 -
 -
 -
 - 다운로드 자동화(Makefile & prompt 등록) :
 :"set bootcmd "tftp 30000000 MDS2450.bin;go 30000000"  ->>   "saveenv"


### Makefile 와 Make에 대하여 추가 공부 필요함

## volitaile 의미
1. 캐쉬메모리에서 값을 가져오지마라
2. 

==================C언어 심화

.

.

.

.


.



## 배열의 주소에 관하여 - 같지만 다른 표현방식

## code

---
int arr[5];

printf("%x \n",ary);//--------1

printf("%x \n",&ary[0]);//----2

printf("%x \n",ary+1);//------3

printf("%x \n",&ary + 1);//---4

---



