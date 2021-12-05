---
title: "바닐라 JS로 크롬 앱 만들기_5차시"
excerpt: "바닐라 JS로 크롬 앱 만들기"

categories:
    - javascript
tags:
    - [javascript]

toc: false
toc_sticky: false

date: 2021-12-01
last_modified_at: 2021-12-05
---

###### 이름을 입력받고 버튼을 클릭하면 입력받은 이름이 보여지는 이벤트

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Momentum</title>
    <link rel="stylesheet" href="style.css"/>
</head>
<body>
    <div id="login-form">
        <input type="text" placeholder="What is your name?">
        <button>Log In</button>
    </div>
    <script src="app.js"></script>
</body>
</html>
```

```js 
const loginInput = document.querySelector("#login-form input");
const loginButton = document.querySelector("#login-form button");

function onLoginBtnClick() {
    console.log("hello", loginInput.value);
}

loginButton.addEventListener("click", onLoginBtnClick);
```
이름을 입력하고 버튼을 클릭하게 되면 hello라는 문장과 함께 입력된 문장이 콘솔에 출력된다.  
하지만 아무것도 입력하지 않았을 때 버튼을 클릭했을 때 빈 값이 콘솔에 출력되기 때문에 유효성 검사를 해준다.  

###### username 유효성 검사  
- 이름이 비어있으면 안된다.
- 이름이 너무 길면 안된다.

```js
function onLoginBtnClick() {
    const username = loginInput.value; // 입력받은 input 값
    if(username === "") { // 입력 값이 비어있을 경우
        alert("Please write your name");
    } else if(username.length > 15) { // 입력 값의 길이가 15보다 클 때
        alert("your name is too long");
    }
}
```

자바스크립트에서 하는 방식은 선호되지 않음.  
html input에 required과 maxlength를 활용할 수 있기 때문    
```html
<form id="login-form">
    <input type="text" placeholder="What is your name?" required maxlength="15">
    <button>Log In</button>
</form>
```
*하지만 input의 유효성 검사는 input이 form 태그 안에 있을 때만 작동한다!*



## submit을 할 때 브라우저의 새로고침을 막는 방법

EventListener를 추가 할 때  
()를 추가하는 건 function을 '즉시' 실행한다는 뜻  
submit event가 발생하면 브라우저가 알아서 function을 실행  

onLoginSubmit();  
브라우저는 우선 onLoginSubmit function을 호출하고 브라우저가 function을 실행시키고 있기는 하지만   
()여기 이 안에서 코드를 입력하고 있는 나에게 정보를 주고 있다.  
onLoginSubmit(info);  
브라우저는 브라우저 내에서 event로부터 정보를 잡아내서   
어떠한 정보를 가지고 있는 채로 onLoginSubmit function 실행 버튼을 누르고 있다.

```js
const loginForm = document.querySelector("#login-form");
const loginInput = document.querySelector("#login-form input");

function onLoginSubmit(event) {
    event.preventDefault();
    const username = loginInput.value;
}

loginForm.addEventListener("submit", onLoginSubmit); // submit은 엔터를 누르거나 버튼을 클릭할 때 발생
```

EventListener를 추가 할 때  
()를 추가하는 건 function을 '즉시' 실행한다는 뜻  
submit event가 발생하면 브라우저가 알아서 function을 실행

onLoginSubmit();  
브라우저는 우선 onLoginSubmit function을 호출하고 브라우저가 function을 실행시키고 있기는 하지만  
()여기 이 안에서 코드를 입력하고 있는 나에게 정보를 주고 있다.  
onLoginSubmit(info);  
브라우저는 브라우저 내에서 event로부터 정보를 잡아내서  
어떠한 정보를 가지고 있는 채로 onLoginSubmit function 실행 버튼을 누르고 있다.  

event가 발생할 때 브라우저가 function을 호출하게 되는데  
첫 번째 argument로써 추가적인 정보를 가진 채로 호출하게 된다.  
어떤 정보를 브라우저가 주고 있는지 보여주기 위해서

새로고침이 일어나는 건 <b>form의 기본 동작 submit</b> - 브라우저는 엔터를 누를 때 새로고침을 하고 form을 submit 하도록 되어있다.

###### 기본 동작(default) 발생하지 않도록 하는 방법
- <b>preventDefault()</b>

발생한 일에 대한 정보 argument  
모든 EventListener function의 첫번째 argument는 발생한 일에 대한 정보  
<b>어떤 event의 기본 동작을 발생되지 않도록 막는 역할</b>  
<b>기본 동작: 어떤 function에 대해 브라우저가 기본적으로 수행하는 동작</b>  

<br>

```html
<form id="login-form">
    <input type="text" placeholder="What is your name?" required maxlength="15">
    <input type="submit" value="Log In">
</form>
<a href="http://nomadcoders.co">Go to courses</a>
```

```js
const link = document.querySelector("a");

function handleLinkClick(event) {
    event.preventDefault(); // 기본 동작 제거
    console.dir(event); // event의 구조 확인
    alert("clicked"); // 모든 동작들을 막는다.. 그래서 잘 안쓴다고 한다.
}

