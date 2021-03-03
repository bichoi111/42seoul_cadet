## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [strstr 구현](#strstr-구현)
        - [최종 코드](#최종-코드)
    - [strnstr 구현](#strnstr-구현)
        - [최종 코드](#최종-코드) 
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-02-22(월)<br/><br/>
## 2. 학습 시간
* 13:00 ~ 14:00 (자가)
* 16:00 ~ 20:00 (자가)<br/><br/>
## 3. 학습 범위 및 주제
* strstr, strnstr 구현 및 테스트<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* ft_strstr.c 파일과 ft_strnstr.c 파일에 함수의 기능을 구현해보고 그 기능을 점검한다.<br/><br/>
## 6. 상세 학습 내용
## strstr 구현
### 포인터 궁금한 점
- ptr 변수에 s1이 가리키는 주소를 저장한 이후, s1이 가리키는 주소를 바꾸면 ptr이 가리키는 주소도 자동으로 바뀌나?
```
int main()
{
    char *s1 = "go go yogurt";
    char *ptr = s1;
    
    printf("ptr : %s \n", ptr);
    s1++;    
    printf("s1 : %s \n", s1);
    printf("ptr : %s \n", ptr);
    return 0;
}
```
```
ptr : go go yogurt
s1 : o go yogurt
ptr : go go yogurt
```
ptr이 가리키는 주소는 그대로인 것을 알 수 있다.  
<br/><br/>

---
<br/><br/>

### 시도 1
```
#include "libft.h"

char    *ft_strstr(const char *haystack, const char *needle)
{
    const char *h_ret;
    const char *n_ret;
    
    h_ret = haystack;
    n_ret = needle;
    while (*h_ret && *n_ret)
    {
        if (*h_ret == *n_ret)
        {
            haystack = h_ret;
            while (*h_ret == *n_ret)
            {
                h_ret++;
                n_ret++;
            }
            if (*n_ret == '\0')
                return ((char *)haystack);
            else if (*h_ret == '\0')
                return (0);
            else
            {
                n_ret = needle;
                continue;
            }
        }
        h_ret++;
    }
    return ((char *)haystack);
}
```
```
// main.c

int main(void)
{
    char *s1 = "go go yogurt";
    char *s2 = "gu";
    
    printf("ft_strstr return : %s \n", ft_strstr(s1, s2));
    printf("strstr return    : %s \n", strstr(s1, s2));
    return (0);
}
```
```
실행 결과

ft_strstr return : gurt
strstr return    : gurt
```
결과가 일치한다.  
그런데 s1, s2를 바꿔서 `ft_strstr(s2, s1), strstr(s2, s1)`으로 실행했더니
```
ft_strstr return : gu
strstr return    : (null)
```
<br/>

haystack에 "\0"을 넣고 실행해 보았더니
```
ft_strstr return :
strstr return    : (null)
```
<br/><br/>

### 시도 2
```
#include "libft.h"

char    *ft_strstr(const char *haystack, const char *needle)
{
    const char *h_ret;
    const char *n_ret;
    
    h_ret = haystack;
    n_ret = needle;
    while (*h_ret && *n_ret)
    {
        if (*h_ret == *n_ret)
        {
            haystack = h_ret;
            while (*h_ret == *n_ret)
            {
                h_ret++;
                n_ret++;
            }
            if (*n_ret == '\0')
                return ((char *)haystack);
            else if (*h_ret == '\0')
                return (0);
            else
            {
                n_ret = needle;
                continue;
            }
        }
        h_ret++;
    }
    if (*h_ret == '\0')    // 이 부분만 추가
        return (0);        //
    return ((char *)haystack);
}
```
위와 같이 수정하였더니 시도 1에서 다르게 나오던 결과가 일치했다.  
그런데 코드가 좀 비효율적으로 짜여진 것 같아서 조금 더 다듬어 보았다.  
```
#include "libft.h"

char    *ft_strstr(const char *haystack, const char *needle)
{
    const char *h_ret;
    const char *n_ret;
    
    h_ret = haystack;
    n_ret = needle;
    while (*h_ret && *n_ret)
    {
        if (*h_ret == *n_ret)
        {
            haystack = h_ret;
            while (*h_ret == *n_ret)
            {
                h_ret++;
                n_ret++;
            }
            if (*h_ret != '\0' && *n_ret != '\0')    // 이 부분 수정
            {
                n_ret = needle;
                continue;
            }
        }
        h_ret++;
    }
    if (*h_ret == '\0')    
        return (0);        
    return ((char *)haystack);
}
```
리턴문이 중복되는 부분들을 수정하였고, 이 함수의 설명 중에 `haystack`을 반환한다고 되어있어서 꼭 `return (haystack)`을 써야한다는 생각에  
중복 되는 부분들이 있었는데 `haystack` 값을 받은 변수를 리턴해도 된다고 한다.  
<br/><br/>

### 틀린 부분
1) haystack, needle 문자열 모두 비어있을 때("\0") 출력값이
```
ft_strstr return : (null)
strstr return    : 
```
이렇게 결과가 다르게 나온다. 즉, strstr함수는 일치하는 널문자를 (`\0`) 출력한 것.  
(널문자는 출력하면 위의 strstr 결과같이 안 보임.)
<br/>

2) 그리고 haystack에 "go gu", needle에 "gu"를 넣어줬을 때 아래와 같은 오류가 발생했다.  
```
"bus error"
```
기대한 출력 값은 `gu`이다.  
중간에 나오는 문자열은 출력하는데 "a", "a"나 "go **gu**", "gu" 같이 마지막 부분에 일치하는 문자열이 있을 경우에는,
널문자 처리가 잘못되었는지 오류가 났다. 
<br/><br/>

### 시도 3
```c
#include "libft.h"

char    *ft_strstr(const char *haystack, const char *needle)
{
    const char *h_ret;
    const char *n_ret;
    
    h_ret = haystack;
    n_ret = needle;
    while (*h_ret && *n_ret)
    {
        if (*h_ret == *n_ret)
        {
            haystack = h_ret;
            while (*h_ret == *n_ret && *h_ret && *n_ret)    // 이 부분에서 널문자 처리를 안해줘서 "\0" == "\0" 이 경우에도 while문이 돌아가고 있었다. (틀린 부분2) 
            {
                h_ret++;
                n_ret++;
            }
            if (*h_ret == '\0' || *n_ret == '\0')    // *h_ret가 널인경우 h_ret++ 연산을 해주면 안되므로 break   
                break;
            else
            {
                n_ret = needle;
                continue;
            }
        }
        h_ret++;
    }
    if (*h_ret == '\0' && *n_ret != '\0')    // 이 부분 수정 (틀린 부분 1)
        return (0);        
    return ((char *)haystack);
}
```
<br/><br/>

### 최종 코드
```c
// ft_strstr.c

#include "libft.h"

char    *ft_strstr(const char *haystack, const char *needle)
{
    const char *h_ret;
    const char *n_ret;
    
    h_ret = haystack;
    n_ret = needle;
    while (*h_ret && *n_ret)
    {
        if (*h_ret == *n_ret)
        {
            haystack = h_ret;
            while (*h_ret == *n_ret && *h_ret && *n_ret)   
            {
                h_ret++;
                n_ret++;
            }
            if (*h_ret == '\0' || *n_ret == '\0')    
                break;
            else
            {
                n_ret = needle;
                continue;
            }
        }
        h_ret++;
    }
    if (*h_ret == '\0' && *n_ret != '\0')    
        return (0);        
    return ((char *)haystack);
}
```
```c
// 라피신

char	*ft_strstr(char *str, char *to_find)
{
	char *tmp1;
	char *tmp2;

	if (*to_find == '\0')
		return (str);
	while (*str != '\0')
	{
		if (*str == *to_find)
		{
			tmp1 = str;
			tmp2 = to_find;
			while (*tmp1 == *tmp2 && *tmp1 != '\0' && tmp2 != '\0')
			{
				tmp1++;
				tmp2++;
			}
			if (*tmp2 == '\0')
				return (str);
		}
		str++;
	}
	return (0);
}
```
<br/><br/>

---
<br/><br/>

## strnstr
### 최종 코드
```
// ft_strnstr.c

#include "libft.h"

char *ft_strnstr(const char *haystack, const char *needle, size_t len)
{
    const char *h_tmp;
    const char *n_tmp;
    
    if (*needle == '\0')
        return ((char *)haystack);
    while (*haystack && len > 0)
    {
        if (*haystack == *needle)
        {
            h_tmp = haystack;
            n_tmp = needle;
            while (*haystack == *needle && *haystack && *needle && len > 0)
            {
                h_tmp++;
                n_tmp++;
                len--;
            }
            if (*n_tmp == '\0' && len >= 0)    // 이 부분 중요
                return ((char *)haystack);
        }
        haystack++;
        len--;
    }
    return (0);
}
```
위 코드에서 중요하다고 적어놓은 부분을 보자. if 문 조건의 의미를 설명하자면 `n_tmp` 문자열을 다 검사한 상태에서  
`len` 의 길이가 애초에 `needle`의 길이보다 길었을 경우나 같았을 경우에 `haystack`을 반환한다.  

> len의 길이가 needle보다 충분히 크고, needle 문자열이 len길이 범위내에서 haystack에 존재할 경우에는  
> 문자열을 다 찾고 나서(while문을 마치고 나서) len의 길이가 0보다 클 수 밖에 없다. 따라서 `len >= 0` 조건을 추가해 주었다.    

<br/>

needle 문자열이 haystack에 있는지 볼 때 주의할 점은 **needle 문자열 끝까지 봐야한다는 것이다.**  
즉, len 이 needle 문자열의 길이보다 작으면 needle 문자열의 끝까지 볼 수 없으므로 아래와 같이 출력된다.  
```
int main()
{
    char *s1 = "abcdef";
    char *s2 = "abc";
    
    printf("ft_strnstr : %s \n", ft_strnstr(s1, s2, 2));    // len이 0일때 실행 결과 (null)
    printf("strnstr    : %s \n", strnstr(s1, s2, 2));
    return 0;
}
```
```
// 실행 결과

ft_strnstr : (null)
strnstr    : (null)
```
<br/><br/>

---
<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 5시간 
* 학습에 참고한 사이트 :
    * 자가 학습

<br/>

## 8. 학습 내용에 대한 개인적인 총평
오늘은 strstr, strnstr 함수에 대한 개념을 지난 보고서를 통해 다시 짚어보고, 그 기능을 구현해보았다. strstr 함수는 문자열 안에서 문자열을 찾는 기능을 한다. 위의 보고서 내용에도 정리해놨지만 최종 코드를 구현하기까지 여러 시도를 해 보았다. 그리고 중간중간 그 함수의 인자로 여러가지 값들을 넣어 보면서 구현하지 못한 기능이 있는지 혹은 잘못된 결과가 나오도록 코드를 짰는지를 점검해 보았다. 또한 오늘 짠 최종 코드와 몇달전에 짠 라피신 코드를 비교하며 내가 짰지만 다른 방식으로도 기능을 구현할 수 있다는 것을 깨달았다.   
<br/>

## 9. 다음 학습 계획
- memcpy, memccpy, memmove, memchr, memcmp 함수의 기능을 살펴보고 함수에 대한 정보들을 정리한다.  
