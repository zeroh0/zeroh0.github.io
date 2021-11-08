---
title: "바닐라 JS로 크롬 앱 만들기_2차시"
excerpt: "바닐라 JS로 크롬 앱 만들기"

categories: 
    - javascript
tags:
    - [javascript]

toc: true
toc_sticky: true

date: 2021-11-08
last_modified_at: 2021-11-08
---

# 바닐라 JS로 크롬 앱 만들기



------ 



```js
const age = 96;

function calculatorKrAge(ageOfForeigner) {
    return ageOfForeigner + 2;
}

const krAge = calculatorKrAge(age);

console.log(krAge);
```

calculatorKrAge를 실행시키면 96이라는 argument가 age 자리로 대체

96이 age 자리로 가고 96 + 2를 return해서 krAge에 대입

###### 함수 결과를 받고 싶을 때는 return이라는 키워드 사용

<br>

```js
const calculator = {
    plus: function(a, b) {
        return a + b;
    },
    minus: function(a, b) {
        return a - b;
    },
    times: function(a, b) {
        return a * b;
    },
    divide: function(a, b) {
        return a / b;
    },
};

const plusResult = calculator.plus(2, 3);
const minusResult = calculator.minus(plusResult, 10);
const timesResult = calculator.times(10, minusResult);
const divideResult = calculator.divide(timesResult, plusResult);
const powerResult = calculator.power(divideResult, minusResult);
```

###### return을 사용해 값을 대체할 수 있다!

<br>

```js
function plus(a, b) {
    console.log(a + b);
}
```

망고 주스를 마시고 싶은데 주스 기계에서 망고 주스가 안나오는 상황이다..!!

그래서 return을 사용해서 망고 주스를 return 해주면된다

```js
function plus(a, b) {
    return a + b;
}
```

<br>

###### 한 번 return을 하면, function은 작동을 멈추고 결과 값을 return하고 끝내버린다!

```js
function plus(a, b) {
    console.log("hello");
    return a + b;
    console.log("bye bye"); // 절대 작동하지 않는다!
}
```

return하는 순간 function이 종료되기 때문에 bye bye는 출력되지 않는다.

<br>

## Conditional (조건문)

