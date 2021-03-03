## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [memmove 구현](#memmove-구현)
        - [최종 코드](#최종-코드) 
    - [memchr 구현](#memchr-구현)
        - [최종 코드](#최종-코드-1)  
    - [memcmp 구현](#memcmp-구현)
        - [최종 코드](#최종-코드-2) 
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-02-26(금)<br/><br/>
## 2. 학습 시간
* 15:00 ~ 19:00 (자가)
* 20:00 ~ 21:00 (자가) <br/><br/>
## 3. 학습 범위 및 주제
* memmove 함수 구현
* memchr 함수 구현
* memcmp 함수 구현 <br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* ft_memmove, ft_memchr, ft_memcmp 함수의 기능을 구현하고, 테스트해본다.<br/><br/>
## 6. 상세 학습 내용
## memmove 구현
### 시도 1
```c
// ft_memmove.c

#include "libft.h"

void    *ft_memmove(void *dst, const void *src, size_t len)
{
    char *tmp1;
    const char *tmp2;
    
    tmp1 = dst;
    tmp2 = src;
    while (len > 0)
    {
        *(tmp1++) = *(tmp2++);
        len--;
    }
    return (dst);
}
```
```
// main.c

int main(void)
{
    char str[] = "memmove can be very useful......";
    ft_memmove(str + 20, str + 15, 11);    // 기대한 결과 memmove can be very very useful.
    puts(str);
    return (0);
}
```
```
memmove can be very very very v.
```
결과가 위와 같이 원하는 결과와 다르게 나왔다.  
**다른 버퍼에 str 내용을 저장을 해놓는 방법**으로 접근해야 할 것 같다.  
<br/><br/>

### 최종 코드
```c
// ft_memmove.c

#include "libft.h"

void    *ft_memmove(void *dst, const void *src, size_t len)
{
    size_t i;
    char   *d_tmp;
    char   s_tmp[len];
    
    i = 0;
    d_tmp = dst;
    ft_memcpy(s_tmp, src, len);
    while (i < len)
    {
        d_tmp[i] = s_tmp[i];
        i++;
    }
    return (dst);
}
```
```
$ gcc main.c ft_memmove.c ft_memcpy.c
$ ./a.out
memmove can be very very useful.
```
<br/><br/>

---
<br/><br/>

## memchr 구현
### 최종 코드
```c
// ft_memchr.c

#include "libft.h"

void    *ft_memchr(const void *s, int c, size_t n)
{
    const char *ret;
    
    ret = (const char *)s;
    while (n-- > 0)
    {
        if (*ret == (unsigned char)c)
            return ((void *)ret);
        ret++;
    }
    return (0);
}
```
```c
// main.c

int main(void)
{
    char *pch;
    char str[] = "Example string";
    pch = (char *)ft_memchr(strm 'p', strlen(str));
    
    if (pch != NULL)
        printf("'p' found at position %ld. \n", pch - str - 1);
    else
        printf("not found. \n");
    return (0);
}
```
```
'p' found at position 5
```
<br/><br/>

---
<br/><br/>

## memcmp
- `man memcmp` 내용 중 일부 : 처음 두 바이트 간의 차이를 반환합니다. (treated as unsigned char values)
### 시도 1 : strncmp 와 비교
```
// ft_strncmp.c

#include "libft.h"

int ft_strncmp(const char *s1, const char *s2, size_t n)
{
    while (*s1 && *s2 && n > 0)
    {
        if (*s1 == *s2)
        {
            s1++;
            s2++;
            n--;
        }
        else
            return (*s1 - *s2);
    }
    if (n == 0 || *s1 == *s2)
        return (0);
    else
        return (*s1 - *s2);
}
```
```
// ft_memcmp.c

int ft_memcmp(const void *s1, const void *s2, size_t n)
{
    const unsigned char *tmp1;
    const unsigned char *tmp2;
    
    tmp1 = (const unsigned char *)s1;
    tmp2 = (const unsigned char *)s2;
    while (n > 0)    // 이 안에서 '\0'이 검사될 것.
    {
        if (*tmp1 == *tmp2)
        {
            tmp1++;
            tmp2++;
            n--;
        }
        else
            return (*tmp1 - *tmp2);
    }
    return (0);    // n 바이트까지 검사를 맞췄는데 리턴한게 없다? 두 문자열은 n 바이트까지 일치한다.
}
```
- ft_memcmp 는 `\0`을 신경쓰지 않기 때문에 ft_strncmp 에서 `\0`을 신경 쓴 코드들을 삭제했다.  
- 매개변수의 자료형이 void 형이므로 이를 `unsigned char`형으로 바꿔주었다. (man memcmp) 

<br/>

#### - 그런데 두 문자열이 완벽히 일치하는 경우, 예로 `\0`과 `\0` 또는 `abc\0`과 `abc\0` 이런 경우는 n bytes까지 비교할 필요가 있나?
```c
// main.c

#include <string.h>
#include <stdio.h>
#include <unistd.h>

int main(void)
{
    char str1[] = "abc";
    char str2[] = "abc";
    printf("%d \n", memcmp(str1, str2, 10));

    write(1, str1, 10);
    printf("\n");
    write(1, str2, 10);
    return (0);
}
```
실행 결과,
```
-97
abc@
abcabc
```
str1과 str2를 각각 출력해보았는데, 4 바이트부터는 내가 설정해주지 않은 값이 들어갔다.  
이것을 미루어 볼 때 **`\0`과 상관없이 `n` 바이트까지 비교함을 알 수 있다.**    
<br/><br/>

### test
```
// main.c

#include <string.h>
#include <stdio.h>
#include <unistd.h>

int main(void)
{
    char str1[] = "abc";
    char str2[] = "abc";
    printf("memcmp    : %d \n", memcmp(str1, str2, 10));
    printf("ft_memcmp : %d \n", ft_memcmp(str1, str2, 10));

    write(1, str1, 10);
    printf("\n");
    write(1, str2, 10);
    return (0);
}
```  
```
memcmp    : -97
ft_memcmp : -97
abc@<
abcabc
```
참고로 `./a.out`할 때마다 wirte 출력 부분에는 다른 문자열이 출력됨. 즉, 쓰레기값 들어감.  

매개변수 n의 값과 write함수의 세번째 인자를 바꿔가면서 테스트해 보았다.
```
// n = 0, 3

memcmp    : 0
ft_memcmp : 0
abc
abc
```
<br/><br/>

### 최종 코드
```c
#include "libft.h"

int ft_memcmp(const void *s1, const void *s2, size_t n)
{
    const unsigned char *tmp1;
    const unsigned char *tmp2;
    
    tmp1 = (const unsigned char *)s1;
    tmp2 = (const unsigned char *)s2;
    while (n > 0)
    {
        if (*tmp1 == *tmp2)
        {
            tmp1++;
            tmp2++;
            n--;
        }
        else
            return (*tmp1 - *tmp2);
    }
    return (0);
}
```
<br/><br/>

---
<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 5 시간 
* 학습에 참고한 사이트 :
    * 자가 학습

<br/>

## 8. 학습 내용에 대한 개인적인 총평
memmove, memchr, memcmp 함수를 구현해보았다. string 함수와 다르지만 기능은 비슷해서 string 함수를 바탕으로 memory 함수를 만들었다. **`\0`을 검사하지 않는다는 차이점만 잘 이해하면 될 것 같다.** 오늘 만든 함수 파일을 개인 깃에 업로드하려다가 push가 안되는 문제가 발생했다. (vnc 접속이 제한되기 이전에는 잘 됐는데, ssh 접속만 허용되고 나서)해결방안을 찾아봤는데, `git push origin +master`이렇게 push를 해줄때 **+옵션**을 주면 오류를 무시하고 push가 된다고 했다. 그 방법으로 push를 완료했는데, 원래 개인 깃에 있었던 파일들은 다 사라지고 오직 ft_memmove.c, ft_memchr.c, ft_memcmp.c 파일들만 업로드 되었다. 사라진 파일들을 다시 작성하는 수고스러움을 겪었으니 **+ 옵션 사용에 주의해야 한다**는 것을 깊이 새겨야겠다.  
<br/>

## 9. 다음 학습 계획
- bzero, isalpha, isdigit, isalnum 함수 개념 정리 및 구현
