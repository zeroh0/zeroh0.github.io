---
title: "바닐라 JS로 크롬 앱 만들기_6차시"
excerpt: "바닐라 JS로 크롬 앱 만들기"

categories:
    - javascript
tags:
    - [javascript]

toc: false
toc_sticky: false

date: 2021-12-07
last_modified_at: 2021-12-07
---

### Date
Date object는 호출하는 당시의 현재 날짜랑 시간을 알려준다.  
- getDate(): 일
- getDay(): 요일(0 ~ 6)
- getFullYear(): 년도
- getHours(): 시  
- getMinutes(): 분
- getSeconds(): 초

### setInterval
<b>setInterval(function, 호출 간격(ms))</b>
설정한 시간 간격마다 function을 호출시킨다.

### setTimeout
<b>setTimeout(function, 지연(ms))</b>
설정한 시간 뒤에 function을 호출시킨다.

*매 초 마다 시간을 나타내기 위해서는 setInterval 사용*

```js
const clock = document.querySelector("h2#clock");

function getClock() {
    const date = new Date();
    clock.innerText = (`${date.getHours()} : ${date.getMinutes()} : ${date.getSeconds()}`);
}

setInterval(getClock, 1000); // 1초 마다 현재 시간을 보여준다.
```

![](/Users/zeroh0/Desktop/screenshots/스크린샷 2021-12-07 오전 2.17.09.png "clock")

시간을 자체 보여주는 것에는 문제가 없지만 시간을 보여지는 방식을 00:00:00으로 변경하려고 한다. 

### padStart
padStart()는 가지고 있는 string을 보다 길게 만들어야 할 때 사용한다.  
원하는 만큼의 길이가 아니라면 string의 앞 쪽에 문자를 넣는다.  
string에만 쓸 수 있는 function이다.  


> "1".padStart(2, "0");

string의 길이는 2, 만약 string의 길이가 2보다 짧다면 앞 쪽에 "0" 추가

<br>

```js
"hello".padStart(20, "x");
```
```
'xxxxxxxxxxxxxxxhello'
```

<br>

date.getHours()에는 padStart를 사용할 수 없다. 왜냐하면 타입이 number이기 때문에  
그렇기 때문에 형변환을 해주어야 한다.  
string 타입으로 형변환을 하려면 String constructor로 감싸주면 된다.
```js
new Date().getHours();
String(new Date().getHours());
```
```
1
'1'
```

<br>

###### 정리

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Momentum</title>
    <link rel="stylesheet" href="./css/style.css"/>
</head>
<body>
    <form id="login-form" class="hidden">
        <input type="text" placeholder="What is your name?" required maxlength="15">
        <input type="submit" value="Log In">
    </form>
    <h2 id="clock">00:00:00</h2>
    <h1 id="greeting" class="hidden"></h1>
    <script src="./js/greeting.js"></script>
    <script src="./js/clock.js"></script>
</body>
</html>
```

```js
const clock = document.querySelector("h2#clock");

function getClock() {
    const date = new Date();
    const hours = String(date.getHours()).padStart(2, "0");
    const minutes = String(date.getMinutes()).padStart(2, "0");
    const seconds = String(date.getSeconds()).padStart(2, "0");
    clock.innerText = (`${hours} : ${minutes} : ${seconds}`);
}

getClock(); //즉시 호출해서 현재 시간을 보여주고
setInterval(getClock, 1000); // 1초 마다 현재 시간을 보여준다. 실시간처럼 보이게 한다.
```