link.addEventListener("click", handleLinkClick); 
```
<b>링크의 기본 동작은 클릭 시 다른 페이지로 이동</b>  
addEventListener({information about the event that just happened})  



## 사용자가 이름을 제출하면 폼을 없애는 방법

1. html 요소 자체를 없애기 
2. &gt; CSS를 활용  

```html
<form id="login-form">
    <input type="text" placeholder="What is your name?" required maxlength="15">
    <input type="submit" value="Log In">
</form>
<h1 id="greeting" class="hidden"></h1>
```

```css
.hidden {
    display: none;
}
```

```js
const loginForm = document.querySelector("#login-form");
const loginInput = document.querySelector("#login-form input");
const greeting = document.querySelector("#greeting");

/* 대문자 사용 : 일반적으로 string만 포함된 변수는 대문자로 표기하고 string을 저장하고 싶을 때 사용 */
const HIDDEN_CLASSNAME = "hidden"; 

function onLoginSubmit(event) {
    event.preventDefault(); // submit 기본 동작 제거
    const username = loginInput.value; // 입력 받은 username 값을 username에 할당
    loginForm.classList.add(HIDDEN_CLASSNAME); // form 태그에 hidden 클래스 추가 
    // greeting.innerText = "Hello " + username;
    greeting.innerText = `Hello ${username}`; // id가 greeting인 요소의 텍스트를 Hello ${입력값} 넣는다.
    greeting.classList.remove(HIDDEN_CLASSNAME); // 기존에 존재하던 hidden 클래스를 제거해준다.
}

loginForm.addEventListener("submit", onLoginSubmit);
```
greeting.innerText = `Hello ${username}`;
1. 변수와 string을 결합하고 싶다면 또는 변수를 string안에 집어넣고 싶다면 ${변수명}으로 표현
2. ` 기호로 시작 (백틱)

### localStorage
브라우저에 무언가를 저장해 줄 수 있다.  
key와 value 형태로 되어있다.  

- setItem(저장할 값의 이름, 저장할 값): localStorage에 정보를 저장한다.
- getItem(불러올 값의 이름): 저장한 값을 불러온다.
- removeItem(삭제할 값의 이름): 저장한 값을 삭제한다.

```js
const loginForm = document.querySelector("#login-form");
const loginInput = document.querySelector("#login-form input");
const greeting = document.querySelector("#greeting");

const HIDDEN_CLASSNAME = "hidden";

function onLoginSubmit(event) {
    event.preventDefault();
    const username = loginInput.value;
    localStorage.setItem("username", username); // 입력된 username을 username이라는 key 값으로 저장
    loginForm.classList.add(HIDDEN_CLASSNAME);
    greeting.innerText = `Hello ${username}`;
    greeting.classList.remove(HIDDEN_CLASSNAME);
}

loginForm.addEventListener("submit", onLoginSubmit);
```

## 입력 값을 제출했는데 새로고침을 하면 form이 안보여지도록 하는 방법

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Momentum</title>
    <link rel="stylesheet" href="style.css"/>
</head>
<body>
    <form id="login-form" class="hidden">
        <input type="text" placeholder="What is your name?" required maxlength="15">
        <input type="submit" value="Log In">
    </form>
    <h1 id="greeting" class="hidden"></h1>
    <script src="app.js"></script>
</body>
</html>
```
form과 h1에게 hidden 클래스를 지정해준다.  

local storage에 저장되어 있는 값이 없으면 form을 보여주고,  
local storage에 값이 없으면 form을 보여주지 않고 입력된 이름이 보이도록 하기 위해 

```js
const loginForm = document.querySelector("#login-form");
const loginInput = document.querySelector("#login-form input");
const greeting = document.querySelector("#greeting");

/*
    string을 반복해서 사용하는 경우에 변수를 생성해 고정시켜 주는 것이 좋다.
    string은 오타가 날 경우 찾기가 힘들지만 고정된 변수명이 오타가 날 경우 에러로 확인할 수 있기 때문이다.    
 */
const HIDDEN_CLASSNAME = "hidden";
const USERNAME_KEY = "username";

function onLoginSubmit(event) {
    event.preventDefault(); // 기본 동작 제거
    loginForm.classList.add(HIDDEN_CLASSNAME); // form 태그에 hidden 클래스를 추가해줘 안보여지도록 한다.
    const username = loginInput.value; // 입력 값을 username 변수에 저장
    localStorage.setItem(USERNAME_KEY, username); // local storage에 username key값으로 입력된 값을 저장
    paintGreetings(username); // paintGreetings 라는 함수에 입력한 값을 가지고 실행
}

function paintGreetings(username) { 
    greeting.innerText = `Hello ${username}`; // h1의 text에 Hello + 입력한 값 or 저장된 값을 설정해준다.
    greeting.classList.remove(HIDDEN_CLASSNAME); // h1 태그에 hidden 클래스를 제거해서 h1 태그를 보여준다.
}

const savedUsername = localStorage.getItem(USERNAME_KEY); // local storage에 username이라는 키의 값을 savedUsername 변수에 넣는다.

if(savedUsername === null) { // 저장된 값이 없을 때   
    loginForm.classList.remove(HIDDEN_CLASSNAME); // form의 hidden 클래스를 제거해줘서 보여지도록 한다. 
    loginForm.addEventListener("submit", onLoginSubmit); // form의 submit event를 listen
} else { // 저장된 값이 있을 때
    paintGreetings(savedUsername); // paintGreetings 라는 함수에 입력되어 저장된 값을 가지고 실행
}
```