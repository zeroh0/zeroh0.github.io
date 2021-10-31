---
title: "바닐라 JS로 크롬 앱 만들기_1차시"
excerpt: "바닐라 JS로 크롬 앱 만들기"

categories: 
    - javascript
tags:
    - [javascript]

toc: true
toc_sticky: true

date: 2021-11-01
last_modified_at: 2021-11-01
---

### 바닐라 JS로 크롬 앱 만들기



------ 



##### variable을 만드는 옵션

- const : 값 업데이트 불가능, 상수
- let : 값 업데이트 가능
- var : 규칙이 없다. const나 let처럼 구분할 수 없다. (사용하지 않는 것을 권장)



##### 데이터 타입

- boolean : true, false(참, 거짓)
- null : 비어있음, null은 자연적으로 발생하지 않는다.
- undefined : 변수에 값을 부여하지 않은 상태(값이 없음), 정의되지 않음, 메모리에 존재하지만 값은 주어지지 않음



##### array(배열)

- 하나의 변수 안에 많은 수의 데이터를 가지고 싶을 때

변수타입 변수명 = [ val1, val2, ...., val(n) ];

```js
const toBuy = ["totato", "potato", "pizza"];
```



- 모든 유효한 데이터 타입이나 변수가 들어갈 수 있다.

```js
const myInfo = ["jangdongho", 20, true, null, undefined];
```



- 배열에 있는 값에 접근

배열의 인덱스 번호는 0번부터

변수명[인덱스번호]

```js
toBuy[0]
```



- 배열에 있는 값 변경

```js
toBuy[2] = "water";
```



- 배열 끝에 값을 하나 더 추가

변수명.push(val);

```js
toBuy.push("one");
```



##### object

변수타입 변수명 = { attr1:val1, ... };

```js
//player라는 object의 property 작성
const player = {
    name:"zero",
    age:"20"
};
```



- 배열에서 한 개의 값을 가져올 수 있었던 것처럼 Object도 한 개의 값을 가져올 수 있다.

변수명.속성명

```js
console.log(player.name);
```



- Object의 속성의 값 변경

```js
player.name = "zeroh0";
console.log(player.name);
```



- 새로운 속성 추가

```js
player.sexy = "soon";
console.log(player);
```



##### function

function을 사용하는 이유는 여러 가지 일을 같은 코드로 처리하기 위해



- 매개변수가 없는 함수

선언 : function 함수명() { };

호출 : 함수명();

```js
function playerInfo() {
  console.log(player);
}

playerInfo();
```





- 매개변수가 있는 함수

선언 : function 함수명(arg1, ...., arg(n)) { };

호출 : 함수명(arg1, ...., arg(n));

```js
function plus(a, b) { //a, b는 function의 body에서만 사용 가능
    console.log(a + b);
}

plus(1, 2); // 3
plus(3, 4); // 7
plus(5, 6); // 11
```



- 배열 속성에 정의

```js
const player = {
    sayHello: function(otherPersonsName) {
        console.log("hello! " + otherPersonsName + " nice to meet you!");
    }
};
player.sayHello("jang"); //hello! jang nice to meet you!
```



------



###### 과제 : 계산기 만들기

```js
const calculator = {
    add: function(a, b) { // 더하기
        console.log(a + b);
    },
    sub: function(a, b) { // 빼기
        console.log(a - b);
    },
    div: function(a, b) { // 나누기
        console.log(a / b);
    },
    powerOf: function(a, b) { // 제곱
        console.log(a ** b);
    }
};

calculator.add(2, 3); // 5
calculator.sub(3, 3); // 0
calculator.div(3, 3); // 1
calculator.powerOf(3, 3); // 27
```
