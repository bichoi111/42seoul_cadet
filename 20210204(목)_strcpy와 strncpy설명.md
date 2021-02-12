## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [strcpy](#strcpy)
    - [strncpy](#strncpy)
    - [strcpy, strncpy 간단한 사용법](#strcpy-strncpy-간단한-사용법)
    - [strcpy, strncpy 주의사항](#strcpy-strncpy-주의사항)
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  

<br/> 

## 1. 학습 날짜
* 2021-02-04(목)<br/><br/>
## 2. 학습 시간
* 12:00 ~ 15:00 (자가)<br/><br/>
## 3. 학습 범위 및 주제
* strcpy와 strncpy의 기본 개념을 정리한다.
* strcpy, strncpy 예제를 살펴본다.
* strcpy와 strncpy의 차이점을 정리한다.<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* strcpy와 strncpy 함수를 이해한다.<br/><br/>
## 6. 상세 학습 내용
# strcpy
### 함수의 기능
문자열을 복사하는 함수로, src 문자열 전체를 dst로 복사한다. (`\0`문자까지)  
<br/>

### 함수의 원형
```
char    *strcpy(char *dst, const char *src);
```
<br/>

### 헤더파일
```
C언어 : <string.h>
C++ : <cstring>
```
<br/>

### 매개변수
- src : 복사할 문자열
- dst : 복사당할 문자열

<br/>

### 반환값
- dst   

<br/><br/>

# strncpy
### 함수의 기능
src의 문자열을 dst로 복사하는데, `len`개의 문자만큼만 복사한다.  
<br/>

### 함수의 원형
```
char    *strncpy(char *dst, const char *src, size_t len);
```
<br/>

### 헤더파일
```
C언어 : <string.h>
C++ : <cstring>
```
<br/>

### 매개변수
- src : 복사할 문자열
- dst : 복사당할 문자열
- len : src에서 복사할 문자의 갯수

<br/>

### 반환값
- dst   

<br/> 

### 추가 설명
- src길이가 len보다 짧으면, dst의 나머지는 `\0`으로 채워진다. 그렇지 않으면 dest는 중단되지 않을 것이다.  
- 10개의 문자가 들어갈 수 있는 공간에, hello 5글자를 넣었다면 나머지 5공간은 널문자로 채워진다. 남은 복사길이 만큼은 널문자로 채운다.
- src와 dst의 문자열이 겹쳐져서는 안된다. 그 행위는 정의되지않기 때문.
 
<br/><br/>

---
<br/><br/>

## strcpy, strncpy 간단한 사용법
- strcpy
```
char str[] = "BLACK";
char dst[100];
strcpy(dst, src);
```
<br/>

- strncpy
```
char src[] = "BLACK";
char dst[100];
strncpy(dst, src, sizeof(src));
```

<br/><br/>

---
<br/><br/>

## 예제
## - strcpy
복사받을 배열의 길이가 충분할 때와 그렇지 않을 때
```
#include<stdio.h>     //C++ <iostream> or <cstdio>
#include<string.h>    //C++ <cstring>
 
int main(void)
{
    char origin[] = "BlockDMask";    // "BlockDMask\0" 이므로 size = 11;
    char dest1[20];
    char dest2[10];
    char dest3[] = "STRCPY_EXAMPLE";    // size = 15;
 
    //case1 : 빈 배열에 전체를 복사할때
    strcpy(dest1, origin);
 
    //case2 : 꽉 차있는 배열에 전체를 복사할때
    //strcpy(dest2, origin);    // run time error
 
    //case3 : 꽉 차있는 배열에 전체를 복사할때
    strcpy(dest3, origin);
 
    printf("case1 : %s\n", dest1);      //ok
    //printf("case2 : %s\n", dest2);    //run time error
    printf("case3 : %s\n", dest3);      //ok
    return 0;
}
```
```
case1 : BlockDMask
case3 : BlockDMask
```
> 1번 케이스를 보면 비어있는 배열에 origin 문자열이 잘 복사 된것을 확인할 수 있다.  
> 3번 케이스를 보면 꽉 찬 배열에 origin 문자열을 복사하면 기존 것은 안나오고 orgin만 나오는 것을 확인 할 수 있다.  
> 3번 케이스에 문자열 dest3이 origin보다 더 긴데, 복사해서 넣으면 왜 origin만 나오나?  
> 그것에 대한 해답은 아래 **strcpy 주의사항**에 있다.

<br/>

## - strncpy
```
#include<stdio.h>     //C++ <iostream> or <cstdio>
#include<string.h>    //C++ <cstring>

int main(void)
{
    char origin[] = "BlockDMask";    // "BlockDMask\0" 이므로 size = 11;
    char dest1[20];
    char dest2[] = "abcdefghijklmnop";    // size = 17;
    char dest3[] = "STRNCPY_EXAMPLE";    // size = 16;
    char dest4[10];
 
    //case1 : 빈 배열에 전체를 복사할때
    strncpy(dest1, origin, sizeof(origin));    // sizeof는 널문자 포함한 문자열의 길이 
 
    //case2 : 꽉 차있는 배열에 전체를 복사할때
    strncpy(dest2, origin, sizeof(origin));
 
    //case3 : 꽉 차있는 배열에 일부만 복사할때
    strncpy(dest3, origin, 4);    // 오!
 
    //case4 : 빈 배열에 일부만 복사할때
    strncpy(dest4, origin, 4);    // 그럼이건?
 
    printf("case1 : %s\n", dest1);
    printf("case2 : %s\n", dest2);
    printf("case3 : %s\n", dest3);
    printf("case4 : %s\n", dest4);
    return 0;
}
```
```
case1 : BlockDMask
case2 : BlockDMask
case3 : BlocCPY_EXAMPLE
case4 : BlocㅊㅊㅊㅊㅊㅊBlocCPY_EXAMPLE
```
> 1번 케이스 : 비어있는 배열에 origin 문자열의 전체가 잘 복사 된 것을 확인할 수 있다.  
> 2번 케이스 : 꽉 차있는 베열에 origin 문자열의 전체를 복사했는데, 분명히 dest2가 더 긴데 왜 origin만 나오지?  
> 3번 케이스 : origin의 4번째 까지 Bloc를 잘 복사해서 dest3에 붙여넣은것을 알 수 있다.  
> 4번 케이스 : 빈 배열에 일부 문자열만 복사했는데, 왜 쓰레기 값들이 나올까?


2번과 4번 케이스가 궁금하다면, 아래 strncpy 주의사항으로 GOGO!  
<br/><br/>

---
<br/><br/>

## strcpy, strncpy 주의사항
### 1. strcpy는 문자열 끝(\0)까지 복사한다.
char\*, char[] 타입의 문자열 끝에는 `\0` 이 있다. 이걸로 문자열의 끝을 판단하게 되는데,  
strcpy로 복사를 하게 되면 문자열의 끝을 나타내는 `\0` 까지 복사가 된다.

위 strcpy 예제의 case3번을 보면,
```
char origin[] = "BlockDMask";       // "BlockDMask\0" 이므로 size = 11;
char dest3[] = "STRCPY_EXAMPLE";    // size = 15;
 
//case3 : 꽉 차있는 배열에 전체를 복사할때
strcpy(dest3, origin);
 
printf("case3 : %s\n", dest3);    // ok
```
> strcpy 함수를 통해서 dest3에 origin을 복사하게 되면, dest3은 메모리 상에서 아래와 같이 될 것 이다.  
> `BlockDMask\0PLE\0` 이런 형식이 된다.
> strcpy를 통해서 배열의 끝인 `\0`까지 복사가 되었기 때문에, dest3 문자열의 10번째 인덱스에 `\0`이 존재하게 됨.  
> 그러므로 dest3의 문자열은 "BlockDMask\0" 여기까지 인식하게 된다.  

<br/>

아래의 예제를 보면 이해가 더 잘 될 것이다.
```
char origin[] = "BlockDMask";
char dest3[] = "STRCPY_EXAMPLE";
strcpy(dest3, origin);
 
printf(" case3 before : %s\n", dest3);
    
dest3[10] = 'y';    //'\0' -> 'y'
printf(" case3 after  : %s\n", dest3);
```
```
case3 before : BlockDMask
case3 after  : BlockDMaskyPLE
```
<br/><br/>

### 2. strncpy로 복사했을 경우 적절한 위치에 \0을 넣어줘야 하는지 살펴본다.
case2, 4번 둘다 문자열의 끝을 가리키는 `\0` 때문에 발생하게 되었다.  
strcpy와 달리 **strncpy는 기본적으로 `\0`를 상관하지 않고 `n`의 길이만큼만 딱 복사한다.**  
```
char origin[] = "blockdmask";

strncpy(dest2, origin, sizeof(origin)) 을 하게되면 blockdmask\0 이므로 \0 까지 복사가 될 것이고

strncpy(dest2, origin, 4) 을 하게되면 bloc 이므로 \0 없이 복사가 될 것이다.
```
<br/>

- case2번 정리
```
char origin[] = "BlockDMask";
char dest2_1[] = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaa";
char dest2_2[] = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaa";
    
strncpy(dest2_1, origin, sizeof(origin));
strncpy(dest2_2, origin, sizeof(origin) - 1);
 
printf(" case2_1 : %s\n", dest2_1);
printf(" case2_2 : %s\n", dest2_2);
```
```
case2_1 : BlockDMask
case2_2 : BlockDMaskaaaaaaaaaaaaaaaaaaa
```
case2_2 처럼 문자열을 연결하고 싶다면, `sizeof(origin) - 1`을 해서 문자열의 끝에 있는 `\0` 이것을 빼고 복사하면 된다.  
<br/>

- case4번 정리
```
char origin[] = "BlockDMask";
char dest4_1[20];
char dest4_2[20];
 
strncpy(dest4_1, origin, 4);
strncpy(dest4_2, origin, 4);
 
printf(" case4_1 : %s\n", dest4_1);
 
dest4_2[4] = '\0';
printf(" case4_2 : %s\n", dest4_2);
```
<center><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile5.uf.tistory.com%2Fimage%2F99DCA2455CDC5D1A051F4C"></center>  
<br/>

> case4_2 처럼 문자열을 복사한 다음에, **문자열 끝을 알리는 `\0`을 적절한 위치에 넣어주면 된다.** (dest4_2[4] = '\\0')  
> case4_1 의 경우에는 printf를 통해서 문자열을 출력하려 하는데 '\\0'이 보이지 않아서, 순서대로 메모리를 쭉 훑다가 origin에 있는  
> `BlockDMask\0` 을 발견하고 출력한 것이다. 당연하게도 메모리 중간중간에 비어있는 곳은 쓰레기 값이 출력 되겠지?  

<br/><br/>

### 3. strncpy로 복사했을경우 n의 길이를 주의해야 한다.
```
strncpy(dest, origin, n)
```
> - n의 크기는 sizeof(origin)보다 작거나 같아야 한다. (휴먼에러 발생할 수 있음)  
> - 또한, dest의 길이보다 n은 작거나 같아야합니다. (런타임 에러)

<br/>

```
1. n <= sizeof(origin)

2. n <= sizeof(dest)
```

<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 3시간 
* 학습에 참고한 사이트 :
    * [strcpy, strncpy](https://blockdmask.tistory.com/348) : 다른 함수들도 잘 정리돼있음!

<br/>

## 8. 학습 내용에 대한 개인적인 총평
strcpy와 strncpy 함수의 개념을 정리하며 라피신 때 공부했던 기억을 상기시켰다. strncpy는 **src가 len 길이보다 작은 경우 나머지를 \\0문자로 채운다**는 점이 중요하다. 실제로 예전에 틀렸던 부분이기도 하다. 또 strcpy와 strncpy 함수를 사용할 때 **주의사항**을 눈여겨보면서 overflow같은 에러에 주의해야겠다. 다음에는 strlcpy에 대한 개념을 정리하면서 함수 구현도 해 볼 예정이다.  
<br/>

## 9. 다음 학습 계획
- 배열의 sizeof 포인터의 sizeof
- strlcpy 학습
