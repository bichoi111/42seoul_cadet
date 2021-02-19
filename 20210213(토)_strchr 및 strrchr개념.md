## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [strchr](#strchr)
    - [strrchr](#strrchr)
    - [사용 예제](#사용-예제)
- [7. 실제 학습 시간 및 참고 사이트](#7-기타-세부-사항)
- [8. 학습 내용에 대한 개인적인 총평](#8-학습-내용에-대한-개인적인-총평)
- [9. 다음 학습 계획](#9-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-02-13(토)<br/><br/>
## 2. 학습 시간
* 15:00 ~ 17:00 (자가)<br/><br/>
## 3. 학습 범위 및 주제
* strchr 함수
* strrchr 함수<br/><br/>
## 4. 동료 학습 방법
* 해당 사항 없음<br/><br/>
## 5. 학습 목표
* strchr, strrchr 함수의 개념을 간단히 정리한다.<br/><br/>
## 6. 상세 학습 내용
## strchr
### 함수의 기능
- s가 가리키는 문자열에서 c에 해당하는 문자의 **첫번째** 발생 위치를 찾는다.
- 종료 문자인 `NULL` 문자는 문자열의 일부로 간주한다.  
따라서 c에 해당하는 문자가 `\0`인 경우 함수는 종료문자인 `\0`을 찾는다.  
- 문자열의 **앞에서 뒤로** 일지되는 문자가 있는지 검색한다.  

<br/>

### 함수의 원형
```
char    *strchr(const char *s, int c)
```
<br/>

### 헤더파일
```
C언어 : <string.h>
C++ : <cstring>
```
<br/>

### 매개변수
- const char \*s : 탐색대상 문자열
- int c : 탐색 대상 문자

<br/>

### 반환값
- 문자를 가리키는 포인터를 리턴한다.
- 문자열에 대상 문자가 없는 경우 `NULL`을 반환한다.

<br/><br/>

---
<br/><br/>

## strrchr
### 함수의 기능
- 문자열의 **뒤에서 앞으로** 일치되는 문자가 있는지 검색한다.
- 마지막 문자부터 찾는 것을 제외하면 `strchr` 함수와 동일하다.
- 사이의 `r`은 `reverse`이다.  

<br/>

### 함수의 원형
```
char    *strrchr(const char *s, int c)
```
<br/>

### 헤더파일
```
C언어 : <string.h>
C++ : <cstring>
```
<br/>

### 매개변수
- const char \*s : 탐색대상 문자열. NULL로 종결되는 문자열의 포인터
- int c : 탐색 대상 문자 또는 ASCII 값

<br/>

### 반환값
- 문자를 가리키는 포인터를 리턴한다.
- 문자열에 대상 문자가 없는 경우 `NULL`을 반환한다.

<br/><br/>

---
<br/><br/>

## 사용 예제
- 예제 1
```
char s[] = "kamill";
printf("return (amill) : %s\n", strchr(s,'a'));	// return (amill) : amill

char s[] = "apple";
printf("return (ple) : %s\n", strrchr(s,'p')); // return (ple) : ple
```
`strchr`은 앞에서부터 찾는 것이고,'k' 다음에 'a'가 첫번째 등장하는 'a'이다.    
이 포인터를 반환하므로 문자열로 출력해보면 'a'부터 출력이 되는 것이다.  

`strrchr`은 뒤에서부터 찾아서 'l'앞의 'p'가 가장 먼저 등장하는 'p'이다.  
이 `strrchr`은 이 'p'의 주소값을 반환하며, 문자열 출력시 이 'p'부터 끝까지 출력된다.  

<br/><br/>

- 예제 2
```c
#include <string.h>
#include <stdio.h>

int main()
{
	char dest[] = "abcdefgabcd";
    char *first;
    char *last;

	/* dest 문자열에서 첫번째 나오는 'b'문자를 찾습니다. */
	first = strchr(dest, 'b');

	/* dest 문자열에서 마지막 나오는 'b'문자를 찾습니다. */
	last = strrchr(dest, 'b');
	
	printf("first : %s \n", first);
	printf("last : %s", last);
    
	return 0;
}
```
```
// 결과

first : bcdefgabcd
last : bcd
```
strchr() 함수 결과는 처음 검색 문자를 찾은 부분부터 마지막 문자열까지 출력하고 있다.  
strrchr() 함수 결과는 검색 문자를 마지막으로 찾은 부분부터 마지막 문자열까지 출력하고 있다.  
<br/><br/>

## 7. 기타 세부 사항
* 실제 학습 시간 : 2시간 
* 학습에 참고한 사이트 :
    * [strchr, strrchr 간단 설명](https://velog.io/@mjung/function-strchr-strrchr)  
    * [예제](https://codingcollection.tistory.com/11)

<br/>

## 8. 학습 내용에 대한 개인적인 총평
strchr 함수와 strrchr 함수의 개념을 정리하다 보니 금방 구현할 수 있을 것 같다. strchr 함수와 strrchr 함수의 차이는 **문자열을 검색하는 방향**이 다르다는 것이다. strchr은 앞에서부터 검색하면 되고 strrchr은 뒤에서부터 검색하는 것을 기억해야겠다. 어떻게 구현해야 할지 생각해 봤는데 `strchr`의 경우 포인터를 앞에서부터 하나하나 읽어보고, 그 주소 값을 c와 비교하는 방법으로 코딩을 하면 될 것 같다. `strrchr`은 뒤에서부터 검색을 해야해서 포인터 위치를 맨 뒤로(NULL 문자 있는 곳) 옮겨야 하는데, 이것은 널문자를 찾을 때까지 포인터를 먼저 증가시킨 이후에 검색을 시작하면 될 것 같다. 다음 시간에 이를 구현해 보겠다.  
<br/>

## 9. 다음 학습 계획
- strlcat 함수 구현
- strchr 및 strrchr 함수 구현
