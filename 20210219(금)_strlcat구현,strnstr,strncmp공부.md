## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [strlcat 구현](#strlcat-구현)
        - [최종 코드](#최종-코드) 
    - [strnstr](#strnstr)
    - [strncmp](#strncmp)
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-02-19(금)<br/><br/>
## 2. 학습 시간
* 10:00 ~ 12:00 (자가)
* 14:00 ~ 16:00 (카페)<br/><br/>
## 3. 학습 범위 및 주제
* strlcat 구현
* strnstr 학습
* strncmp 학습<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* strlcat 함수를 구현하고, strnstr함수와 strncmp함수의 개념을 정리한다.<br/><br/>
## 6. 상세 학습 내용
## strlcat 구현
- strlcat은 문자열 src 를 dst 의 끝에 추가해주는 함수이다.
- dst 의 마지막 위치에 src 을 `size - strlen(dst) - 1` 만큼 복사하고, 끝에 널문자를 삽입한다.  
이후, 결합되는 문자열의 총 길이를 반환한다.  
- 여기서 size는 대상 버퍼의 크기이고 size와 dst의 크기에 따라 반환값이 달라지게 된다.  
- size가 dst의 크기보다 작을 때, `strlen(src) + size`를 반환한다.
- size가 dst의 크기보다 클 때, `strlen(src) + strlen(dst)`를 반환한다.  

<br/>

### 시도 1
```
size_t    ft_strlcat(char *dst, const char *src, size_t dstsize)
{
    size_t len;
    
    len = 0;
    while (dst[len])    // dst와 dstsize를 비교하기 위해 dst의 길이를 먼저 계산한다.
        len++;
    if (dstsize < len)  // 여기서 비교를 하고, 이 조건을 만족하면 src의 길이를 계산해서 반환.
    {
        len = 0;
        while (src[len])
            len++;
        return (len + dstsize);
    }
    while (len + 1 < dstsize && *src)  // 만일 위 조건을 만족하지 않으면 연결작업.
        dst[len++] = *(src++);
    dst[len] = '\0';
    while (*(src++))     // case1의 경우 이 줄 위까지의 코드로는 src의 길이를 다 구할 수가 없다. 
        len++;           // 반환값이 (src의 길이 + dst의 길이)가 되어야 하므로 while문을 통해 나머지 src의 길이를 구한다.    
    return (len);
}
```
이 코드를 짜면서 고려했던 점을 아래 정리했다.  
<br/>

#### - case 1 : src의 길이가 dstsize보다 충분히 길 경우
```
// 테스트를 위한 코드

#include <stdio.h>
#include <string.h>

int main(void)
{
    char src[20] = "go go yogurt"; 
    char dst[30] = "gu";
    size_t n = 0;    // strlcat 반환값을 저장하기 위한 변수 선언
    
    n = strlcat(dst, src, 4);
    
    printf("return : %zu \n", n);
    printf("%s \n", dst);
    return (0);
}
```
```
// 실행 결과

return : 14
gug
```
<br/><br/>

#### - case 2 : dstsize의 길이가 (src길이 + dst길이)를 합친 것 보다 길 경우 
이 경우를 해결하기 위해 while문 조건에 `*src != '\0'`
```
n = strlcat(dst, src, 20);
```
```
return : 14
gugo go yogurt
```
<br/><br/>

#### - case 3 : dstsize = dst의 길이
```
n = strlcat(dst, src, 2);
```
```
return : 14
gu
```
<br/><br/>

#### - case4 : dstsize < dst의 길이
```
n = strlcat(dst, src, 1);    // dstsize = 0 일땐 12가 return됨.
```
```
return : 13
gu
```
<br/><br/>

#### - case5 : src의 주소값을 직접 옮기는 방법이 맞나?
```
int main(void)
{
    char src[20] = "go go yogurt"; 
    char dst[30] = "gu";
    size_t n = 0;   
    
    n = strlcat(dst, src, 4);
    
    printf("return : %zu \n", n);
    printf("dst : %s \n", dst);
    printf("src : %s \n", src);
    return (0);
}
```
```
return : 14
dst : gug
src : go go yogurt
``` 
> `call-by-reference`는 주소값으로 가서 그 주소에 있는 값을 바꾸는건데, 그래서 함수 종료 후에도 그 주소에는 바뀐 값이 남아있다.  
> 주소를 바꾸는 개념이 아니고... `ft_strlcat` 함수 안에서 주소를 바꾸는 거는 단순히 변수 `src`가 가지고 있는 주소를 바꾸는 것.  
> **`src`는 주소값을 담고 있는 변수일뿐...**  

<br/>

---
<br/><br/>

### 최종 코드 
```
size_t    ft_strlcat(char *dst, const char *src, size_t dstsize)
{
    size_t len;
    
    len = 0;
    while (dst[len])    
        len++;
    if (dstsize < len)  
    {
        len = 0;
        while (src[len])
            len++;
        return (len + dstsize);
    }
    while (len + 1 < dstsize && *src)  
        dst[len++] = *(src++);
    dst[len] = '\0';
    while (*(src++))     
        len++;             
    return (len);
}
```
```
// 라피신

unsigned int	ft_strlcat(char *dest, char *src, unsigned int size)
{
	unsigned int ret;    // 길이

	ret = 0;
	while (*dest && ret < size)
	{
		dest++;
		ret++;
	}
	while (*src && ret + 1 < size)
	{
		*dest = *src;
		dest++;
		src++;
		ret++;
	}
	if (ret < size)
		*dest = '\0';
	while (*src)
	{
		ret++;
		src++;
	}
	return (ret);
}
```
<br/><br/>

---
<br/><br/>

## strnstr
### 함수의 기능
- 문자열 내에서 부분 문자열을 탐색하는 함수
- `strstr` 함수는 NULL 종료 전까지의 haystack 문자열 내에서 NULL 종료 전까지의 needle 문자열을 찾아서 첫번째로 나온 결과를 탐색한다.  
- `strnstr` 함수는 **종료전까지의 문자들 중 haystack 문자열 내에서 찾은 needle 문자열 중 첫번째로 나온 결과를 찾는다.**  
(문자열을 최대 `len`의 수까지만 탐색한다.)
- `haystack` 문자열에 `len` 길이 중에서 `needle` 문자열을 찾는 것이다.
- 문자열들은 `\0`을 만나면 더이상 찾지 않는다.  

<br/>

### 함수의 원형
```
char    *strnstr(const char *haystack, const char *needle, size_t len);
```
<br/>

### 헤더파일
```
#include <string.h>
```
<br/>

### 매개변수
- haystack : 탐색할 전체 문자열
- needle : 추출해 낼 부분 문자열
- size_t len : 문자열 내에서 탐색할 최대 범위. len 만큼의 길이에서 탐색을 한다.

<br/>

### 반환값
- needle 값이 비어 있으면 `haystack`을 반환한다.
- haystack 문자열에서 needle 문자열을 찾지 못하면 `NULL`을 반환한다.  
- needle 문자열을 찾으면 haystack에 **needle 문자열 시작 부분 위치 주소**를 반환한다.

<br/>

### 예제
```
char big[] = "go go yogurt";
char little[] = "gu";
printf("return(gurt) : %s\n", strnstr(big,little,14));	// return(gurt) : gurt
printf("return(null) : %s\n", strnstr(big,little,9));	//return(null) : null
```
<br/><br/>

---
<br/><br/>

## strncmp
### 함수의 기능
- 매개변수로 들어온 **두 개의 문자열을 비교하여 문자열이 완전히 같다면 0을 반환하고, 다르면 음수 혹은 양수를 반환하는 함수**이다.  
- 여기서 -1, 1은 매개변수로 들어온 문자열들을 비교하다가 다른 문자가 나왔을 때, 그 문자의 아스키 코드 값에 의해서 정해진다. 
  
<br/>

### 함수의 원형
```
int    strncmp(const char *s1, const char *s2, size_t n);
```
<br/>

### 헤더파일
```
#include <string.h>
```
<br/>

### 매개변수
- s1 : 비교할 문자열 1
- s2 : 비교할 문자열 2
- n : 비교할 문자열 길이. 검사할 문자의 갯수.

> s1, s2 문자열 길이보다 큰 값을 넣게 되면 알아서 문자열 전체를 비교한다.  
> 예를 들어 s1이 문자 5개, s2가 문자 10개로 이루어져 있을 경우 `n`에 10을 넣든 100을 넣든 s1이 5이므로 5개까지의 문자만 비교하게 된다.  

<br/>

### 반환값
- **s1 < s2**인 경우 **음수** 반환
- **s1 > s2**인 경우 **양수** 반환
- **s1 == s2**인 경우 **0**을 반환 

<br/>

### 추가 설명
- strcmp, strncmp 함수가 앞에서부터 각각 문자를 비교할 때, 아스키 코드값으로 비교를 한다.
- **각 문자를 아스키 코드로 비교**하기 때문에, 각 문자별 숫자가 정해져 있으므로 대소문자 비교가 가능하다.  

<br/>

### 예제
```
const char* str1 = “BlockDmask”
const char* str2 = “BlockFmask”
strncmp(str1, str2, 5);          //‘Block” 까지만 검사하므로 0 반환
strncmp(str1, str2, 6);          // D < F 이므로 음수 반환
```
<br/><br/>

---
<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 4시간 
* 학습에 참고한 사이트 :
    * [strlcat 구현](https://hanul-dev.netlify.app/c/strlcat()-%EA%B5%AC%ED%98%84/)
    * [strnstr 설명](https://velog.io/@mjung/function-strstr-strnstr)
    * [strncmp 자세한 설명](https://blockdmask.tistory.com/391)
    * [strnstr, strncmp 구현](https://minsoftk.tistory.com/33)

<br/>

## 8. 학습 내용에 대한 개인적인 총평
strlcat 함수를 구현해봤는데, 이 함수는 지금 봐도 반환값을 이해하기가 어렵다. dstsize가 dst의 길이보다 작을 경우 문자열 연결이 이루어지지 않고 src의 길이와 dstsize의 합이 반환이 된다. 왜 이 값이 반환이 되는지 정확하게 설명된 글을 아직 찾지 못했다. dstsize는 우리가 만들고자 하는 문자열의 최종 길이인데, 여기에 src의 길이를 왜 더해줄까? '만들려고 시도했던'거라고 끼워맞춰 생각해보면 원래는 dst에 src를 연결해서 (dst의 길이 + src 길이)를 반환하는게 이 함수의 목표인데, dstsize가 dst 길이보다 짧아서 연결하지 못하니 (dst의 길이 + src 길이)에서 dst 길이를 dstsize로 대체했다고 생각해야겠다.   
<br/>

## 9. 다음 학습 계획
- strnstr 함수 및 strncmp 함수 구현
