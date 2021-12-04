---
title: "바닐라 JS로 크롬 앱 만들기_4차시"
excerpt: "바닐라 JS로 크롬 앱 만들기"

categories:
    - javascript
tags:
    - [javascript]

toc: false
toc_sticky: false

date: 2021-11-25
last_modified_at: 2021-12-04
---

  

  
## document에서 항목들을 가지고와 가져온 항목들을 변경할 수 있다.

```js
const title = document.getElementById("title");
title.innerText = "Got you!";
```

<br>

# 자바스크립트에서 HTML 요소들을 가져올 수 있는 방법

## getElementBy...(Id, ClassName, TagName, Name)

```js
const title = document.getElementById("hello");
```
'#'을 적어줄 필요가 없다.   
-> getElementById()가 id를 찾고 있다는 걸 알고 있기 때문에

<br>

```js
const hellos = document.getElementsByClassName("hello");
console.log(hellos); //array
```
getElementByClassName, getElementByTagName, getElementByName 등으로  
콘솔 값을 출력해보면 배열로 출력되는 걸 확인할 수 있다.

<br>

## querySelector, querySelectorAll
  
```js
const title = document.querySelector("#hello");
```
querySeletor는 #을 명시해주어야 한다.  
-> querySeletor()는 CSS selector 자체를 전달하기 때문이다.

<br>

```html
<div class="hello">
  <h1>Grab me1!</h1>
</div>
<div class="hello">
  <h1>Grab me2!</h1>
</div>
<div class="hello">
  <h1>Grab me3!</h1>
</div>
```

```js
const title = document.querySelector(".hello h1");
```

[comment]: <> (이미지 삽입)

querySelector는 element를 css 방식으로 검색 할 수 있다.  
해당하는 태그의 첫번째 요소의 값만 나온다.

<br>

```html
<div class="hello">
  <h1>Grab me1!</h1>
</div>
<div class="hello">
  <h1>Grab me2!</h1>
</div>
<div class="hello">
  <h1>Grab me3!</h1>
</div>
```

```js
const title = document.querySelectorAll(".hello h1");
title.innerText = "hi!";
```

[comment]: <> (이미지 삽입)

selector 안의 조건에 부합하는 모든 element를 가져다 준다.

<br>

```js
const title = document.querySelector(".hello h2");
console.log(title);
```
```
null
```
해당 요소가 존재하지 않으면 null 값이 나온다. 

<br>

```js
const title = document.querySelector(".hello h1");
console.dir(title)
```

[comment]: <> (이미지 삽입)

element의 내부를 보고 싶으면 console.dir()  
기본적으로 object로 표시한 element를 보여준다

<br>

# Event

on... <- event라고 한다.  
이 항목들이 전부 javascript object  
javascript object 내부에 있는 property들의 값들을 변경할 수 있다.  
특정 property들은 변경 불가(namespaceURI,...)  

style -> element의 style을 볼 수 있다.(javascript 형식으로)

[comment]: <> (이미지 삽입)

```js
const title = document.querySelector("div.hello:first-child h1");
title.style.color = "blue";
```
요소들을 확인해서 자바스크립트에서 style을 변경해줄 수 있다.

<br>

## addEventListener
addEventListener는 말 그대로 event를 listen  
javascript에 무슨 event를 listen하고 싶은 지 알려주어야 함  
왜냐하면 모든 event에 대해서 알고싶은 것이 아니라  
단 하나의 event만 알아보고 싶기 때문  

<br>

###### click

```js
// 누군가가 title을 click했을 때 무언가를 해주기 위해 function 추가
function handleTitleClick() {
    console.log("title was clicked");
}

title.addEventListener("click", handleTitleClick); // click event에 대해서 listen
```
addEventListener 두번째 인수로 handleTitleClick 전달  
function에 ()가 들어가지 않는다!! -> 실행버튼을 누르길 원하지 않음

##### 웹사이트에서 click event를 감지하는 방법

querySeletor()를 통해 element를 찾아서 title에 할당
addEventListen()를 추가 -> click event를 listen하고, click event가 발생하면,  
handleTitleClick이라는 function 동작  

function을 바로 실행하지 않기 위해 funciton을 javascript에 넘겨주고  
user가 title을 click할 경우에 javascript가 실행버튼을 대신 눌러주도록 하는 역할을 한다.

<br>

listen하고 싶은 event를 찾는 가장 좋은 방법은  
구글에 찾고 싶은 element의 이름, 예들 들어 h1 element를  
'h1 html element mdn(Mozilla Develop Network)' 검색  
Web APIs라는 문장이 포함된 페이지  

아니면 console.dir(title); 을 통해서  
property 앞에 on이 붙어있다면 그게 바로 event listener  
ex) onabort를 사용할 때에는 onabort 대신 abort로 사용해야 함  

<br>

###### mouseenter, mouseleave
```js
function handleMouseEnter() {
    // 마우스가 title 위에 올라갈 때 title의 innerText를 "Mouse is here!"로 보여준다.
    title.innerText = "Mouse is here!"; 
}

function handleMouseLeave() {
  // 마우스가 title 영역에서 벗어날 때 title의 innerText를 "Mouse is gone!"로 보여준다.
    title.innerText = "Mouse is gone!";
}

title.addEventListener("mouseenter", handleMouseEnter);
title.addEventListener("mouseleave", handleMouseLeave);
```

<br>

###### resize
```js
function handleWindowResize() {
  document.body.style.backgroundColor = "tomato";
}

window.addEventListener("resize", handleWindowResize);
```
window element의 resize event를 listen 하고 있고,  
만약 이 event가 발생한다면, handleWindowResize가 실행될 거고,  
handleWindowResize function은 body의 backgroundColor를 변경

h1은 document 밑으로 가져올 수 없다.  
body는 document 밑으로 가져올 수 있다.  
body는 특별해서 호출가능!  
document.div 이렇게 할 수 없다.  
document의 body, head, title 이런 것들은 중요하기 때문에 이렇게 존재할 수 있다.  
나머지 element들은 querySelector나 getElementById 등으로 찾아와야 함.  

<br>

###### copy
```js
function handleWindowCopy() {
    alert("copire!");
}

window.addEventListener("copy", handleWindowCopy); // copy에 반응
```

<br>

###### online, offline
```js
function handleWindowOnline() {
  alert("All Good");
}

function handleWindowOffline() {
  alert("SOS no Wifi");
}

// wifi가 연결되어 있을 때와 없을 때
window.addEventListener("online", handleWindowOnline);
window.addEventListener("offline", handleWindowOffline);
```

<br>

##### event를 사용하는 두가지 방식
```js
title.addEventListener("click", handleTitleClick); // 1. addEventListener(event, function);
title.onclick = handleTitleClick; // 2. onclick = function;
```
addEventListener는 removeEventListener를 통해서 event listener를 제거할 수 있다.  
addEventListener를 더 선호한다고 한다.

<br>