 # 5주차 
 - 목차 
  1. ㅁㄴㅇ 
  2. ㅁㄴㅇ
  3. 
  4. 
  5. 
 - [가일의 임베디드 스쿨 참조](https://blog.naver.com/guile21c)
 #
   ## ㄴㅇㄹ
     - ㄴㅇㄹㅁㄴㅇ
	 - ㄴㄴㅇㄹ



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
