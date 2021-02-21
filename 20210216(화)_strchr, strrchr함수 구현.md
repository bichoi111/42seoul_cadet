## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [strchr](#strchr)
        - [최종 코드](#최종-코드) 
    - [strrchr](#strrchr)
        - [최종 코드](#최종-코드) 
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-02-16(화)<br/><br/>
## 2. 학습 시간
* 13:00 ~ 17:00 (자가)<br/><br/>
## 3. 학습 범위 및 주제
* strchr 함수 구현
* strrchr 함수 구현<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* strchr, strrchr 함수를 구현하고, 테스트해본다.<br/><br/>
## 6. 상세 학습 내용
## strchr
```
char    *strchr(const char *s, int c);
```
### 두번째 인자의 자료형
이 함수는 문자열에서 문자를 찾는 함수인데, 왜 두번째 매개변수가 char형이 아니라 int형일까?  

> 두번째 인자에 `int`형태로 형변환 되어서 전달되지만 **함수 내부적으로는 다시 `char`형태로 처리된다.**  
> [참고 사이트](https://modoocode.com/93)  

<br/>

### 시도 1
```
// ft_strchr.c 

#include "libft.h"

char    *ft_strchr(const char *s, int c)
{
    while (*s)
    {
        if (*s == (char)c)
            return (s);
        else
            s++;
    }
    if (*s == (char)c)    // 널문자 일치하는지 확인하는 작업
        return (s);
    return (0);
}
```
```
// libft.h

char    *ft_strchr(const char *s, int c);    // 이 부분 추가
```
```
// main.c

int main(void)
{
    char *s = "hi sally!";
    //printf("strchr : %p \n", strchr(s, 'h'));    // 0x10a5e9f98
    printf("ft_strchr : %p \n", ft_strchr(s, 'h'));
    return (0);
}
```
```
$ gcc main.c ft_strchr.c

ft_strchr.c: warning: returning 'const char *' from a function with result type 'char *' discards qualifiers
        [-Wincompatible-pointer-types-discards-qualifiers]
        retunn (s);
```
**결과 유형이 'char \*'인 함수에서 'const char \*'를 반환하면 한정자가 취소된다**는 오류가 떴다.  

> ft_strchr.c 파일을 보면 ft_strchr 함수의 반환형은 `char *`인데 내가 **return**한 값은 s로 `const char *`형이다.  

<br/>

### 시도 2
```
// ft_strchr.c 

#include "libft.h"

char    *ft_strchr(const char *s, int c)
{
    while (*s)
    {
        if (*s == (char)c)
            return ((char *)s);    // 이 부분 수정. 형변환.
        else
            s++;
    }
    if (*s == (char)c)
        return ((char *)s);    // 이 부분 수정
    return (0);
}
```
```
$ gcc main.c ft_strchr.c

strchr : 0x10bc57f84
ft_strchr : 0x10bc57f84
```
<br/>

### 잘못 생각했던 점
```
int main(void)
{
    char *s = "hi sally!";
    printf("strchr : %s \n", strchr(s, 'a'));          // (1)
    printf("ft_strchr : %s \n", ft_strchr(s, 'h'));    // (2)
    return (0);
}
```
내가 위에서 구현한 `ft_strchr`함수는 **포인터 s자체의 주소값을 바꿔가며** 값에 접근을 해서,  
이 함수를 실행 후에 s가 가리키는 주소값이 바뀌어있다고 생각했다.  
실제 `strchr` 함수도 이런 방식이라면 (1)의 코드를 실행 후에 s가 가리키는 주소값은 'a' 자리로 변해있을 것이다.  
그러므로 (2)코드를 이어서 실행하면 **'a'부터 탐색하기 때문에 'h'를 찾지 못할 것**이라고 생각했다. 그러나 실행 결과,  
```
strchr : ally!
ft_strchr : hi sally!
```
<br/>

main함수를 아래와 같이 수정해도,  
```
printf("ft_strchr : %s \n", strchr(s, 'a'));    // (1)
printf("strchr : %s \n", ft_strchr(s, 'h'));    // (2)
```
```
ft_strchr : ally!
strchr : hi sally!
```
이와 같이 출력되었다. 내가 예상했던 결과는 (2) 코드 실행결과 `NULL`이 반환될 것이라고 생각했었다.  

> 이게 혹시 `ft_strchr` 함수 실행을 printf문 안에서 해서 s가 가리키는 주소값이 안바뀐건가?  

<br/><br/>

- main 수정
```
int main()
{
    char *s = "hi sally!";
    printf("initial address : %p  %s \n", s, s);
    s = ft_strchr(s, 'a');    // 이렇게 s값에 ft_strchr함수 반환값을 직접 대입하는 과정 추가
    
    printf("after ft_strchr : %p %s \n", s, s);
    printf("strchr : %p  %s \n", strchr(s, 'h'), strchr(s, 'h'));
    return 0;
}
```
```
start address : 0x100d2cf64  hi sally!
ft_strchr : 0x100d2cf68  ally!
strchr : 0x0  (null)
```
이렇게 하니 내가 예상했던 결과가 나왔다.  
`print`문을 통해 내가 s값을 넘겨준 방식이 `call-by-vlaue`였나 보다.  

실제 `strchr`도 내가 만든 `ft_strchr`함수와 같은 방법으로 동작하는지 확인하기 위해 아래 코드를 실행하였다.  
```
int main()
{
    char *s = "hi sally!";
    printf("initial address : %p  %s \n", s, s);
    s = trchr(s, 'a');    // 이렇게 s값에 ft_strchr함수 반환값을 직접 대입하는 과정 추가
    
    printf("after strchr : %p %s \n", s, s);
    printf(" ft_strchr : %p  %s \n", ft_strchr(s, 'h'), ft_strchr(s, 'h'));
    return 0;
}
```
```
start address : 0x1071c5f64  hi sally!
strchr : 0x1071c5f68  ally!
ft_strchr : 0x0  (null)
```
같은 결과가 나왔다.  
<br/>

### 최종 코드
```
// ft_strchr.c 

#include "libft.h"

char    *ft_strchr(const char *s, int c)
{
    while (*s)
    {
        if (*s == (char)c)
            return ((char *)s);
        else
            s++;
    }
    if (*s == (char)c)
        return ((char *)s); 
    return (0);
}
```
<br/><br/>

---
<br/><br/>

## strrchr
### 시도 1
```
// ft_strrchr.c

#include "libft.h"

char    *ft_strrchr(const char *s, int c)
{
    int len;
    
    len = 0;
    while (*s)    // 문자열의 끝으로 포인터를 이동시킴과 동시에 길이를 파악.
    {
        len++;
        s++;
    }
    while (len >= 0)   // 길이를 파악해야 문자열의 처음을 알 수 있음.
    {
        if (*s == (char)c)
            return ((char *)s);
        else
        {
            s--;
            len--;
        }
    }
    return (0);
}
```
```
// main.c

int main(void)
{
    char *s = "ha sally!";
    printf("strrchr : %p  %s \n", strrchr(s, 'a'), strrchr(s, 'a'));
    printf("ft_strrchr : %p  %s \n", ft_strrchr(s, 'a'), ft_strrchr(s, 'a'));
    printf("\n");
    printf("strrchr : %p  %s \n", strrchr(s, 'a'), strrchr(s, 'a'));
    printf("ft_strrchr : %p  %s \n", ft_strrchr(s, 'h'), ft_strrchr(s, 'h'));
    
    return (0);
}
```
```
strrchr : 0x10b77bf7c  ally!
ft_strrchr : 0x10b77bf7c  ally!

strrchr : 0x10b77bf7c  ally!
ft_strrchr : 0x10b77bf78  ha sally!
```
예상대로 출력되었다.  

`strrchr`실행 결과, main함수의 s가 가리키는 주소값을 직접적으로 변화시키면,  
```
int main(void)
{
    char *s = "ha sally!";
    s = strrchr(s, 'a');  
    printf("strrchr : %p  %s \n", s, s);
    printf("ft_strrchr : %p  %s \n", ft_strrchr(s, 'h'), ft_strrchr(s, 'h'));
    return (0);
}
```
```
strrchr : 0x10ad49f7c  ally!
ft_strrchr : 0x0  (null)
```
<br/>

### 최종 코드
```
// ft_strrchr.c

#include "libft.h"

char    *ft_strrchr(const char *s, int c)
{
    int len;
    
    len = 0;
    while (*s)
    {
        len++;
        s++;
    }
    while (len >= 0)
    {
        if (*s == (char)c)
            return ((char *)s);
        else
        {
            s--;
            len--;
        }
    }
    return (0);
}
```
<br/><br/>

---
<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 4시간
* 학습에 참고한 사이트 :
    * 자가 학습

<br/>

## 8. 학습 내용에 대한 개인적인 총평
지난 시간에 strchr, strrchr 함수 개념 정리한 것을 토대로 ft_strchr, ft_strrchr 함수를 구현해보았다. 대략적으로 기능 구현을 하는 데에는 문제없었지만,
"결과 유형이 'char \*'인 함수에서 'const char \*'를 반환하면 한정자가 취소된다"는 오류가 있어서, return 하는 부분에서 `char *`로 형 변환을 해주었다. 다른 방법으로는 새로운 char형 포인터 변수를 선언을 해서 그 변수를 반환해도 될 것 같다. 솔직히 함수 구현은 인터넷에 검색하면 다 나오긴 하지만 내가 직접해봐야 기억에도 남기 때문에 오래 걸려도 직접 짜보고 있다. strchr, strrchr 함수의 반환 값중 일치하는 문자가 없을 때 NULL을 반환하는데, 이 함수들의 반환형을 보면 `char *`이다. 즉 그냥 NULL이 아니라 NULL 포인터를 반환하는 것이다. 이 널 포인터의 의미를 다음 시간에 자세하게 공부해봐야겠다.       
<br/>

## 9. 다음 학습 계획
- null 포인터와 void 포인터에 대해 학습한다.
