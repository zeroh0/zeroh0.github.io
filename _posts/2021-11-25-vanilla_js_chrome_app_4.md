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
querySelector는 element를 css 방식으로 검색 할 수 있다.  
해당하는 태그의 첫번째 요소의 값만 나온다.

[comment]: <> (이미지 삽입)

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
selector 안의 조건에 부합하는 모든 element를 가져다 준다.

[comment]: <> (이미지 삽입)

<br>

```js
const title = document.querySelector(".hello h2");
console.log(title);
```
```
null
```
해당 요소가 존재하지 않으면 null 값이 나온다. 
