
# MDS2450 개발보드에 관한 정보를 공유합니다

- Intro & Index
```
스위치를 기능 수행을 위해 아래의 과정이 요구됩니다.
1. 스위치 관련 Pin들의 GPXCON 설정(Configration)
2. 스위치 HW 에 관련한 순차 접근 ★
3. 스위치 관련 Pin들의 GPXDAT 접근하여 Data 읽기
```


## 1. 스위치 관련 Pin들의 GPXCON 설정(Configration)
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

- ()사진넣어줭
- 글적어줭



 ## 2. 스위치 HW 에 관련한 순차 접근 ★
 * 스위치 하드웨어( 교재 161p ) 책을 펼처놓고 진행을 추천합니다
 

 * 하드웨어 설명
    ```
    1. GPF0, GPF1에 연결된 SW14, SW15는 GPFDAT의 레지스터 값을 읽어 해당하는 PIN의 위치에 '0'으로 Clear 되어있는지 확인하면 됩니다.
    2. GPF2 ~ GPF6 까지 5개의 Pin에 10개의 스위치가 연결되어 있습니다.
    3. GPF7과 GPG0 을 사용하여 ->> SW4~SW8 스위치가 눌렸는지, SW9~SW13 스위치가 눌렸는지를 구별할 수 있습니다
    ```

    위의 설명에 해당하는 코드입니다
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


    3.Timer
    =============
    ```
    강사님의 코드를 추천드려요
    ```
