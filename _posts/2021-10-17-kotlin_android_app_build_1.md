---
title: "코틀린을 활용한 안드로이드 앱 개발_1차시"
excerpt: "코틀린을 배워보자"

categories:
    - kotlin
tags:
    - [kotlin]

toc: true
toc_sticky: true

date: 2021-10-17
last_modified_at: 2021-11-10
---

## 코틀린 파일의 구성

패키지, 임포트, 클래스, 변수, 함수 선언과 주석이 파일에 포함

클래스를 사용하지 않고 **변수**와 **함수**로만 구성 가능

<br>

패키지: 관련클래스들을 묶기 위한 물리적인 개념

- 이용하려는 클래스가 다른 패키지에 있다면 import 구문

- 코틀린 파일이 있는 폴더와 다른 패키지명을 사용할 수 있음

- 클래스로 묶지 않는 변수와 함수를 최상위 레벨로 관리

- 패키지 내에 선언된 전역변수나 전연함수처럼 취급

<br>



## Variable의 이해

변수 선언

- val(혹은 var) 변수명 타입 = 값
- val(value)은     Assign-once 변수, var(variable)은 Mutable 변수
- 타입 추론 지원

```kotlin
val data1 : int = 10
val data2 = 20
var data3 = 30
//초기값을 준 다음 변경 가능
Fun main(agrs:Array<String>) {
    // data2 = 40
    data3 = 40
}
```

<br>

변수 초기화

- 변수 선언은 최상위(클래스 외부), 클래스 내부, 함수 내부에 선언
- 최상위 레벨이나 클래스의 멤버 변수는 선언과 동시에 초기화해주어야 함
- 함수 내부의 지역 변수는 선언과 동시에 초기화하지 않아도 됨 -> 초기화한 후에 사용 가능

<br>

Null이 될 수 있는 변수와 null

- 코틀린에서는 null을 대입할 수 없는 변수와 있는 변수로 구분
- 변수에 null 값을 대입하려면 타입에 "?" 기호를 사용하여 명시적으로 null이 될 수 있는 변수로 선언

<br>

상수변수 선언

- 코틀린에서 변수 -> 프로퍼티(property) *변수를 외부에서 이용할때 사용되는 setter getter가 내장된 변수*
- val로 선언한 변수의 초기값을 변경할 수 없음 -> 일반적인 상수변수와 차이가 있음
- const라는 예약어를 이용해 상수변수를 만듬
- 최상위 레벨로 선언할 때만 const 예약어 사용 가능

<br>



## Data Type

코틀린 타입

- Int,     Double, Float, Long, Short, Byte, Char, Boolean, String, Any, Unit,     Nothing 타입을 제공
- Int,     Double 등은 클래스이며 이 클래스로 타입을 명시하여 선언한 변수는 그 자체로 객체

<br>

숫자 타입

- 코틀린에서 숫자 타입의 클래스들은 모두 Number 타입의 서브 클래스

- 문자(Characters)는 Number Type이 아님

- Number Type에 대한 자동 형 변형(implicit conversions for number)을 제공하지 않음

  *Int -> Double (x) 객체와 객체간의 형 변환 -> 상하위 관계가 존재하지 않음*

 <br>

논리, 문자와 문자열 타입

- Boolean(true,     false)



- 문자열은 escaped string과 raw string으로 구분 *"", """ : """는 이스케이프 문자 그대로 출력*
- string template 지원 *printf*

 <br>

Any 타입

- 코틀린 클래스의 최상위 클래스 Any

 <br>

null 허용 타입

- Any, Any? 타입의 상하위 관계

 <br>

Unit과 Nothing

- Unit : 주로 함수의 리턴 구문이 없을 때
- Nothing : 의미있는 결과값이 아닐 때 ex) 예외

<br>



## Array, List, Map

배열

- Array로 표현되며 Array는 get, set, size 등의 함수를 포함하는 클래스
- arrayOf 함수를 이용해 선언 *any타입의 데이터*
- Array 클래스로 선언

 <br>

List, Set, Map

- Collection 타입의 클래스들은 mutable 클래스와 immutable 클래스 구분 *추가 가능, 불가능 구분*

- kotlin.collection. List 인터페이스로 표현되는 객체는 immutable이며 size(), get() 함수만 제공
- kotlin.collection. MutableList 인터페이스로 표현되는 객체는 mutable이며 size(), get() 함수 이외에 add(), set() 함수 제공




