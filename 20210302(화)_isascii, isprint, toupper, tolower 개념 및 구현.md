## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [isascii](#isascii)
        - [함수의 기능](#함수의-기능)
        - [함수의 원형](#함수의-원형)
        - [헤더파일](#헤더파일)
        - [매개변수](#매개변수)
        - [반환값](#반환값)
        - [예제](#예제)
        - [isascii 구현](#isascii-구현)
            - [최종 코드](#최종-코드) 
    - [isprint](#isprint)
        - [함수의 기능](#함수의-기능-1)
        - [함수의 원형](#함수의-원형-1)
        - [헤더파일](#헤더파일-1)
        - [매개변수](#매개변수-1)
        - [반환값](#반환값-1)
        - [예제](#예제-1)
        - [isprint 구현](#isprint-구현)
            - [최종 코드](#최종-코드-1) 
    - [toupper](#toupper)
        - [함수의 기능](#함수의-기능-2)
        - [함수의 원형](#함수의-원형-2)
        - [헤더파일](#헤더파일-2)
        - [매개변수](#매개변수-2)
        - [반환값](#반환값-2)
        - [예제](#예제-2)
        - [toupper 구현](#toupper-구현)
            - [최종 코드](#최종-코드-2) 
    - [tolower](#tolower)
        - [함수의 기능](#함수의-기능-3)
        - [함수의 원형](#함수의-원형-3)
        - [헤더파일](#헤더파일-3)
        - [매개변수](#매개변수-3)
        - [반환값](#반환값-3)
        - [예제](#예제-3)
        - [tolower 구현](#tolower-구현)
            - [최종 코드](#최종-코드-3)
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-03-02(화)<br/><br/>
## 2. 학습 시간
*  (자가)<br/><br/>
## 3. 학습 범위 및 주제
* isascii, isprint 함수 개념 정리 및 구현<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* <br/><br/>
## 6. 상세 학습 내용
## isascii
### 함수의 기능
- 아스키 코드 테스트 함수
- 아스키 코드인지를 테스트 하기위해 **8진수로 0부터 0177사이의 문자인지 확인한다. (10진법으로는 0 ~ 127)**  
- man isascii
```
The isascii() function tests for an ASCII character, which is any character between 0 and octal 0177 inclusive.
```
<br/>

### 함수의 원형
```
int    isascii(int c);
```
<br/>

### 헤더파일
```
<ctype.h>
```
<br/>

### 매개변수
- c : 테스트할 문자

<br/>

### 반환값
- 0 : 문자 테스트 후 문자가 아닌 경우
- 1 : 아스키 문자가 맞는 경우 

<br/>

### 예제
```
printf("result : %d \n", isascii('a'));    // 1
printf("result : %d \n", isascii('뷁'));   // 0
```
<br/>

### iascii 구현
### 최종 코드
```c
#include "libft.h"

int ft_isascii(int c)
{
    if (c >= 0 && c <= 127)
        return (1);
    return (0);
}
```
> 참고로 16진수 숫자를 표현할때는 `0x`, 8진수 숫자 표현할 때는 `0`.  
> ex) 8진수 **0**177

<br/><br/>

---
<br/><br/>

## isprint
### 함수의 기능
- **출력할 수 있는 문자의 ASCII 코드 값인지 판별(공백 포함)**
- 8진수 아스키 코드 40 ~ 47, 50 ~ 57, 60 ~ 67, 70 ~ 77, 100 ~ 107, 110 ~ 117, 120 ~ 127, 130 ~ 137, 140 ~ 147, 150 ~ 157, 160 ~ 167, 170 ~ 176  
(**10진수로 32 ~ 126**)
- man isprint
```
The isprint() function tests for any printing character, including space(' ').  
The value of the argument must be representable as an unsigned char or the value of EOF.
```
<br/>

### 함수의 원형
```
int    isprint(int c);
```
<br/>

### 헤더파일
```
<ctype.h>
```
<br/>

### 매개변수
- c

<br/>

### 반환값
- c가 출력할 수 있는 문자이면 1, 아니면 0을 반환

<br/>

### 예제
```c
#include <ctype.h>
#include <stdio.h>
 
int main(void)
{
    int i = 0;
    int count = 0;
    printf("=== 출력 가능한 ASCII 코드 ===\n");
    for (i = 0; i < 128; i++)
    {
        if (isprint(i))
        {
            printf("%#x:%c    ", i, i);
            count++;
            if (count % 5 == 0)
            {
                printf("\n");
            }
        }
    }
    printf("\n");
    return 0;
}
```
```
=== 출력 가능한 ASCII 코드 ===
0x20:     0x21:!    0x22:"    0x23:#    0x24:$    
0x25:%    0x26:&    0x27:'    0x28:(    0x29:)    
0x2a:*    0x2b:+    0x2c:,    0x2d:-    0x2e:.    
0x2f:/    0x30:0    0x31:1    0x32:2    0x33:3    
0x34:4    0x35:5    0x36:6    0x37:7    0x38:8    
0x39:9    0x3a::    0x3b:;    0x3c:<    0x3d:=    
0x3e:>    0x3f:?    0x40:@    0x41:A    0x42:B    
0x43:C    0x44:D    0x45:E    0x46:F    0x47:G    
0x48:H    0x49:I    0x4a:J    0x4b:K    0x4c:L    
0x4d:M    0x4e:N    0x4f:O    0x50:P    0x51:Q    
0x52:R    0x53:S    0x54:T    0x55:U    0x56:V    
0x57:W    0x58:X    0x59:Y    0x5a:Z    0x5b:[    
0x5c:\    0x5d:]    0x5e:^    0x5f:_    0x60:`    
0x61:a    0x62:b    0x63:c    0x64:d    0x65:e    
0x66:f    0x67:g    0x68:h    0x69:i    0x6a:j    
0x6b:k    0x6c:l    0x6d:m    0x6e:n    0x6f:o    
0x70:p    0x71:q    0x72:r    0x73:s    0x74:t    
0x75:u    0x76:v    0x77:w    0x78:x    0x79:y    
0x7a:z    0x7b:{    0x7c:|    0x7d:}    0x7e:~  
```
<br/>

### isprint 구현
### 최종 코드
```c
#include "libft.h"

int    ft_isprint(int c)
{
    if (c >= ' ' && c <= '~')
        return (1);
    return (0);
}
```
<br/><br/>

---
<br/><br/>

## toupper
### 함수의 기능
- man toupper
```
The toupper() function converts a lower-case letter to the corresponding upper-case letter.
The argument must be representable as un unsigned char or the value of EOF.
```
<br/>

### 함수의 원형
```
int    toupper(int c);
```
<br/>

### 헤더파일
```
<ctype.h>
```
<br/>

### 매개변수
- c : 대문자인지 판별할 문자. 

<br/>

### 반환값
- 만일 소문자를 인자로 받으면 대문자를 리턴하고, 그렇지 않으면 그냥 리턴한다.

<br/>

### 예제
```
printf("[B] : %c \n", toupper('B'));
printf("[@] : %c \n", toupper('@'));
printf("[a] : %c \n", toupper('a'));
```
```
[B] : B
[@] : @
[a] : A
```
<br/>

### toupper 구현
### 최종 코드
```
#include "libft.h"

int    ft_toupper(int c)
{
    if (c >= 'a' && c <= 'z')
        return (c - 32);
    return (c);
}
```
<br/><br/>

---
<br/><br/>

## tolower
### 함수의 기능
- 대문자를 소문자로 변환한다.
<br/>

### 함수의 원형
```
int    tolower(int c);
```
<br/>

### 헤더파일
```
<ctype.h>
```
<br/>

### 매개변수
- c

<br/>

### 반환값
- 인자가 대문자면 소문자를 리턴하고, 그렇지 않으면 그냥 c 를 리턴한다.

<br/>

### 예제
```
printf("[B] : %c \n", tolower('B'));
printf("[@] : %c \n", tolower('@'));
printf("[a] : %c \n", tolower('a'));
```
```
[B] : b
[@] : @
[a] : a
```
<br/>

### tolower 구현
### 최종 코드
```
#include "libft.h"

int    ft_tolower(int c)
{
    if (c >= 'A' && c <= 'Z')
        return (c + 32);
    return (c);
}
```
<br/><br/>

---
<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 3 시간
* 학습에 참고한 사이트 :
    * [isascii](https://velog.io/@mjung/function-isascii)
    * [isprint](https://ehpub.co.kr/isprint-%ED%95%A8%EC%88%98/)
<br/>

## 8. 학습 내용에 대한 개인적인 총평


## 9. 다음 학습 계획
- 
