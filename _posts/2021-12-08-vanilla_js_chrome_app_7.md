---
title: "바닐라 JS로 크롬 앱 만들기_7차시"
excerpt: "바닐라 JS로 크롬 앱 만들기"

categories:
    - javascript
tags:
    - [javascript]

toc: false
toc_sticky: false

date: 2021-12-08
last_modified_at: 2021-12-08
---

## Randomness 성격을 가진 배경, 명언 보여주기

### 명언 추가하기
명언을 담기 위해 object가 들어있는 array 생성
###### quotes.js
```js
const quotes = [
  {
    quote: "꿈을 이루고자 하는 용기만 있다면 모든 꿈을 이룰 수 있다.",
    author: "월트 디즈니",
  },
  {
    quote: "비록 해가 진다고 해도, 나에게는 전기불이 있다.",
    author: "커트 코베인",
  },
  {
    quote: "웃음이 없는 하루는 버린 하루다.",
    author: "찰리 채플린",
  },
  {
    quote: "우리는 한 번도 존재하지 않았던 것을 꿈꿀 수 있는 사람들이 필요하다.",
    author: "존 F. 케네디",
  },
  {
    quote: "변화는 우리가 누군가나 무엇, 혹은 후일을 기다린다고 찾아오지 않는다. 우리 자신이 우리가 기다리던 사람이고 우리가 바로 우리가 추구하는 변화이다.",
    author: "버락 오바마",
  },
  {
    quote: "무슨 일을 하기 전에는 그 일에 대해 기대를 가져야 한다.",
    author: "마이클 조던",
  },
  {
    quote: "조금도 도전하지 않으려고 하는 것이 인생에서 가장 위험한 일이다.",
    author: "오프라 윈프리",
  },
  {
    quote: "남들이 할 수 있거나 하려는 일을 하지 말고 남들이 할 수 없거나 하지 않으려는 일을 하라.",
    author: "아멜리아 에어하트",
  },
  {
    quote: "새로운 일에 도전하다 보면 가끔 실수를 저지를 수 있다. 자신의 실수를 빨리 인정하고 다른 시도에 집중하는 것이 최선이다.",
    author: "스티브 잡스",
  },
  {
    quote: "행동은 모든 성공의 가장 기초적인 핵심이다.",
    author: "파블로 피카소",
  }
];
```

###### index.html
```html
<body>
<form id="login-form" class="hidden">
    <input type="text" placeholder="What is your name?" required maxlength="15">
    <input type="submit" value="Log In">
</form>
    <h2 id="clock">00:00:00</h2>
    <h1 id="greeting" class="hidden"></h1>
    <div id="quote">
        <span></span>
        <span></span>
    </div>
    <script src="./js/greeting.js"></script>
    <script src="./js/clock.js"></script>
    <script src="./js/quotes.js"></script>
</body>
```
그리고 quotes.js를 html 파일에 import하고  
id가 quote인 div 태그 안에 명언과 이름을 넣어줄 span 태그 생성  

###### quotes.js 
```js
const quote = document.querySelector("#quote span:first-child"); //id가 quote인 element의 첫번째 span
const author = document.querySelector("#quote span:last-child");

const todaysQuote = quotes[Math.floor(Math.random() * quotes.length)];
quote.innerText = todaysQuote.quote;
author.innerText = todaysQuote.author;
```
html 파일에 명언과 이름을 넣어주기 위해 해당 태그를 `querySelector()`로 가져와준다.

###### Math.random()
명언을 랜덤으로 보여주기 위해서는 `Math.random()`을 사용한다.    
소수점을 가지는 float 타입이고, 0 <= n < 1 사이의 랜덤 값을 준다.    

배열의 길이를 알 수 있는 length를 활용해서 배열의 길이가 10일 때 Math.random()에 곱해주게되면  
0 <= n < 10 사이의 값을 가진다.  
즉, [0],[1],[2],....,[9]의 랜덤 값을 가질 수 있어 배열의 있는 모든 값이 랜덤 값에 포함된다.

###### Math.floor()
Math.random()에 배열의 길이를 곱한 값이 소수점이 나오기 때문에 배열의 인덱스 번호에 활용할 수 없다.  
그렇게 때문에 소수점을 없애주어야 하는데 그 역할을 하는 함수가 `Math.floor()`이다.  
Math.floor()는 소수점을 버리는 역할을 한다.(내림)

배열의 인덱스 번호를 랜덤으로 받고 해당 배열의 값을 todaysQuote 변수에 넣어준다.  
object가 들어있는 배열이기 때문에 .quote, .author로 접근해서  
해당 태그 안에 배열 값을 넣어준다.

### 배경 추가하기
###### index.html
```html
<body>
    <form id="login-form" class="hidden">
        <input type="text" placeholder="What is your name?" required maxlength="15">
        <input type="submit" value="Log In">
    </form>
    <h2 id="clock">00:00:00</h2>
    <h1 id="greeting" class="hidden"></h1>
    <div id="quote">
        <span></span>
        <span></span>
    </div>
    <script src="./js/greeting.js"></script>
    <script src="./js/clock.js"></script>
    <script src="./js/quotes.js"></script>
    <script src="./js/background.js"></script>
</body>
```
background.js를 html 파일에 import

###### background.js
```js
const images = [
    "0.jpg",
    "1.jpg",
    "2.jpg"
];
``` 
이미지 파일 이름 배열 생성

img 폴더 추가 - 배열에 있는 값과 이미지 파일 명이 동일해야 한다.
![imageArray](https://user-images.githubusercontent.com/89443479/145134357-782f0ba7-06bb-4824-aece-8ba79855ec7e.png)

<br>

###### background.js
```js
const chosenImage = images[Math.floor(Math.random() * images.length)];

// 자바스크립트에서 이미지를 만들고 html에 추가
// createElement() img element 추가
const bgImage = document.createElement("img"); //bgImage = <img>
bgImage.src = `img/${chosenImage}`; // bgImage = <img src="img/(n).jpg">
document.body.appendChild(bgImage); // bgImage를 body 내부에 추가 (+ appendChild는 가장 뒤에 prependChild는 가장 위에 오게 한다!)
```
위에서 했던 것과 마찬가지로 랜덤으로 배경을 보여주기 위해 Math.random()을 활용해준다.  

###### createElement()
HTML의 태그를 생성한다.

`createElement()`를 사용해 img 태그를 추가해준다.
src 속성에 이미지 경로와 합쳐서 배열의 랜덤 값(파일 명)을 주고  
`appendChild()`를 사용해 img 태그를 body 내부에 추가해준다.




