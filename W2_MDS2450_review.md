
# MDS2450 개발보드에 관한 정보를 공유합니다

- Intro & Index
  ```
  0.LED : 다들 잘 하므로 PASS

  1.Switch : Switch를 기능 수행을 위해 아래의 과정이 요구됩니다.
  - Switch 관련 Pin들의 GPXCON 설정(Configration)
  - Switch H/W 에 관련한 순차 접근 ★
  - Switch 관련 Pin들의 GPXDAT 접근하여 Data 읽기

  2. Timer : Timer0 ~ 4, 총 5가지 종류의 타이머가 존재하며, Timer4는 MPU의 Pin과 연결되지 않았으며, 내부적으로만 사용이 가능합니다.
  - 6가지 레지스터

  3. Example Code (Application)

  ```



# 1.Switch
  ```
  [ Switch ] : Switch를 기능 수행을 위해 아래의 과정이 요구됩니다.
  1. Switch 관련 Pin들의 GPXCON 설정(Configration)
  2. Switch H/W 에 관련한 순차 접근 ★
  3. Switch 관련 Pin들의 GPXDAT 접근하여 Data 읽기
  ```

  ## 1.1 스위치 관련 Pin들의 GPXCON 설정(Configration)

  - 코드먼저 보고 가실께요

  ```C
   void GPIO_Init(void);

   void GPIO_Init() 
   {
	    rGPGCON |= (0x55<<8); // LED Init 
   	    rGPGDAT |= (0x0f<<4); // LED Off
   
     	rGPGCON |= (0x01); //Switch downbelow Init
    	rGPFCON |= (0x01<<14); //Switch downbelow Init
   
    	rGPFCON &= ~(0x3ff << 4);
    	rGPFCON &= ~(0x2 << 14);
   
    	rGPFCON |= (0x3<<14);
    	rGPGCON |= (0x1);
   
    	rGPFCON |= (0x1<<14); //Switch Init(GPF7)
	    rGPFCON &= ~(0x1<<15); //Switch Init(GPF7)
    }
    //주석좀 달고 코드를 깔꿈하게 수정해야되는데..?
   ```
   * ()사진넣어줭

   * 글적어줭



  ## 1.2. 스위치 HW 에 관련한 순차 접근 ★
  * 스위치 하드웨어( 교재 161p ) 책을 펼처놓고 진행을 추천합니다
  * 하드웨어 설명
  ```
  1. GPF0, GPF1에 연결된 SW14, SW15는 GPFDAT의 레지스터 값을 읽어 해당하는 PIN의 위치에 '0'으로 Clear 되어있는지 확인하면 됩니다.
  2. GPF2 ~ GPF6 까지 5개의 Pin에 10개의 스위치가 연결되어 있습니다.
  3. GPF7과 GPG0 을 사용하여 ->> SW4~SW8 스위치가 눌렸는지, SW9~SW13 스위치가 눌렸는지를 구별할 수 있습니다
  ```

  ## 1.3. Switch 관련 Pin들의 GPXDAT 접근하여 Data 읽기
  ```c
    int Get_Key(void);//Prototype

    int Get_Key(void) {
	// Lonely Switch ======================================
    if ((rGPFDAT & 0x01) == 0x00) {//SW14
        return 14;}
	
    if ((rGPFDAT & 0x02) == 0x00) {//SW15
        return 15;}

	// First Line Switch ==============================
	rGPFDAT &= ~(0x1<<7);   //Clear GPF7
	rGPGDAT |= (0x1);       //Set GPG0
	
    if ((rGPFDAT & 0x04) == 0x00) { //SW9
        return 4;}
    
    if ((rGPFDAT & 0x08) == 0x00) {//SW10
        return 5;}
    
    if ((rGPFDAT & 0x10) == 0x00) {//SW11
        return 6;}
     
    if ((rGPFDAT & 0x20) == 0x00) {//SW12
        return 7;}
	
    if ((rGPFDAT & 0x40) == 0x00) {//SW13
        return 8;}
	
	// Second Line Switch =====================================
	
	rGPFDAT |= (0x1<<7);    //Clear GPF7
	rGPGDAT &= ~(0x1);      //Clear GPG0
		
    if ((rGPFDAT & 0x04) == 0x00) { //SW9
        return 9;}
    
    if ((rGPFDAT & 0x08) == 0x00) { //SW10
        return 10;}
    
    if ((rGPFDAT & 0x10) == 0x00) { //SW11
        return 11;}
     
    if ((rGPFDAT & 0x20) == 0x00) { //SW12
        return 12;}
	
    if ((rGPFDAT & 0x40) == 0x00) { //SW13
        return 13;}
	
	
	 	
	return 0;//SW 눌리지 않았을떄 0을 리턴
    }

  ```
  
   * 참 쉽쥬??
   


