 # 5주차 
 - 목차 
  1. ㅁㄴㅇ 
  2. ㅁㄴㅇ
  3. 
  4. 
  5. 
 - [가일의 임베디드 스쿨 참조](https://blog.naver.com/guile21c)
 
 
 #
 ## 환경설정
  - 강사님의 Makefile(disasm 나오는 코드)
   ```
   ##########[Embedded C test firmware Makefile]##############
   #
   # NAME : Makefile - S3C2450 test Firmware Makefile
   # Brief history
   #----------------------------------------------------------
   #
   #	2015.08.10, Seonghye : Modified
   #
   ###########################################################

   .EXPORT_ALL_VARIABLES:

   ## If you want to change path, modify here

   TOPDIR =$(PWD)
   TOOLPATH = /opt/CodeSourcery/Sourcery_G++_Lite

   SRCS	= libc.c  Main.c Uart.c exception.c
   ASRCS	= s3c2450_startup.S libs.S

   OBJS	= ${SRCS:.c=.o} ${ASRCS:.S=.o}

   CC = $(TOOLPATH)/bin/arm-none-eabi-gcc
   LD = $(TOOLPATH)/bin/arm-none-eabi-ld
   OBJCOPY	= $(TOOLPATH)/bin/arm-none-eabi-objcopy
   OBJDUMP	= $(TOOLPATH)/bin/arm-none-eabi-objdump

   LIBCDIR =$(TOOLPATH)/arm-none-eabi/lib
   LIBGCCDIR =$(TOOLPATH)/lib/gcc/arm-none-eabi/4.5.2
   LIBC =$(TOOLPATH)/arm-none-eabi/lib/libc.a
   LIBGCC = $(TOOLPATH)/lib/gcc/arm-none-eabi/4.5.2/libgcc.a

   ## User library for UART1 Driver
   MY_LIB_PATH = $(TOPDIR)/Libraries
   LIBUART =  $(MY_LIB_PATH)/libUart1.a

   #### Option Definition ####
   INCLUDE	=  -I$(TOPDIR) -I$(LIBCDIR)/include -I$(LIBGCCDIR)/include

   CFLAGS	+= $(INCLUDE) -g -Wall -Wstrict-prototypes -Wno-trigraphs -O0
   CFLAGS	+= -fno-strict-aliasing -fno-common -pipe
   CFLAGS += -march=armv4t -mtune=arm9tdmi -fno-builtin -mapcs

   LDFLAGS	= --cref -Bstatic -nostartfiles -T S3C2450-RAM.ld -Map 2450main.map
   OCFLAGS = -O binary -R .note -R .comment -S

   2450TEST = MDS2450.bin

   %.o:%.S
      $(CC) -c $(CFLAGS) -o $@ $<

   %.o:%.c
      $(CC) -c $(CFLAGS) -o $@ $<

   all: $(2450TEST)

   $(2450TEST) : $(OBJS)
      $(LD) $(LDFLAGS) -o MDS2450 $(OBJS) $(LIBC) $(LIBGCC) \
      -I$(LIBGCCDIR)/include -I$(LIBCDIR)/include -L$(LIBC) -L$(LIBGCCDIR) -lgcc

      $(OBJCOPY) $(OCFLAGS) $(TOPDIR)/MDS2450 $(TOPDIR)/$@
      $(OBJDUMP) -d $(TOPDIR)/MDS2450 > $(TOPDIR)/MDS2450.dis
      cp $(TOPDIR)/$@ /tftpboot

   clean:
      rm -f *.o 
      rm -f $(TOPDIR)/$(2450TEST)
      rm -f $(TOPDIR)/MDS2450
      rm -f $(TOPDIR)/2450main.map
      
   dep:
      $(CC) -M $(INCLUDE) $(SRCS) $(ASRCS) > .depend

   ifeq (.depend,$(wildcard .depend))
   include .depend
   endif					

   ```
 
 #
  ## 이번주 학습내용 Overview
   - 이번주 학습목표 : 하드웨어 제어에 대한 개념(감잡기)확립, 프로세서에 대한 이해

     - 레지스터의 3가지 종류 
       1. 범용 레지스터
       2. 제어용 레지스터
       3. 상태 레지스터
	  - 산술&논리 연산 장치(ALU) : 산술연산과 논리연산을 수행해서 레지스터에 저장
     - 버스의 3가지 종류
       1. 데이터 버스
       2. 어드레스 버스
       3. 제어 버스
     - 파이프라인 : 프로세서 내부에서 명령어들이 처리되는과정을 좀더 빠르고 효율적으로 수행하는 기법
     - 구동 프로그램
       1. 머신랭귀지 : 기계어
       2. 니모닉 코드 : 유사 어셈블리어
       3. 어셈블리어 : 니모닉 코드 수준으로 직접 프로그래밍 할수 있게 만든 언어
     - 어셈블리어 특징(장점만 적겠슴.. 강사님께서 어셈블리어를 매우 좋아하심)
       - 기계어에 비해 상대적으로 이해하기 쉽다..(02ea13f98f 이런것보다야..)
       - 가장 기본적인 수행 단계를 이해할 수 있다
       - 고급언어보다 이해하고 사용하기 어렵다(는 편견이다) -> 오히려 C보다 문법적 요소가 적다
       - 해당 아키텍쳐의 구조 및 동작에 대한 깊은 이해가 필요하다
       - 아키텍쳐마다 다르기 때문에 배우기 어렵다(는 편견이다) -> 한번 배워두면 다른 프로세서의 어셈블리어 배우기가 매우 편해진다
    - 버스란: 신호선의 집합
       1. 어드레스 버스
       2. 제어버스
       3. 데이터 버스
    - 폰노이만 아키텍쳐 VS 하버드 아키텍쳐
       - 데이터와 명령어 버스가 분리된 경우 하버드, 아닌겅우 폰노이만
    - 저장장치의 물리적 구성
       - [RAM]
         1. SRAM : flip-flop으로 구성(NOT Gate 2개의 Shift Registor)
         2. DRAM(SDRAM, DDR 포함) : Cap으로 구성됨, Refresh 필요
       - [Storage]
       3. EEPROM
       4. Flash - NAND
       5. Flash - NOR
    - 사담으로 항상 느끼는건데 ROM,RAM,주기억장치 보조기억장치 이런 항목들은 너무 오래되고 시대에 한참 뒤떨어진 분류기준인듯 하다. 주기억장치가 주로 기억을 담당하는건지 CPU가 요청하는 정보를 주로 전달해주는지도 명확하지 않다. 따라서 필자는 여기서, 단순히  RAM과 Storage 두가지로 구분하겠다. 
     - IO자원관리는 세가지 방식이 주로 사용된다
       1. polling
       2. interrupt
       3. DMA
 
 #
  ## ARM Architecture  
   - ARM Architecture 
  

    - 리틀엔디안과 빅엔디안에 관하여
      - 리틀엔디안용 하드웨어가 별도로 존재하고 (메모리 배선에 의존적)
      - 리틀엔디안용 컴파일러 또한 별도로 존재한다.   
    - .global 키워드의 의미
      - 없으면 에러 발생 ...
      - 유사어셈블러 sudo 코드
    - b. 의미 while(1)
      - c언어에서의 무한루프
    - 다양한 명령어 지원
      1. ARM 명령어
      2. Thumb명령어 / (Thumb-2 명령어)
      3. jazell 코어
    - 다양한 인터럽트 : IRQ, FRQ
      - FRQ 는 고속 인터럽트 : Low Latency
      - ARM9이전 : PIC(프로그램 인터럽트), 이후 : VIC(벡터 인터럽트 컨트롤러) -> NVIC로 진화
    - 프로그래머가 ASM 작성시 필요한 정보들 : Programmer's model
      1. 명령어
      2. 메모리 구조
      3. 데이터 구조
      4. 프로세서의 동작 모드
      5. 프로세서 내부 레지스터 구성 및 사용법
      6. Excption 처리
      7. 인터럽트 처리
    - 대부분의 회사에서는 코딩할사람은 많다. 문제해결할 사람은 적다.
      - 문제를 해결할수 있는 해결사가 되자
    - ARM의 명령어 3가지 종류
      1. ARM명령어
      2. Thumb/Thumb2
      3. jazzle
    - 명령어 관련한 알아둘 사항들
      1. Load/Store 만으로 메모리접근
      2. Branch 명령어(c언어의 goto)
      3. 상대주소 방식
      4. asm에서의 상수는 immedeate상수 -> #문자, 해시문자 사용
      5. 32비트 고정 명령어 길이 사용 : pipeline 구성이 용이 = RISC 프로세서 특징
      6. CISC프로세서는 가변길이 명령어(Veriable operand length)
      7. 어셈블리어를배우는것은 임베디드의 개념을 탑재한 사람이 되는것
      8. 기본개념(체계, system)이 중요한 이유는 문제해결
    - 명령어 처리 절차(3단계)
      1. Fetch : 코드읽기
      2. Decode : 코드해독
      3. Execute : 코드 실행
        - 파이프라인 프로세서 : 매순간 매 클럭마다 3단계의 모든 실행기들이 오퍼레이션 동작 수행 중
        - 프로세서가 매우 고효율로 동작할 수 있다.
        - Like as 분업화

    - Java명령 : jazelle core 확장시 자바 명령어 수행 가능
    - Latency, througput, bandwidth 차이점
      - Latency : 지연시간, 외부 인터럽트 요청으로부터 실제 ISR cord실행시까지 걸리는 지연시간
      - througput : 시간당의 CPU 처리량, CPU의 관점에서 시간당 데이터 러리할 수 있는 능력에 대한 지표
      - bandwidth : 시간당의 회선의 데이터 전송량 , 회선의 관점에서 시간당 데이터를 전송 해 줄 수 있는 능력에 대한 지표

    - 빅엔디안 VS 리틀엔디안
      - 모토롤라 CPU VS 인텔 CPU
      - 애플 PC VS IBM PC
      - ARM프로세서는 후발주자로써, 리틀엔디안 빅엔디안 모두 대응할 수 있도록 제작함.
      - 단, s3c2450은 언얼라인드 엑세스 비허용함, 메모리는 반드시 4의 정수배로만 읽거나 쓸 수 있다. - Data abort 발생
      - 동작모드(Operation Mode)
        - User Mode : 유일한 비특권 모드. IO와 RAM에 접근에 제한이 있다
        - FIQ : 빠른 인터럽트 처리를 위한 모드
        - IRQ : 일반적으로 사용되는 인터럽트 처리를 위해 프로세서의 동작 모드
        - SVC : 시스템 자원을 관리할 수 있는 프로세서의 동작 모드 슈퍼바이저 모드임
        - 이밖에도 abort, undefined, System, Exception 이 있음
        - Operating 모드 별 레지스터 : 6가지 모드별 37개의 레지스터가 존재한다.
        - 참고로,  IO 맵핑레지스터와 cpu 연산에서의 레지스터가 존재한다
        
    




  # 어셈블리 코딩에 관하여(ARM11 기준)
   ## 어셈블리어 규칙 - 레이블 작성법
     1. 알파뉴메릭(_포함)으로 작성
     2. 1번 COL 에 작성
     3. 콜론은 있을수도 없을 수도...
     4. 공백문자, 탭문자는 하지말아 ->> 컴파일러마다 다름.
     5. 어셈블리 코드에서는 레이블 꼭 필요함 
     6. 레이블만 첫번째 칼람에 작성
     7. 나머지는 탭문자 한칸 삽입 후 작성
     8. 일반적으로 탭 대신 스페이스바 추천 이유는 편집기마다 탭길이가 변경됨
	   -> 탭대신 space2로 변경, 공백문자2개 사용함.
     9. 오퍼렌드는 대소문자 섞어쓰지마 소문자 추천 레지스터도 소문자(일반적)
     10. 오피코드와 오퍼렌드 사이에는 분명하게 공백문자 삽입해야함.
     11.   bl HOW_TO_RETURN @ branch with Link
     12. mov pc,lr @ return명령어
  ## 어셈블리어 규칙 - 1
   ```c
	HOW_TO_RETURN(1,2,3,4);
   ```
   ```a
	mov	r0, #1
	mov	r1, #2
	mov	r2, #3
	mov	r3, #4
	bl	HOW_TO_RETURN
   ```
  ## 어셈블리어 규칙 - 2
   ```C
	HOW_TO_RETURN(1,2,3,4,5,6,7);
   ```
   ```m
	mov	r3, #5
	str	r3, [sp, #0]
	mov	r3, #6
	str	r3, [sp, #4]
	mov	r3, #7
	str	r3, [sp, #8]
	mov	r0, #1
	mov	r1, #2
	mov	r2, #3
	mov	r3, #4
	bl	HOW_TO_RETURN
   ```
  ## 어셈블리어 규칙 - 3

   ```
	Uart_Printf("result =%d ",HOW_TO_RETURN(1,2,3,4,5,6,7));
   ```
   ```
	.loc 1 28 0
	ldr	r0, .L2+8
	bl	Uart_Send_String
	.loc 1 29 0
	mov	r3, #5
	str	r3, [sp, #0]
	mov	r3, #6
	str	r3, [sp, #4]
	mov	r3, #7
	str	r3, [sp, #8]
	mov	r0, #1
	mov	r1, #2
	mov	r2, #3
	mov	r3, #4
	bl	HOW_TO_RETURN
   ```
idr r3, 


idr r3, [pc, #16]

@상대 주소
@*(ptr+16)




	cmp r0,r1
	blt L1
	bgt L2
	beq L3
	b _EXIT

L1:
	mov r0,#1
	mov pc,lr

L2:
	mov r0,#2
	mov pc,lr

L3:
	mov r0,#3
	mov pc,lr

_EXIT:
	mov r0,#-1
	mov

====================================================

cmp r0, r1
//비교결과에 따라서
movlt r0,#1
//맞으면 1 아니면 skip


movgt r0,#2
//맞으면 2 아니면 skip


moveq r0,#3
//맞으면 3 아니면 skip

mov pc,lr
=============================
<디스어셈블 코드>

30001430 <CONDITIONAL_EXECUTE>:
30001430:	e1500001 	cmp	r0, r1
30001434:	b3a00001 	movlt	r0, #1
30001438:	c3a00002 	movgt	r0, #2
3000143c:	03a00003 	moveq	r0, #3
30001440:	e1a0f00e 	mov	pc, lr
===============================
모든 명령어를 lt(조건부접미사)를 사용-> 성능도잡고 코드사이즈도 잡고, 두마리 토끼



[실습결과
=======================

mov r0 r2 lsl

<도움이 되는 지문> 책 페이지 : 
DATA_PROCESS1:
	/*
	 * IMPLEMENT function for calcurate result=(a+b)- c in this location
	 * use R3 Destination Register(Rd)
	 * use ADD, SUB and MOV instruction : 3 line
	 */

	 add r0,r0,r1
	 sub r0,r0,r2
	 mov pc, lr	/* return */

	/* 
	 * unsigned long DATA_PROCESS2(unsigned long a, unsigned long b);
	 * -----------------------------------------------------------------
	 *    r0=a, r1=b
	 *    The return value is result (r0)
	 */
	 .globl	DATA_PROCESS2
DATA_PROCESS2:	 
	/*
	 * IMPLEMENT function for calcurate result=(a<<2) | (b&15) in this location
	 * use R2 Destination Register(Rd)
	 * use AND, ORR and MOV instruction : 3 line
	 */
   mov r0,r0,lsl #2
	 and r1,r1,#15
	 orr r0,r0,r1
	 mov pc, lr	/* return */	
 
=======================
 
   and r2,r1,#15
   mov r0,r0,lsl #2
	 orr r0,r2,r0
	 mov pc, lr	/* return */	
 =====================
 ==========================================================



#if 0
	/* IMPLEMENT the function for calcurate result=(a+b)-c */
{
	int a,b,c;
	int result;
	
	a = 11;
	b = 22;
	c = 30;
	Uart_Printf("Calcurate [(%d+%d)-%d] ",a,b,c);
	result = DATA_PROCESS1(a,b,c);
	Uart_Printf("=> Result is %d (must 3)\n",result);	
}
#endif

#if 1
	/* IMPLEMENT the function for calcurate result=(a<<2) | (b&15) */
{
	unsigned long a,b;
	unsigned long result;
	//논리 연산 실습 : result=(a<<2) | (b&15)
	a = 0x10;
	b = 0x33;
	/*
	 * unsigned long function(a,b)
	 */
	Uart_Printf("Calcurate [(0x%08x << 2) | (0x%08x & 15)] \n",a,b);	
	result = DATA_PROCESS2(a,b);
	Uart_Printf("   => Result is 0x%08x (must 0x00000043)\n",result);	
}
#endif

===============
if( !(status^0x1234))
if(staus = 0x1234)
===============
1.
mvneq r1,#@ @0xffffffff(해결가능)
mveq rq,0xffffff(문법오류)

2.mvneq r1,#@ @0xff(해결가능) 
 = 0xffffff00 :: 으로 대체 가능

=======================



	 .globl	DATA_PROCESS2
DATA_PROCESS2:	 
	/*
	 * IMPLEMENT function for calcurate result=(a<<2) | (b&15) in this location
	 * use R2 Destination Register(Rd)
	 * use AND, ORR and MOV instruction : 3 line
	 */
   mov r0,r0,lsl #2
	 and r1,r1,#15
	 orr r0,r0,r1
	 mov pc, lr	/* return */	
 
	 mvneq r1,#0xffffff01 @(해결가능)

   and r2,r1,#15
   mov r0,r0,lsl #2
	 orr r0,r2,r0
	 mov pc, lr	/* return */	


디스어셈블
300013d0 <DATA_PROCESS2>:
300013d0:	e1a00100 	lsl	r0, r0, #2
300013d4:	e201100f 	and	r1, r1, #15
300013d8:	e1800001 	orr	r0, r0, r1
300013dc:	e1a0f00e 	mov	pc, lr
300013e0:	03a010fe 	moveq	r1, #254	; 0xfe
300013e4:	e201200f 	and	r2, r1, #15
300013e8:	e1a00100 	lsl	r0, r0, #2
300013ec:	e1820000 	orr	r0, r2, r0
300013f0:	e1a0f00e 	mov	pc, lr

=============================================


1단계.find OP2
2단계.operation(OP code)
3단계.save(register write)

OP코드 Rd, Rs, OP2
--------------------
add r0,r0,#1  		@r0=r0+1
add r0,r0,r0  		@r0=r0+r0=2r0
add r0,r0,r0,lsl #1  		@r0=r0+2r0=3r0
add r0,r0,r0,lsr #1  	@r0=r0+1/2r0=1.5r0
add r0,r0,r0,asr #1  	@r0=r0+1/2r0=1.5r0(부호)
add r0,r0,r0,lsl r1  		@r0=r0+(2^r1)*r0
add r0,r0,r0,rrx
================================================

add r0,r0,#1  		@r0=r0+1

add r0,r0,r0  		@r0=r0+r0=2r0
// 곱셈!
add r0,r0,r0,lsl #1  		@r0=r0+2r0=3r0
add r0,r0,r0,lsr #1  	@r0=r0+1/2r0=1.5r0
add r0,r0,r0,asr #1  	@r0=r0+1/2r0=1.5r0(부호)
@>>lsr과 asr의 차이 : lsr은 논리라서 
@>>부호있는 정수값일때 asr

add r0,r0,r0,lsl r1  		@r0=r0+(2^r1)*r0
@>>동일한 lsl이지만,레지스터 사용

add r0,r0,r0,rrx
@비트 회전 로테이트
================================================
<너무 큰숫자라 에러날때 mov대신 사용>
ldr r0,=0x1230 @0xff000000//지시어, 유사명령어(.  = ) 해독
================================================

MSR cpsr_fsxc, r0 : 
@ cpsr 영역  업데이트
MSR cr_fsxc, r0
@ cr 영역  업데이트
MSR c_fsxc, r0
@ c 영역  업데이트
MSR r_fsxc, r0
@ r 영역  업데이트
==========================
disableIRQ();
----
  .global disableIRQ()
disableIRQ():
  MRS r0, cpsr
  orr r0,r0,#0x80
  MRS cpsr_c,r0
  mov pc, lr
==========================================
EnableIRQ();
----
  .global EnableIRQ()
EnableIRQ():
