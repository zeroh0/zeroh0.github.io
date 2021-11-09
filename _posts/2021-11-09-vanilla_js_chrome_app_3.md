---
title: "바닐라 JS로 크롬 앱 만들기_3차시"
excerpt: "바닐라 JS로 크롬 앱 만들기"

categories: 
    - javascript
tags:
    - [javascript]

toc: true
toc_sticky: true

date: 2021-11-09
last_modified_at: 2021-11-09
---

# 바닐라 JS로 크롬 앱 만들기



---



promt는 사용자에게 창을 띄울 수 있게 해주고 2개의 argument를 받는다,

*function prompt(message?: string, _default?: string): string*

```js
const age = prompt("How old are you?");
console.log(age);
```

<br>

typeof는 value의 type을 확인할 수 있다.

```js
const age = prompt("How old are you?");
console.log(typeof age); // string
```

<br>

숫자형으로 type 변경

*function parseInt(string: string, radix?: number): number*

```js
console.log(typeof "15", typeof parseInt("15")); // string, number
```

<br>

```js
const age = parseInt(prompt("How old art you?"));
console.log(age, parseInt(age));
```

숫자가 아닌 값을 입력하면 Nan(Not a Number)

<br>

무언가가 Nan인지를 판별하는 방법

*function isNaN(number: number): boolean*

```js
const age = parseInt(prompt("How old art you?"));
console.log(isNaN(age)); // false
```

isNan은 boolean을 return한다.

값이 Nan일 때 true, Nan이 아닐 때 false

<br>



## Conditional(조건문)

<br>

```js
if (condition) {
    // condition === true
} else {
    // condition === false
}
```

<br>

```js
const age = parseInt(prompt("How old art you?"));

if (isNaN(age)) {
    console.log("Please writer a number"); // true일 때 실행 
} else {
    console.log("Thank you for writing your age"); // fales일 때 실행 
}
```

<br>

```js
const age = parseInt(prompt("How old art you?"));

if (isNaN(age)) {
    console.log("Please writer a number");
} else if (age < 18){
    console.log("You art too young");
} else {
    console.log("You can drink");
}
```

<br>

```js
const age = parseInt(prompt("How old art you?"));

if (isNaN(age) || age < 0) {
    console.log("Please writer a real positive number");
} else if (age < 18){
    console.log("You art too young");
} else if (age >= 18 && age <= 50) {
    console.log("You can drink");
} else if (age > 50 && age <= 80) {
    console.log("You should exrcise");
} else if (age > 80) {
    console.log("You can do whatever you want");
}
```

AND : &&는 둘 다 true일 때 true!

OR : ||는 둘 중 하나가 true일 때 true!

<br>

