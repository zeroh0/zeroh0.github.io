---
title: "바닐라 JS로 크롬 앱 만들기_8차시"
excerpt: "바닐라 JS로 크롬 앱 만들기"

categories:
    - javascript
tags:
    - [javascript]

toc: false
toc_sticky: false

date: 2021-12-11
last_modified_at: 2021-12-11
---

## To-Do List

todo.js 파일 생성 html 파일에 연결  
- 사용자가 입력을 하기 위한 form : id가 todo-form인 form 생성  
- 입력 받은 to-do를 나열하기 위한 list를 보여주기 : id가 todo-list인 ul 생성

###### index.html
```html
<h2 id="clock">00:00:00</h2>
<form id="login-form" class="hidden">
    <input type="text" placeholder="What is your name?" required maxlength="15">
    <input type="submit" value="Log In">
</form>
<h1 id="greeting" class="hidden"></h1>
<form id="todo-form">
    <input type="text" placeholder="Write a To Do and Press Enter" required>
</form>
<ul id="todo-list"></ul>
<div id="quote">
    <span></span>
    <span></span>
</div>
<script src="./js/greeting.js"></script>
<script src="./js/clock.js"></script>
<script src="./js/quotes.js"></script>
<script src="./js/background.js"></script>
<script src="./js/todo.js"></script>
```

###### todo.js
```js
const toDoForm = document.getElementById("todo-form");
// const toDoInput = toDoForm.querySelector("input");
const toDoInput = document.querySelector("#todo-form input");
const toDoList = document.getElementById("todo-list");
```
사용자 입력 form과 form 안에 input 그리고 입력 값을 보여줄 ul을 자바스크립트에 가져온다.

### list 입력
```js
function handleToDoSubmit(event) {
    event.preventDefault();
    const newToDo = toDoInput.value; 
    toDoInput.value = ""; // 사용자가 입력할 때 마다 이전 값들을 지우고 입력하지 않도록
    paintToDo(newToDo); // toDoInput.value를 비우기 전의 값을 paintToDo에 넣어서 호출
    /* 
        toDoInput.value를 비웠다고 해서 newToDo가 비워지지 않는다.
        input의 value를 새로운 변수에 복사하는 것이기 때문에
        newToDo에는 아무런 영향이 없다. 
     */
}

toDoForm.addEventListener("submit", handleToDoSubmit);
```
사용자가 input에 값을 입력할 때 마다 이전에 입력했던 값을 지워야 되서 이전 입력 값을 지우도록 한다.  
그리고 입력 값을 지우기 전 입력 값을 newTodo 변수에 저장한다.  

### list 추가
```js
/* todo를 그리는 역할 */
function paintToDo(newToDo) {    
    const li = document.createElement("li"); // ul태그 안에 li 태그를 만들어 입력값을 추가해주기 위해
    /* 추후 삭제 버튼을 만들기 위해 <li><span></span></li> span 태그을 이용해 li 태그를 생성 */
    const span = document.createElement("span");
    span.innerText = newToDo; // span의 텍스트는 handleToDoSubmit에서 가져온 newToDo
    const button = document.createElement("button"); // 삭제를 하기 위한 버튼 생성
    button.innerText = "❌";
    button.addEventListener("click", deleteToDo);
    li.appendChild(span); // li는 span이라는 자식을 가지게 됨.
    li.appendChild(button);
    toDoList.appendChild(li);
}
```
paintToDo는 입력 값을 받아서 리스트로 보여주는 역할을 한다.  
그래서 기존에 존재하는 ul 태그 안에 li를 추가해주기 위해 생성 추가  
li 안에 삭제 버튼을 추가하기 위하여 span과 button을 생성 추가

