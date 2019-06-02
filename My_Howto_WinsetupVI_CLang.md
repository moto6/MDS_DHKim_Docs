[Windows 환경에서 C 컴파일하기 ( GCC컴파일러 + VIM편집기 ) ]
====
> # 서론
   > #### 서문 : MS Windows 환경에서 C컴파일을 할수 있는 다양한 방법이 있습니다. 대부분의 C기본서들은 Visual Studio 에서 VC++ 컴파일러를 활용하지만, 이 문서에서는 Like as Liunx환경과 같은 vim편집기와 GCC컴파일러를 활용 해 C언어 개발환경을 구축법을 알아보겠습니다.
   > * 준비물 : Windows 10이 설치된 PC
   > * [190601 수정사항]
      ```
      GCC 컴파일러 -> GCC = GNU Compiler Collection 의미이므로 중복되는 단어가 두번사용됨, 틀린표현이므로 "GCC"로 표현 변경
      ```
   

> # overview
 > 1. MinGW설치 (컴파일러)
 > 2. VIM 설치 (에디터)
 > 3. 환경변수 셋팅
   



> # 1. MinGW설치 (Windows에서 동작하는 GCC(컴파일러) 설치)
 > 1. (https://osdn.net/projects/mingw/releases/) <- 링크를 클릭하여 MinGW컴파일러를 다운받는다.
 > 2. 주의사항!! : 정말 많은 사람들이 실수한다 (사진삽입) 여기 있는 [*!!! MinGW Installation Manager (mingw-get)*] 항목을 클릭하여 다운로드 해야한다. 
 > 3. 