# 3.Timer
 ``` 
 [ Timer ] : Timer0 ~ 4, 총 5가지 종류의 타이머가 존재하며, Timer4는 MPU의 Pin과 연결되지 않았으며, 내부적으로만 사용이 가능합니다.
 1. TCFG0   : --
 2. rTCFG1  : --
 3. rTCON   : --
 4. rTCNTB0 : --
 5. rTCMPB0 : --
 ->> 총 6가지의 레지스터를 사용하여 TimerX 의 기능을 구현합니다.
  
  ※ 보다 자세한 사항은 S3C2450 Usermannul을 참고하십시오
 ```
 * 코드

  ```c
  //filename : Timer.c
  #include "2450addr.h"
  #include "option.h"
  #include "libc.h"

  //Function Declaration
  void Timer0_Init(void);
  void Timer0_Delay(int msec);
  //
  void Timer1_Init(void);
  void Timer1_Delay(int msec);
  //
  void Timer2_Init(void);
  void Timer2_Delay(int msec);
  //
  void Timer3_Init(void);
  void Timer3_Delay(int msec);
  //
  void Timer4_Init(void);
  void Timer4_Delay(int msec);


  //
  //
  //
  void Timer0_Init(void)
  {
    rTCFG0 |= 	(0xff);
	rTCFG1 |= 	(0x03); //manual update no operation, timer0 stop, TCNTB0=0, TCMPB0 =0
	rTCON |= 	(0x01<<3);
	rTCNTB0 &= 	0x00;
	rTCMPB0 &= 	0x00;
  }  

  void Timer0_Delay(int msec)
  {
  	rTCNTB0 =	16.113*msec; //  HCLK 133MHz ,Div 1/2 =62MHz , 16.113 clock =  ms 
	rTCON |=	(0x01<<1); // Set Bit TCON [1] ->> mannual update
	rTCON &= 	~(0x01<<1); // Clear Bit TCON [1] ->> no more update
	rTCON |=  	(0x01); // Start for Timer 0
	while(rTCNTO0); // Every (1/16.113) msec, TCNTB0 Counter value reduced by Timer 0
  }


  void Timer1_Init(void)
  {
	rTCFG0 |= 	(0xff);
	rTCFG1 |= 	(0x3<<4); //manual update no operation, timer0 stop, TCNTB0=0, TCMPB0 =0 ,Div1/16
	rTCON |= 	(0x01<<11);
	rTCNTB1 &= 	0x00;
	rTCMPB1 &= 	0x00; 
  }

  void Timer1_Delay(int msec)
  {
	rTCNTB1 =	16.113*msec; // HCLK 133MHz ,Div 1/2 =62MHz , 16.113 clock =  ms 
	rTCON |=	(0x01<<9); // Set Bit TCON [1] ->> mannual update
	rTCON &= 	~(0x01<<9); // Clear Bit TCON [1] ->> no more update
	rTCON |=  	(0x01<<8); // Start for Timer 1
	while(rTCNTO1); // Every (1/16.113) msec, TCNTB1 Counter value reduced by Timer 1
  }


  void Timer2_Init(void)
  { 
	rTCFG0 |= 	(0xffff);
	rTCFG1 |= 	(0x3<<8); //manual update no operation, timer2 stop, TCNTB2=0, TCMPB2 =0 ,Div1/16
	rTCON |= 	(0x01<<15);//Timer2 auto reload mode set
	rTCNTB2 &= 	0x00;
	rTCMPB2 &= 	0x00;
  }

  void Timer2_Delay(int msec)
  { 
	rTCNTB2 =	16.113*msec; // HCLK 133MHz ,Div 1/2 =62MHz , 16.113 clock =  ms 
	rTCON |=	(0x01<<13); // Set Bit TCON [1] ->> mannual update
	rTCON &= 	~(0x01<<13); // Clear Bit TCON [1] ->> no more update
	rTCON |=  	(0x01<<12); // Start for Timer 2
	while(rTCNTO2); // Every (1/16.113) msec, TCNTB2 Counter value reduced by Timer 2
  }


  void Timer3_Init(void)
  {
	rTCFG0 |= 	(0xffff);
	rTCFG1 |= 	(0x3<<12); //manual update no operation, timer2 stop, TCNTB2=0, TCMPB2 =0 ,Div1/16
	rTCON |= 	(0x01<<19);//Timer3 auto reload mode set
	rTCNTB3 &= 	0x00;
	rTCMPB3 &= 	0x00;
  }

  void Timer3_Delay(int msec)
  {
	rTCNTB3 =	16.113*msec; // HCLK 133MHz ,Div 1/2 =62MHz , 16.113 clock =  ms 
	rTCON |=	(0x01<<17); // Set Bit TCON [1] ->> mannual update
	rTCON &= 	~(0x01<<17); // Clear Bit TCON [1] ->> no more update
	rTCON |=  	(0x01<<16); // Start for Timer 3
	while(rTCNTO3); // Every (1/16.113) msec, TCNTB3 Counter value reduced by Timer 3
  }

  void Timer4_Init(void)
  {
	rTCFG0 |= 	(0xffff);
	rTCFG1 |= 	(0x3<<16); //manual update no operation, timer2 stop, TCNTB2=0, TCMPB2 =0 ,Div1/16
	rTCON |= 	(0x01<<22);//Timer4 auto reload mode set
	rTCNTB4 &= 	0x00;
	 
  }

  void Timer4_Delay(int msec)
  {
	rTCNTB4 =	16.113*msec; // HCLK 133MHz ,Div 1/2 =62MHz , 16.113 clock =  ms 
	rTCON |=	(0x01<<21); // Set Bit TCON [1] ->> mannual update
	rTCON &= 	~(0x01<<21); // Clear Bit TCON [1] ->> no more update
	rTCON |=  	(0x01<<20); // Start for Timer 4
	while(rTCNTO4); // Every (1/16.113) msec, TCNTB1 Counter value reduced by Timer 4
  }
  ```
  * 코드를 참고하세요
  * Unserstaing Timer : Link ( === )



# 4. Example Code (Application)

 asd