### list 삭제
```js
// 버튼이 클릭된다는 걸 알지만 어디를 클릭한 건지 알 수 없다.
// 어떤 li를 제거해야 되는 지를 알지 못함
/* 
    evene를 인수로 받아 클릭된 button이 어떤 건지에 대한 단서를 얻을 수 있다.
    console.dir(event); 
        path를 통해서 event가 click된 위치를 알 수 있다. -> 클릭 target이 무엇인지를 체크할 수 있다.
    console.dir(event.target); / target은 클릭된 HTML element, 모든 HTML element에는 하나 이상의 property가 존재
        parentNode, parentElement를 통해 button의 부모를 알 수 있다. / parentElement는 클릭된 element의 부모
    console.log(event.target.parentElement.innerText);
        어떤 것이 클릭되었는 지 알 수 있다.
*/
function deleteToDo(event) {
    const li = event.target.parentElement;
    li.remove();
}
```
삭제 버튼을 클릭하게 되면 어떤 버튼을 클릭하지 알 수가 없기 때문에  
event의 property를 활용해 event.target.parentElement를 통해서 어떤 li를 삭제해야 되는지 알 수 있다.

### list 저장
```js
/* paintToDo newToDo가 그려질 때 마다 그 텍스트를 array에 push */
const toDos = [];

function saveToDos() {
    localStorage.setItem("toDos", JSON.stringify(toDos));
}

function handleToDoSubmit(event) {
    event.preventDefault();
    const newToDo = toDoInput.value;
    toDoInput.value = ""; 
    toDos.push(newToDo); // localStorage에 array를 저장할 수 없다. localStorage는 오직 텍스트만 저장할 수 있다.
    paintToDo(newToDo);
    saveToDos(); //
}
```
localStorage에 array를 저장할 수 없다.  
localStorage는 오직 텍스트만 저장할 수 있다.  
그래서 `JSON.stringify()`를 사용  
JSON.stringify() : javascript object나 array 또는 어떤 javascript 코드건 간에 그걸 string으로 만들어준다.

### list 불러오기
```js
const TODOS_KEY = 'toDos'; //중복

// application이 시작될 때 toDos array는 항상 비어있다
let toDos = [];

function sayHello(item) { 
    // eventListener가 event를 제공해 주는 것처럼 javascript는 지금 처리되고 있는 item 또한 제공
    console.log("this is the turn of", item);
}

const savedToDos = localStorage.getItem(TODOS_KEY);
if(savedToDos !== null) {
    const parseToDos = JSON.parse(savedToDos);
    toDos = parseToDos; // toDos에 parseToDos를 넣어서 이전의 입력값들을 복원
    parseToDos.forEach((item) => console.log("this is the turn of", item)); // forEach() : array에 있는 각각의 item에 대해서 실행해준다.
    // parseToDos.forEach(sayHello);
}

```
string을 array 으로 변환 : `JSON.parse()`  
`forEach()` : array에 있는 각각의 item에 대해서 실행해준다.  

값을 추가하고나서 새로고침을 한 뒤 값을 추가하면 화면 상에서는  
값이 추가된 것 처럼 보이지만 local storage에는 새로운 값들로만 채워져있다.  
그래서 입력 값들을 담는 toDos를 let으로 변경한 뒤 값이 있을 때는  
local storage에 존재하는 값을 toDos 배열에 추가  

### list 삭제
```js
function deleteToDo(event) {
    const li = event.target.parentElement;
    toDos = toDos.filter(toDo => toDo.id !== parseInt(li.id)); // 저장되있는 id와 li에 있는 id가 다를 때 남기기 위해, toDo.id는 number li.id는 string이기 때문에 형변환을 해준다.
    li.remove();
    saveToDos(); // 업데이트
}
```
만약 array에서 뭔가를 삭제할 때 실제로 array에서 지우는 것이 아님.  
진짜 일어나는 일은 지우고 싶은 item을 빼고 새 array를 만든다.  
**item을 지우는 게 아니라 item을 제외하는 것**

```js
const toDoArr = [{text:"lalala"}, {text:"lololo"}];
function sexyFilter(todo) { return todo.text !== "lololo"}
toDoArr.filter(sexyFilter);
```
filter 함수는 반드시 true를 리턴해야 됨. 만약 새 array에서 object를 유지하고 싶으면
