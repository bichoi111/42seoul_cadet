## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [bzero](#bzero)
        - [함수의 기능](#함수의-기능)
        - [함수의 원형](#함수의-원형)
        - [헤더파일](#헤더파일)
        - [매개변수](#매개변수)
        - [반환값](#반환값)
        - [예제](#예제)
        - [bzero 구현](#bzero-구현)
            - [최종 코드](#최종-코드) 
    - [isalpha](#isalpha)
        - [함수의 기능](#함수의-기능-1)
        - [함수의 원형](#함수의-원형-1)
        - [헤더파일](#헤더파일-1)
        - [매개변수](#매개변수-1)
        - [반환값](#반환값-1)
        - [예제](#예제-1)
        - [isalpha 구현](#isalpha-구현)
            - [최종 코드](#최종-코드-1) 
    - [isdigit](#isdigit)
        - [함수의 기능](#함수의-기능-2)
        - [함수의 원형](#함수의-원형-2)
        - [헤더파일](#헤더파일-2)
        - [매개변수](#매개변수-2)
        - [반환값](#반환값-2)
        - [예제](#예제-2)
        - [isdigit 구현](#isdigit-구현)
            - [최종 코드](#최종-코드-2) 
    - [isalnum](#)
        - [함수의 기능](#함수의-기능-3)
        - [함수의 원형](#함수의-원형-3)
        - [헤더파일](#헤더파일-3)
        - [매개변수](#매개변수-3)
        - [반환값](#반환값-3)
        - [예제](#예제-3)
        - [isalnum 구현](#isalnum-구현)
            - [최종 코드](#최종-코드-3)
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-02-28(일)<br/><br/>
## 2. 학습 시간
* 14:00 ~ 16:00 (자가)
* 18:00 ~ 19:00 (자가)<br/><br/>
## 3. 학습 범위 및 주제
* bzero, isalpha, isdigit, isalnum 함수 개념 정리
* ft_bzero, ft_isalpha, ft_isdigit, ft_isalnum함수 구현<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* bzero, isalpha, isdigit, isalnum 함수의 개념을 정리하고 구현해본다.<br/><br/>
## 6. 상세 학습 내용
## bzero
### 함수의 기능
- 스트링 `s`의 처음 `n` 바이트를 `0`으로 채운다.  
- 메모리를 초기화하기 위한 목적으로 주로 사용되지만, 다른 값으로 채우는 것은 불가능하기 때문에 `memset` 함수가 더 유용하다.  
- `man bzero` description
```
The bzero() function writes n zeroed bytes to the string s.
if n is zero, bzero() does nothing.
```
<br/>

### 함수의 원형
```
void bzero(void *s, size_t n);
```
<br/>

### 헤더파일
```
<string.h>
<strings.h>
```
<br/>

### 매개변수
- `s` : 0으로 초기화하기 시작할 주소값
- `n` : 0으로 초기화 할 바이트 수

<br/>

### 반환값
- 어떤 값도 반환하지 않는다.

<br/>

### 예제
```
#include <string.h>

struct mydata
{
    int a;
    char b[255];
};

int main()
{
    char buf[255];
    struct mydata data;
    
    bzero(buf, 255);
    bsero((void *)&data, sizeof(data));    // 구조체 초기화도 가능!
}
```
<br/>

### bzero 구현
#### 원래 bzero 함수 기능 확인
```
#include <string.h>
#include <stdio.h>

int main(void)
{
    char a[10];
    int b = 1;
    
    bzero(&b, 1);    // 변수 b를 0으로. n값에 4초과 값이 들어가면 overflow warning. int 형 변수가 4바이트라 그런 듯.
    printf("b = %d \n", b);
    
    printf("before bzero : ");
    for (int i = 0; i < 10; ++i)
        printf("%d ", a[i]);
    printf("\n");
    
    bzero(a, 10);
    printf("after bzero  : ");
    for (int i = 0; i < 10; ++i)
        printf("%d ", a[i]);
    return (0);
}
```
```
b = 0
before bzero : 0 0 64 -100 1 -25 -2 127 0 0
after bzero  : 0 0 0 0 0 0 0 0 0 0 
```
<br/>

#### 최종 코드
```
// ft_bzero.c

#include "libft.h"

void ft_bzero(void *s, size_t n)
{
    char *ptr;
    
    ptr = (char *)s;
    while (n-- > 0)
        *(ptr++) = 0;
    return;
}
```
<br/><br/>

---
<br/><br/>

## isalpha
### 함수의 기능
- **문자가 알파벳인가**를 확인해주는 함수
- man isalpha
```
The isalpha() function tests for any character for which isupper(3) or islower(3) is true.
The value of the argument must be representable as an unsigned char or the value of EOF.
```
<br/>

### 함수의 원형
```
int    isalpha(int c);
```
<br/>

### 헤더파일
```
<ctype.h>
```
<br/>

### 매개변수
- `int c`
- C언어에서 아스키 코드에 해당하는 문자들은 숫자로 표현이 되고, 문자를 넣으면 자동으로 아스키 코드에 있는 숫자로 들어가기 때문에  
`int` 타입이긴 하지만 'a', 'A', '1' 등을 집어 넣어도 된다.  
- 즉, 'a'와 같이 `char` 타입으로 집어 넣어도 자동으로 `int`타입으로 형변환 되어서 들어간다.  
('a'는 아스키 코드 표를 참고하면 자동으로 숫자 97로 형변환되어 들어감.)  

<br/>

### 반환값
- 알파벳이면 `0이 아닌 값`을 리턴하고, 그렇지 않으면 '0'을 리턴한다.  
- 더 정확하게 이야기하면 **알파벳 대문자(A-Z), 알파벳 소문자(a-z)는 1**를 반환, **알파벳이 아닌 것은 0**을 반환
- **알파벳 대문자(아스키 코드 65번-90번), 알파벳 소문자(97번-122번)는 1**를 반환, **그 이외의 값은 0**을 반환

<br/>

### 예제
#### - 문자열 내부 하나하나 isalpha 함수로 확인
```
#include<ctype.h>
#include<stdio.h>
 
int main(void)
{
    char str[] = "BlockDMask1234";
    
    for (int i = 0; i < (int)strlen(str); ++i)
    {
        printf("isalpha(\'%c\') : %d\n", str[i], isalpha(str[i]));
    }
```
```
isalpha('B') : 1
isalpha('l') : 1
isalpha('o') : 1
isalpha('c') : 1
isalpha('k') : 1
isalpha('D') : 1
...
isalpha('1') : 0
isalpha('2') : 0
isalpha('3') : 0
isalpha('4') : 0
```
<br/>

#### - 아스키 코드표에 있는 숫자로 테스트
```
#include<ctype.h>
#include<stdio.h>
 
int main(void)
{
    // 대문자 A
    printf("[ %c ] isalpha(%d) : %d\n", 65, 65, isalpha(65));
 
    // 대문자 B
    printf("[ %c ] isalpha(%d) : %d\n", 66, 66, isalpha(66));
 
    // 소문자 d
    printf("[ %c ] isalpha(%d) : %d\n", 100, 100, isalpha(100));
 
    // 소문자 z
    printf("[ %c ] isalpha(%d) : %d\n", 122, 122, isalpha(122));
 
    // 숫자 0
    printf("[ %c ] isalpha(%d) : %d\n", 48, 48, isalpha(48));
 
    // 숫자 9
    printf("[ %c ] isalpha(%d) : %d\n", 57, 57, isalpha(57));
 
}
```
```
[ A ] isalpha(65) : 1
[ B ] isalpha(65) : 1
[ d ] isalpha(65) : 1
[ z ] isalpha(65) : 1
[ 0 ] isalpha(65) : 0
[ 9 ] isalpha(65) : 0
```
<br/>

### isalpha 구현
#### 최종 코드
```
#include "libft.h"

int ft_isalpha(int c)
{
    if ((c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z'))
        return (1);
    return (0);
}
```
<br/><br/>

---
<br/><br/>

## isdigit
### 함수의 기능
- 문자인지 숫자인지 판별해주는 함수
- 인자가 10진수 숫자로 변경이 가능하면 0이 아닌 숫자(true), 아니면 0(false)를 반환하는 함수
- man isdigit
```
The isdigit() function tests for a decimal digit character.
Regardless of locale, this includes the following charaters only:

'0' '1' '2' '3' '4' '5' '6' '7' '8' '9'
```
<br/>

### 함수의 원형
```
int    isdigit(int c);
```
<br/>

### 헤더파일
```
<ctype.h>
```
<br/>

### 반환값
- 숫자이면 0이 아닌 값(실행해보니 1), 문자이면('\0'도 문자) 0이 반환된다.  

<br/>

### 매개변수
- int c
- 매개변수 타입이 char 타입이 아닌 int 타입으로 받는데, 이는 char 타입이 아스키 코드 번호로 들어갈 수 있기 때문이다.  
- 아스키 코드표에서 **48~57번에 매칭되는 문자 '0' ~ '9'가 들어오면 True를 반환하는 형태이다.**    

<br/>

### 예제
```
#include <stdio.h>
#include <ctype.h>

int main()
{
    printf("%d \n", isdigit('B'));
    printf("%d \n", isdigit('b'));
    printf("%d \n", isdigit('0'));
    printf("%d \n", isdigit('\0'));
    printf("%d \n", isdigit('@'));
    printf("%d \n", isdigit(0));    // 1 도 결과 동일 
    printf("%d \n", isdigit(48));   // '0'의 아스키 코드
    return 0;
}
```
```
0
0
1
0
0
0
1
```
<br/>

### isdigit 구현
#### 최종 코드
```
#include "libft.h"

int ft_isdigit(int c)
{
    if (c >= '0' && c <= '9')
        return (1);
    return (0);
}
```
<br/><br/>

---
<br/><br/>

## isalnum
### 함수의 기능
- 숫자 혹은 알파벳(대, 소문자)인지 판별해주는 함수
- man isalnum
```
The isalnum() function tests for any character for which isalpha(3) or isdigit(3) is true. 
The value of the argument must be representable as an unsigned char or the value of EOF.
```
<br/>

### 함수의 원형
```
int    isalnum(int c);
```
<br/>

### 헤더파일
```
<ctype.h>
```
<br/>

### 매개변수
- int c

<br/>

### 반환값
- 숫자 또는 알파벳이면 0이 아닌 값(실행해보니 1), 그렇지 않으면 0이 반환된다.  

<br/>

### isalnum 구현
#### 최종 코드
```
#include "libft.h"

int ft_isalnum(int c)
{
    if ((c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z'))
        return (1);
    if (c >= '0' && c <= '9')
        return (1);
    return (0);
}
```
<br/>

#### test
```
#include <stdio.h>
#include <ctype.h>

int main()
{
    printf("%d \n", ft_isalnum('B'));
    printf("%d \n", ft_isalnum('b'));
    printf("%d \n", ft_isalnum('0'));
    printf("%d \n", ft_isalnum('\0'));
    printf("%d \n", ft_isalnum('@'));
    printf("%d \n", ft_isalnum(0));    // 1 도 결과 동일 
    printf("%d \n", ft_isalnum(48));   // '0'의 아스키 코드
    return 0;
}
```
```
1
1
1
0
0
0
1
```
<br/><br/>

---
<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 3 시간
* 학습에 참고한 사이트 :
    * [bzero](https://www.joinc.co.kr/w/man/3/bzero)
    * [isalpha](https://blockdmask.tistory.com/448)

<br/>

## 8. 학습 내용에 대한 개인적인 총평
bzero, isalpha, isdigit, isalnum 함수에 대한 기본 개념만 정리해볼 생각이었는데, 함수 기능이 간단하고 쉬워서 구현까지 해보았다. bzero 함수를 작성하면서 memset과의 차이점에 대해서도 생각해 봤는데, bzero는 오직 '0'으로만 초기화할 수 있지만 memset 함수 같은 경우에는 0 이외에도 내가 원하는 문자로 초기화할 수 있다. 찾아보니 bzero 함수보다는 memset 함수를 더 권장한다고도 했다. bzero 함수의 기능을 memset 함수가 충분히 해낼 수 있기 때문이다. 다양한 함수들을 접하고 기능을 익히면서, 더 효율적이고 (라피신때는 배열 요소 하나 하나를 직접 초기화했다면) 직관적인 코드를 짤 수 있을 것 같다. 블랙홀이 어느덧 일주일밖에 남지 않았는데, 마지막까지 최선을 다해볼 것이다.  
<br/>

## 9. 다음 학습 계획
- isascii, isprint 함수 개념 정리 및 구현
