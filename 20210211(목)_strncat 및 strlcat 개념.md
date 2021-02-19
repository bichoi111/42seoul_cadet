## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [strncat](#strncat)
        - [함수의 기능](#함수의-기능)
        - [함수의 원형](#함수의-원형)
        - [헤더파일](#헤더파일)
        - [매개변수](#매개변수)
        - [반환값](#반환값)
        - [추가 설명](#추가-설명)
    - [strlcat](#strlcat)
        - [함수의 기능](#함수의-기능)
        - [함수의 원형](#함수의-원형)
        - [헤더파일](#헤더파일)
        - [매개변수](#매개변수)
        - [반환값](#반환값)
        - [추가 설명](#추가-설명) 
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-02-11(목)<br/><br/>
## 2. 학습 시간
* 17:00 ~ 20:00 (자가)<br/><br/>
## 3. 학습 범위 및 주제
* strncat 함수의 개념을 학습한다.
* strlcat 함수의 개념을 학습한다.<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* strncat 함수와 strlcat 함수를 비교하며 이해한다.<br/><br/>
## 6. 상세 학습 내용
## strncat
### 함수의 기능
- s1 문자열 뒤에 n 길이 만큼의 s2 문자열을 연결한다.   

<br/>

### 함수의 원형
```
char    *strncat(char *s1, const char *s2, size_t n)
```
<br/>

### 헤더파일
```
C언어 : <string.h>
C++ : <cstring>
```
<br/>

### 매개변수
- char \*dst : 도착 문자열
- const char \*src : dest 뒤에 연결할 문자열
- size_t n : 문자열에서 합할 문자 개수

<br/>

### 반환값
- s1 (dst)

<br/>

### 추가 설명
- s2(src)에서 n바이트를 s1(dst)에 이어 붙인다.
- s2(src)에 n바이트 이상이 포함되어 있으면 null 종료되지 않아도 된다.
- strcat 에서와 같이 s1(dst)의 결과 문자열은 항상 `null`로 끝난다.
- s2에 n개 이상의 바이트가 포함되어 있으면, strncat은 `n + 1` 바이트를 s1에 쓴다.(n은 src와 null 종료 바이트)
- 따라서 s1(dst)의 크기는 적어도 `strlen(dst) + n + 1`이어야 한다.
- strncat 함수는 s1 버퍼 크기가 `strlen(s1) + n` 보다 작으면 버퍼 오버 플로우 버그가 발생한다.  
C11에서는 이를 개선한 strncat_s 함수를 제공한다. 그리고 strncat 함수에서는 문자열을 합한 맨 끝에 종료 문자를 추가한다.  

<br/><br/>

---
<br/><br/>

## strlcat
### 함수의 기능
- dst끝에 src 문자를 덧붙인다. 최대 (dstsize - strlen(dst) - 1)만큼의 문자가 덧붙여지고 끝에 널문자를 삽입한다.    
- strcat 함수와 동일한데, 보안을 위해 strcat 대신 사용할 목적으로 만들어졌다.  
- **dst의 기존 데이터에 src 데이터를 `dstsize`만큼 붙여 넣는다.**  
(***size = dst 길이 + 붙일 데이터의 길이 + NULL***)  
즉, dstsize는 (dst 길이 + NULL 길이)보다 클 때부터 src 데이터가 들어간다.

<br/>

### 함수의 원형
```
size_t    strlcat(char *dst, const char *src, size_t dstsize)
```
<br/>

### 헤더파일
```
C언어 : <string.h>
C++ : <cstring>
```
<br/>

### 매개변수
- char \*dst : 도착 문자열
- const char \*src : dest 뒤에 연결할 문자열
- size_t dstsize : dst길이 + 붙일 데이터의 길이 + NULL

<br/>

### 반환값
- **dst와 복사된 src 길이의 총합 (`NULL` 값 제외)** 즉, 결합되는 문자열의 총 길이
- 만일 dst 길이가 5이고 src에서 2바이트만 복사했다면 반환 값은 7이 된다.  
- `dstsize`가 `dst`의 크기보다 작을 때, `strlen(src) + dstsize`를 반환한다.  
- `dstsize`가 `dst`의 크기보다 클 때, `strlen(src) + strlen(dst)`를 반환한다.  

> 처음에 굉장히 헷갈렸던 부분이 `dstsize`이다. `dst`가 `dstsize`보다 큰 경우는 `dstsize` 크기를 벗어나기 때문에 문자열을 이어붙일 수가 없다.   
> 따라서 연결 작업은 건너 뛰고 `return(dstlen + strlen)` 해준다고 생각하면 된다.  
> 즉, `dstsize`는 10이지만 `dst`의 문자열의 길이가 4인 경우 src의 문자열은 6이 들어갈 수 있다.  
> 반면 `dstsize`가 10이지만 dst의 문자열의 길이가 11인 경우엔 CONCATENATE(연결과정)이 생략되고 `destsize의 길이와 + src길이값`이 return 된다.

<br/>

### 추가 설명
- 예시
```
dst의 길이가 5이고, src의 길이가 4라고 할 때,
dstsize의 길이가 4이면 dst 길이보다 작으므로 src 데이터가 들어가지 않는다.  
src 데이터를 넣으려면 dstsize를 "5 + 1(dst길이 + NULL)" 이상의 값으로 넣어야 한다.  

만약 dstsize를 8로 주면,
dst에는 src의 2바이트만 들어가게 된다.
```
<br/>

- 이 함수는 size가 strlen (dest) 보다 작은 경우를 제외 하고는 src 에서 문자열 size에 null 종료 문자열 src 를 추가하고  
src 에서 최대 `size-strlen (dest) -1` 을 복사 한 다음 결과에 `null` 종결자를 추가한다. 이 함수는 strcat ()의 버퍼 오버런 문제를 해결하지만  
크기가 너무 작으면 호출자가 데이터 손실 가능성을 처리해야한다. 이 함수는 strlcat ()이 생성하려고 시도한 문자열의 길이를 반환한다.  
반환 값이 크기 보다 크거나 같으면 데이터 손실이 발생하므로 호출자는 호출 전에 인수를 확인하거나 함수 반환 값을 테스트해야한다.  
strlcat ()은 glibc에는 존재하지 않고 POSIX에 의해 표준화되지도 않았지만, libbsd 라이브러리를 통해 리눅스에서 사용 가능하다.  

<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 3시간 
* 학습에 참고한 사이트 :
    * [strlcat1](https://whatdocumentary.tistory.com/45)
    * [strlcat2](https://minsoftk.tistory.com/32)
    * [strlcat3](https://hanul-dev.netlify.app/c/strlcat()-%EA%B5%AC%ED%98%84/)
    * [strncat](https://ehpub.co.kr/strncat-%ED%95%A8%EC%88%98/)

<br/>

## 8. 학습 내용에 대한 개인적인 총평
지난 시간 strcat 함수에 이어 strncat, strlcat을 공부해보았다. `strlcat` 의 반환값을 이해하는게 조금 힘들었는데, 특히 dstsize가 dst 보다 작을  때 `dstsize + strlen(src)`을 반환하는 부분이었다. 정확히 왜 이러한 값을 반환하는지에 대해서는 이해하지 못했는데, 암기를 해야할 것 같다. 이 부분에 유의해서 다음 시간에 strlcat 함수를 구현해 볼 예정이다. 라피신 때 분명 구현해 보았던 함수들인데도 다시 기본부터 공부하려니 시간이 많이 걸리는데, 아직 구현해야 할 함수는 많이 남아서 좀 서둘러야겠다. 오늘까지 레포트를 깃에 잘 업로드 해놓고, 다음주에는 Makefile을 만들어 컴파일하기 더 쉽게 프로그램을 만들어 봐야겠다.  
<br/>

## 9. 다음 학습 계획
- strchr, strrchr 함수를 공부한다.  
