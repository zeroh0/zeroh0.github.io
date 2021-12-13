---
title: "[hello-spring] 06. MVC와 템플릿 엔진"
excerpt: "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술"

categories:
    - java
tags:
    - [java]

toc: false
toc_sticky: false

date: 2021-12-09
last_modified_at: 2021-12-09
---

_웹을 개발하는데 있어서는 크게 3가지 방법이 있다._

**정적컨텐츠**  
Welcome Page와 같이 파일을 웹브라우저에 그대로 내려주는 것

**MVC와 템플릿 엔진**  
과거의 JSP, PHP 이런 것들이 소위 템플릿 엔진이다.  
HTML을 그대로 주는 것이 아닌 서버에서 프로그래밍을 해서 동적으로 바꿔서 내리는 것  
이런 것들을 하기 위해서 컨트롤러, 모델, 화면 이걸 MVC라고 부른다.

**API**  
만약 안드로이드나 아이폰 클라이언트랑 개발을 할 시에는  
서버 입장에서는 과거에는 xml을 사용했지만  
요즘에는 json이라는 데이터 포맷으로 내려준다.  
html을 내리는 게 아닌  json이라는 데이터 구조 포맷으로 클라이언트에게 전달하는 방식  
vue나 react 이런 것들을 사용할 때도 API로 데이터만 보내주면 화면은 클라이언트가 알아서 그리고 정리  
서버끼리 통신할 때 서버끼리는 HTML을 통신할 필요가 없기 때문에 어떤 데이터가 왔다 갔다 하는지가 중요하기 때문에 사용  

## MVC와 템플릿 엔진
#### MVC : Model, View, Controller
View는 화면과 관련된 일에 집중    
비즈니스 로직과 서버 뒷 단에 관련된 건 Controller나 뒷 단 비즈니스 로직에서 처리하고  
Model에 화면에 필요한 관련된 정보들을 담아서 넘겨준다.  

###### hello.hellospring.controller.HelloController
```java
@GetMapping("hello-mvc") 
public String hellomvc(@RequestParam("name") String name, Model model) { 
    model.addAttribute("name", name); // 외부에서 parameter를 model에 담아서 view에서 렌더링 할 때 사용 
    return "hello-template"; // hello-template.html
}
```

###### resources / templates / hello-template.html
```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello! ' + ${name}">hello! empty</p> <!-- ${name}은 HelloController에서 넘겨준 값 -->
</body>
```
타임리프의 장점: html을 그대로 쓰고 파일을 서버없이 열어봐도 껍데기를 볼 수 있다.  
hello! empty -> hello! ${name}으로 치환  
hello! empty가 있는 이유는 서버없이 html을 만들어서 볼 때, 마크업 할 때 가이드 역할  

localhost:9090/hello-mvc?name=spring -> model에 name(param)을 담아서 hello-template.html로 return

![image](https://user-images.githubusercontent.com/89443479/145786421-53c713e4-709b-4738-97af-91187f43056c.png)

1. 웹 브라우저에서 localhost:9090/hello-mvc를 넘기면 
2. 내장 톰캣 서버를 먼저 거쳐서 hello-mvc를 스프링에서 던져준다.
3. helloController에 매핑이 되있기 때문에 메소드 호출
4. return을 해줄 때 hello-template으로, model은 키는 name, 값은 spring
5. 스프링한테 넘겨준다. 그러면 스프링이 viewResolver가 동작 view를 찾아주고 템플릿 엔진을 연결해준다.
6. return 값과 똑같은 걸 찾아서 타임리프 템플릿 엔진에 처리해주세요. 템플릿 엔진이 렌더링을 해서 변환을 한 html을 웹 브라우저에 넘겨준다.