## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [strlcpy](#strlcpy)
        - [함수의 기능](#함수의-기능)
        - [함수의 원형](#함수의-원형)
        - [헤더파일](#헤더파일)
        - [매개변수](#매개변수)
        - [반환값](#반환값)
        - [추가 설명](#추가-설명)
    - [strcpy의 문제점](#strcpy의-문제점)
    - [strlcpy와 strncpy 비교](strlcpy와-strncpy-비교)
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-02-08(월)<br/><br/>
## 2. 학습 시간
* 14:00 ~ 17:00 (자가)<br/><br/>
## 3. 학습 범위 및 주제
* strlcpy함수를 조사한다.
* strcpy, strncpy와의 차이점을 조사한다.<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* strlcpy 함수를 이해하고, strcpy 함수와 strncpy 대신 쓰는 이유를 알아본다.<br/><br/>
## 6. 상세 학습 내용
## strlcpy
strcpy 함수(src 문자를 대상 버퍼(dst)에 복사)를 대상 버퍼를 오버플로우 할 수 없는 보안 버전으로 대체하기 위한 것이다.  
버퍼 오버플로우를 방지하는 데 사용할 수 있는 표준 C함수인 strncpy는 올바르게 사용하기 어렵고 느리게 만드는 결함을 가지고 있다.  

**strncpy 에서는 source의 길이가 destination의 버퍼 길이와 같거나 더 길 경우, NUL-terminate되지 않는다.   
strncpy의 NUL-terminate를 보장하기 위한 함수 `strlcpy`**이다. `size-1` 만큼의 string 복사와 함께 NULL로 끝남을 보장해 준다.

 
`strn*`함수들이 `strl*`함수들보다 비효율적인 이유는 dst의 맨 마지막에 널을 하나만 넣어주느냐(strlcpy) dst의 남은 공간에 size까지 널을 넣느냐의 차이 같다.  
<br/>

### 함수의 기능
- dstsize가 0이 아닌 경우 `(dstsize - 1)`만큼의 문자를 src에서 dst로 복사 후 **NULL 종료**한다.
- src에서 dst로 값을 복사하는데 dstsize 길이 만큼 한다. 여기서 dstsize는 문자열 끝의 `NULL` 까지 **포함**한 길이를 넣어줘야한다.

<br/>

### 함수의 원형
```
size_t    strlcpy(char *dst, const char *src, size_t dstsize);
```
<br/>

### 헤더파일
```
C언어 : <string.h>
C++ : <cstring>
```
<br/>

### 매개변수
- char \*dst
```
수정 또는 추가되는 널로 끝나는 문자열
```
<br/>

- const char \*src
```
src은 항상 상수로 유지해야 하는 char형 ‘값'을 가리키는 포인터 변수이다.  
src가 가리키는 주소는 변경될 수 있다. 
```
<br/>

- size_t dstsize
```
- 널 문자를 포함한 복사할 문자열의 길이
- 버퍼의 크기
```
<br/>

### 반환값
- **src의 길이**
- 자료형 : **size_t**   
```
'unsigned int'

메모리나 문자열 등의 길이를 구할 때에는 `unsigned int` 대신 `size_t`라는 형으로 길이가 반환된다.  
```
<br/>

- 복사되어진 데이터의 길이이므로 NULL은 빠진다.  
`dstsize - 1` 값이라고 생각하면 이해하기 편할 것이다.  
<br/> 

### 추가 설명
- 만약 size에 1을 넣으면 dst에 NULL만 들어가있다.
- size에 2를 넣을 때부터 src 1바이트와 NULL이 dst에 들어가있다.
- dstsize가 0 또는 본래 dst 문자열보다 dstsize가 큰 경우 NULL 종료한다.  
(실제로 이것은 dstsize와 dst문자열이 올바르지 않음을 의미하기 때문에 일어나선 안되는 일이다.)  
- 만약 src와 dst 문자열이 겹치는 경우 동작이 정의되지 않는다.  

<br/><br/>

---
<br/><br/>

## strcpy의 문제점
### - strcpy 문제점
**복사될 메모리(dst)의 크기보다 원본 문자열(src)의 크기가 더 크면 버그(Bug)가 발생할 수 있다.**  
예를 들어 아래와 같이 코드를 구성하면 컴파일할 때는 오류가 나지 않지만 프로그램 실행 시에 오류가 나거나 프로그램이 오작동할 수 있다는 뜻이다.  
```
char str[4];

strcpy(str, "tipsware");    // 버그 발생
```
str 배열의 크기는 총 4바이트까지 문자열을 저장할 수 있는데 'tipsware' 문자열은 NULL 문자까지 포함하면 총 9바이트이다.  
그런데 이 9바이트 크기의 문자열을 4바이트 크기의 메모리에 저장하려고 하면 메모리 침범이 발생하기 때문에  
프로그램이 실행 중에 비정상 종료되거나 잘못된 조건으로 동작하게 된다.  
<br/>

### - strcpy 올바른 사용
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char dst[] = "BLACK";
    char src[] = "Hi";
    
    printf("strcpy : %s \n", strcpy(dst, src));
    return 0;
}
```
```
strcpy : Hi
```
<br/>

### - strcpy 오류
```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char src[] = "BLACK";
    char dst[] = "Hi";
    
    printf("strcpy : %s \n", strcpy(dst, src));
    return 0;
}
```
```
$ ls
$ a.out    <-  실행 파일은 만들어짐!
$ ./a.out
zsh: abort    ./a.out
```
<br/><br/>

---
<br/><br/>

## strlcpy와 strncpy 비교
### - strlcpy
- src길이가 dst보다 길 때
```
#include <stdio.h>
#include <string.h>

int main(void)
{
    char src[] = "BLACK";
    char dst[] = "Hi";
    
    printf("strlcpy : %zu \n", strlcpy(dst, src, 3));
    printf("dst : %s \n", dst);
    return 0;
}
```
```
strlcpy : 5
dst : BL    (dstsize가 0이면 Hi, 1이면 아무것도 안뜸(널문자), 2면 B)
```
dst 문자열의 길이는 3(널문자 포함)이기 때문에! dstsize를 4이상을 넣게 되면 overflow오류가 뜬다.  
<br/>

- src 길이가 dst보다 짧을 때
```
#include <stdio.h>
#include <string.h>

int main(void)
{
    char dst[] = "BLACK";
    char src[] = "Hi";
    
    printf("strlcpy : %zu \n", strlcpy(dst, src, 0));
    printf("dst : %s \n", dst);
    return 0;
}
```
```
strlcpy : 2
dst : BLACK

dst :    (dstsize가 1일 때)
dst : H  (dstsize = 2)
dst : Hi (dstsize = 3, 4, 5, 6)
```
dstsize = 7 부터 overflow  
<br/><br/>

### - strncpy
- src길이가 dst보다 길 때
```
#include <stdio.h>
#include <string.h>

int main(void)
{
    char src[] = "BLACK";
    char dst[] = "Hi";
    
    printf("strncpy : %s \n", strlcpy(dst, src, 0));
    return 0;
}
```
```
strncpy : Hi    (len이 1이면 Bi, 2이면 BL, 3이면 BLABLACK, 4부터 overflow)
```
strlcpy와 차이점은 복사를 한 후 NULL-terminated 시키느냐 차이가 있는 것 같다.  
<br/><br/>

- src길이가 dst보다 짧을 때
```
#include <stdio.h>
#include <string.h>

int main(void)
{
    char dst[] = "BLACK";
    char src[] = "Hi";
    
    printf("strncpy : %s \n", strlcpy(dst, src, 5));
    return 0;
}
```
```
strncpy : Hi    (2일 때 HiACK, 3,4,6일 때도 Hi, 7이상 overflow)
```
<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 3시간 
* 학습에 참고한 사이트 :
    * [구현](https://wonillism.tistory.com/158)
    * [strlcpy1](https://velog.io/@mjung/function-strlcpy-strlcat) 
    * [strlcpy2](https://whatdocumentary.tistory.com/44)  

<br/>

## 8. 학습 내용에 대한 개인적인 총평
strlcpy를 한 번 다뤄본 적이 있지만, 자세하게 공부했던 적은 없었다. 이번에 함수의 원형, 매개변수 등을 다시 정리하며, strcpy에 비해 안전하다는 장점까지 알게되어 strlcpy 함수를 더 잘 이해할 수 있었다. 이렇게 보고서로 정리하는 게 너무 의미있는게, 대충 알고 넘겼던 부분들도 다시 한 번 자세히 공부할 수 있도록 동기 부여를 주는 것 같다. 다만 대면 학습이 아니기 때문에 동료 학습이 아무래도 잘 이뤄지지 않아 정보를 얻는데 시간이 많이 걸린다는 점이 아쉽다.  
<br/>

## 9. 다음 학습 계획
- strlcpy 함수를 구현해본다.  
