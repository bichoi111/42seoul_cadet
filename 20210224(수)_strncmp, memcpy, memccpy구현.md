## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [strncmp 구현](#strstr-구현)
    - [memcpy 구현](#memcpy-구현)
    - [memccpy 구현](#memccpy-구현)
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-02-24(수)<br/><br/>
## 2. 학습 시간
* 17:00 ~ 19:00 (자가)
* 20:00 ~ 23:00 (자가)<br/><br/>
## 3. 학습 범위 및 주제
* strncmp 함수 구현
* memcpy, memccpy 함수 구현<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* strncmp, memcpy, memccpy 함수를 구현하고 테스트한다. <br/><br/>
## 6. 상세 학습 내용
## strncmp 구현
- strncmp 함수에서 매개변수 `n`에 `0`을 넣으면 `0`이 반환된다.  
- strncmp 함수에서 반환값은 `*s1의 아스키 코드값 - *s2의 아스키 코드값` 이다.

### 최종 코드
```c
// ft_strncmp.c

#include "libft.h"

int    ft_strncmp(const char *s1, const char *s2, size_t n)
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
<br/><br/>

---
<br/><br/>

## memcpy 구현
### 최종 코드
```
#include "libft.h"

void    *ft_memcpy(void *dst, const void *src, size_t n)
{
    size_t i;
    char *tmp1;      // 얘도 const로 선언해버리면 안됨. "read-only variable is not assignable" 오류
    const char *tmp2;
    
    i = 0;
    tmp1 = dst;
    tmp2 = src;
    while (i < n)
    {
        tmp1[i] = tmp2[i];
        i++;
    }
    return (dst);
}
```
<br/>

### 테스트
#### - int형 배열을 인자로 넣었을 때
```
int main(void)
{
    int src[] = {133, 2, 4};
    int dst[3];
    
    ft_memcpy(dst, src, sizeof(int) * 3);
    for (int i = 0; i < 3; ++i)
    {
        printf("%d \n", dst[i]);
    }
    return (0);
}
```
```
133
2
4
```
개념 정리할 때 살펴보았던 예제와 유사한데, 내가 짠 `ft_memcpy.c` 를 보면 `void` 포인터를 인자로 받아 함수 내에서 `char *`로 바꾸는 과정을 거친다.  
그래서 `int`형 배열을 인자로 받을 때도 `char` 형으로 형변환이 되었을 때, 결과가 제대로 출력될까 하는 의문점에 테스트해 본 예제이다.   
결과가 잘 출력되는 것을 확인할 수 있다.  
<br/><br/>

---
<br/><br/>

## memccpy 구현
### 최종 코드
```
#include "libft.h"

void *ft_memccpy(void *dst, const void *src, int c, size_t n)
{
    char *tmp1;
    const char *tmp2;
    
    tmp1 = dst;
    tmp2 = src;
    while (*tmp2 != (unsigned char)c && n > 0)
    {
        *(tmp1++) = *(tmp2++);
        n--;
    }
    if (*tmp2 == (unsigned char)c)
    {
        *tmp1 = *tmp2;
        return (tmp1 + 1);
    }
    return (0);
}
```
- **10행 (while문)** : 문자열 src의 문자 중 c와 일치하면 while문을 빠져나오도록 하는 조건.  
c를 발견하면 복사를 중지해야 하니까. 그리고 `n > 0`으로 n 바이트까지만 copy하도록 조건을 줌.  
- **15행 (if 문)** : 만일 문자 **c**가 발견되어 while문이 종료되었다면 거기(c와 일치하는 문자)까지 복사를 해주고,  
c의 다음 바이트의 주소를 반환한다.  
- return (0) : 문자 c가 발견되지 않으면 null 반환  

<br/><br/>

### memccpy 함수의 특징
- c 문자가 포함되지 않을 때 n의 값에 주의하기
```
int main()
{
    char dst[] = "GeeksForGeeks is for programming geeks";
    char src[] = "hi hello";
    
    printf("answer : %s \n", memccpy(dst, src, 'a', 10));
    return (0);
}
```
```
$ ./a,out
zsh: abort
```
이렇게 실행하면 `zsh: abort`오류가 뜨는데 이유는 `n`에 `src`의 길이, 9를 넘는 값을 넣었기 때문이다.  
9 이하의 값을 넣어주면 `answer : (null)`이 출력된다.  
그러나 내가 구현한 함수는 9 초과의 값을 넣어도 오류가 뜨지 않고 `(null)`이 출력된다.  
<br/><br/>

- c 문자가 포함될 때 n의 값
```
int main()
{
    char dst[] = "GeeksForGeeks is for programming geeks";
    char src[] = "hi hello";
    
    printf("answer : %s \n", memccpy(dst, src, 'o', 10));
    printf("my     : %s \n", ft_memccpy(dst, src, 'o', 10));
    return (0);
}
```
```
$ ./a,out
answer : Geeks is for programming geeks
my     : Geeks is for programming geeks
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
아직 구현해야 할 함수도 많이 남았지만 이제까지 약 10개 정도의 함수를 구현해 봤는데, 한가지 찝찝한 점은 본 함수의 기능을 완벽하게 구현하지는 못했다는 점이다. 예를 들어 'memccpy 구현' 부분을 보면, 실제 memccpy 함수는 n의 값에 특정 값(c 문자가 포함되지 않을 때 n에 src의 길이보다 큰 값)을 넣으면 실행파일 오류가 뜨는데, 내가 구현한 ft_memccpy 함수는 실행파일 오류가 뜨지 않는다는 점이다. 앞서 구현했던 함수들에서도 본 함수는 overflow 오류 문구가 나오는데, 내가 짠 함수들은 오류처리가 되지 않고 있다. 이 점 때문에 평가를 받을 때 지적을 받을 것 같아 걱정이 되지만, 내가 라피신때 구현해보고 평가에서 100점을 맞았던 함수들도 지금 다시 실행해보니 그런 오류들이 처리되지 않았던 것으로 보아 따로 오류 처리를 해줘야 하는 것은 아닌 것 같다.  
<br/>

## 9. 다음 학습 계획
- memmove, memchr, memmcmp 함수를 구현해본다.  
