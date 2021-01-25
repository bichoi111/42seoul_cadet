## 목차
- [1. 학습 날짜](#1-학습-날짜)  
- [2. 학습 시간](#2-학습-시간)  
- [3. 학습 범위 및 주제](#3-학습-범위-및-주제)  
- [4. 동료 학습 방법](#4-동료-학습-방법)  
- [5. 학습 목표](#5-학습-목표)  
- [6. 상세 학습 내용](#6-상세-학습-내용)  
    - [Makefile 정리](#--makefile-정리)
        - [Makefile 예제](#makefile-예제)
        - [C++에서 Makefile 작성시](#c-에서-makefile-작성할-때)
        - [라이브러리 링크](#라이브러리와-링크가-필요한-makefile)
        - [ranlib](#ranlib)
    - [relink](#--relink)
    - [실제 학습 시간 및 참고 사이트](#--기타-세부-사항)
- [7. 학습 내용에 대한 개인적인 총평](#7-학습-내용에-대한-개인적인-총평)
- [8. 다음 학습 계획](#8-다음-학습-계획)  
<br/> 

## 1. 학습 날짜
* 2021-01-17(일)<br/><br/>
## 2. 학습 시간
* 12:30 ~ 17:00(자가)<br/><br/>
## 3. 학습 범위 및 주제
* 그동안 배운 Makefile을 정리한다.
* relink란 무엇인지 찾아본다.<br/><br/>
## 4. 동료 학습 방법
* 슬랙 general <br/><br/>
## 5. 학습 목표
* Makefile을 정리하고, relink에 대해 알아본다.<br/><br/>
## 6. 상세 학습 내용
## - Makefile 정리
### Makefile 예제
```
# 예제 1

.SUFFIXES : .c .o

CC = gcc

INC =                 # include 되는 헤더 파일의 패스를 추가한다.
LIBS =                # 링크할 때 필요한 라이브러리를 추가한다.
CFLAGS = -g $(INC)    # 컴파일에 필요한 각종 옵션을 추가한다.

OBJS =                # 목적 파일의 이름을 적는다.
SRCS =                # 소스 파일의 이름을 적는다.

TARGET =              # 링크 후에 생성될 실행 파일의 이름을 적는다.

all : $(TARGET)

$(TARGET) : $(OBJS)
    $(CC) -o $@ $(OBJS) $(LIBS)

dep :
    gccmakedep $(INC) $(SRCS)

clean :
    rm -rf $(OBJS) $(TARGET) core

new : 
    $(MAKE) clean 
    $(MAKE) 
```
```
$ make dep        <- 자동으로 의존관계 생성
$ make            <- make 동작
```
<br/>

- **.SUFFIXES : .c .o**
```
make 내부에서 정의된 확장자 규칙을 이용하기 위한 것이다.
make는 자동적으로 .c와 .o로 끝나는 파일들간에 정의된 규칙이 있는지 찾게 되고 적당한 규칙을 찾아 수행하게 된다.
```

<br/>

- **CFLAGS = -g $(INC)**
```
CFLAGS 매크로를 재정의하고 있다. '-g'는 디버그 정보를 추가하라는 것이고, $(INC)는 컴파일할 때 필요한 include 패스를 적어 두는 곳이다.
```
<br/>

- **all : $(TARGET)**
```
make 는 Makefile을 순차적으로 읽어서 가장 처음에 나오는 규칙을 수행하게 된다.
여기서 all이란 더미타겟(dummy target)이 바로 첫 번째 타겟으로써 작용하게 된다.
관습적으로 all이라는 타겟을 정의해 두는 것이 좋다. 결과 파일이 많을 때도 all의 의존 관계(dependency)로써 정의해 두면 꽤 편리하다.
```

<br/>

- **dep : gccmakedep $(INC) $(SRCS)
```
의존 관계를 자동적으로 생성해 주기 위한 것이다. 헤더 파일의 패스까지 추가되어야 한다는 것에 주의!
이것은 내부적으로 gcc가 작동하기 때문이다.
```
<br/>

---
<br/>

### C++에서 Makefile 작성할 때

- 위 예제를 C++ 파일에 이용하려면 .SUFFIXES, CC, CFLAGS 그리고 타겟에 대한 명령어를 아래와 같이 바꾸면 된다.  
```
.SUFFIXES : .cc .o

CXX = g++
CXXFLAGS = -g $(INC)

$(TARGET) : $(OBJS)
    $(CXX) -o $@ $(OBJS) $(LIBS)
```
물론 각자 취향에 따라 Makefile을 만들면 된다.  

<br/>

---
<br/>

### 라이브러리와 링크가 필요한 Makefile
라이브러리를 만들기 위한 Makefile은 위의 예제와 거의 유사하다. 다만 **TARGET이 실행파일이 아니고 라이브러리 라는 것!**  
그리고 라이브러리 만드는 방법을 알고 있어야 한다.
> 참고 : 라이브러리 만드는 방법을 소개하기로 한다.
> read.o write.o를 libio.a로 만들어 보자.
> 라이브러리를 만들기 위해서는 `ar` 유틸리티와 `ranlib` 유틸리티가 필요하다. (자세한 설명은 `man` 이용)

<br/>

- **정적 라이브러리**
```
$ ar rcv libio.a read.o write.o

a - read.o     <- 라이브러리에 추가(add)
a - write.o

$ ranlib libio.a     <- libio.a의 색인(index)을 생성
```
위의 과정을 Makefile로 일반화시켜 본다. '예제 1'에서 TARGET을 처리하는 부분만 아래와 같이 바꾼다.
```
TARGET = libio.a

$(TARGET) : $(OBJS)
    $(AR) rcv $@ $(OBJS)    # ar rcv libio.a read.o write.o
    ranlib $@               # ranlib libio.a
```
<br/>

- **동적 라이브러리**

`read.c write.c`를 컴파일해서 `libio.so.1`을 만들어 보자.  
(`so`는 shared object를 의미하고 `.1`은 `동적 라이브러리 버전`을 의미한다.)  
```
$ gcc -fPIC -c read.c    <- '-fPIC'을 추가해서 컴파일한다.
$ gcc -fPIC -c write.c

$ gcc -shared -WI,-soname,libio.so.1 -o libio.so.1 read.o write.o
```
위와 같이 하면 `libio.so.1` 가 생성된다. 사용자가 만든 동적 라이브러리를 사용하는 방법에 대해서는 간단히만 언급하기로 한다.  
우선 `libio.so.1` 을 `/usr/lib`로 옮겨서 `ldconfig -v` 해서 **라이브러리 설정을 갱신**해 주던지, 아니면 `LD_LIBRARY_PATH`를 지정해 두면 된다.  
아래는 `test.c`를 `libio.so.1`과 링크 시키는 예제이다.  
```
$ gcc -c test.c
$ gcc -o test test.o -L. -lio    <- 현재 디렉토리에 있다고 가정
```
<br/>

'예제 1'을 고쳐서 동적 라이브러리를 자동적으로 만들어 보자.
```
.SUFFIXES : .c .o 

CC = gcc

INC =
LIBS =
CFLAGS = -g $(INC) -fPIC    # -fPIC 추가

OBJS = read.o write.o
SRCS = read.c write.c

TARGET = libio.so.1         # libio.so.1이 최종 파일

all : $(TARGET)
                
$(TARGET) : $(OBJS)
    $(CC) -shared -WI,-soname,$@ -o $@ $(OBJS)

dep :
    gccmakedep $(INC) $(SRCS)

clean :
    rm -rf $(OBJS) $(TARGET) core
```
<br/>

---
<br/>

### ranlib
- 정적 라이브러리를 성공적으로 사용하기 위해서 일부 시스템, 특히 버클리(Berkeley) 유닉스로부터 파생된 시스템에서는 라이브러리 목차를 만들 필요가 있는데,  
이를 위해 `ranlib`을 사용한다. 리눅스와 같이 GNU 소프트웨어 개발 도구를 사용할 때에는 이러한 것이 필요하지 않으며 또한 실행한다 하더라도 문제가 없다.  
- 정적 라이브러리의 경우에는 단순히 object 파일을 합치는 것 이외에도 **symbol index**를 생성하는 작업이 필요하다.  
(라이브러리 내에 정의된 심볼들을 빨리 찾아서 **링크 속도를 높이기 위한 목적이다.**)
- 이는 `ar` 실행시 `s`옵션을 주거나 `ranlib` 프로그램을 실행하면 된다.  

> 그러니까 위의 예제에서 `ranlib` 안쓰고 그냥 간단하게 `ar s`옵션 주면 될 듯.  

<br/>

---
<br/>

## - relink
- 코드를 바꾸지 않은 상태에서 다시 make 했을 때, 변경사항이 없어서 타겟을 다시 만들지 않으면 리링크되지 않은 것.  
- 보통 의존성이 있는 경우 리링크가 많이 발생함.

> **relink 되어서는 안된다**는 의미는 `make all` 실행 직후 다시 `make all`을 했을 때, 
> 즉 **소스코드의 수정이 없을 때는 아무것도 컴파일 되지 않는다.**  
> **변경사항이 있을 때만 변경된 파일이 컴파일되어야 한다**는 의미.

<br/>

---
<br/>

## - 기타 세부 사항
* 실제 학습 시간 : 4시간 30분 
* 학습에 참고한 사이트 :
    * [makefile 정리](http://doc.kldp.org/KoreanDoc/html/GNU-Make/GNU-Make-7.html)
    * [ranlib](https://progh2.tistory.com/entry/%EC%A0%95%EC%A0%81-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC%EC%99%80-ar-ranlib-nm)<br/>
    * [ar s 옵션](https://mintnlatte.tistory.com/154)
    * [relink](https://stdbc.tistory.com/69)

<br/>

## 7. 학습 내용에 대한 개인적인 총평
오늘은 지금껏 공부했던 Makefile에 대해 간단히 정리해보았다. 동적 라이브러리 링크하는 부분은 이전 보고서로 돌아가서 다시 한 번 자세히 복습할 예정이다. 그리고 42seeoul Makefile규칙에 `makefile이 리링크(relink)를 할 경우, 프로젝트는 정상적으로 작동하지 않는 것으로 처리됩니다.`라는 항목이 있어서 relink의 개념에 대해 알아보았다. 내가 소스 파일을 변경하지 않았는데, make를 했을 때 오브젝트 파일 생성부터 링크과정이 반복되면 안되는 것이다. 앞으로 과제하면서 꼼꼼히 확인해보고, 만약에 리링크가 됐으면 원인이 뭔지 찾아봐야겠다.   
<br/>

## 8. 다음 학습 계획
- 라이브러리와 헤더파일의 차이점을 정리한다.
