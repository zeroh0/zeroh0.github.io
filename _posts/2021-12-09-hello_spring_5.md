---
title: "[hello-spring] 05. 정적컨텐츠"
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

## [정적컨텐츠](https://docs.spring.io/spring-boot/docs/current/reference/html/web.html#web.servlet.spring-mvc.static-content)
스프링부트은 자동으로 정적컨텐츠를 제공해준다.  

###### main / resources / static / hello-static.html
```html
<!DOCTYPE HTML>
<html>
<head>
    <title>static content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    정적 컨텐츠 입니다.
</body>
</html>
```
`localhost:9090/hello-static.html` 접속    
원하는 HTML 파일을 넣고 실행하면 정적 파일이 그대로 반환  
대신 어떤 프로그래밍을 할 순 없다.  

![image](https://user-images.githubusercontent.com/89443479/145378200-79e12211-421e-4bf9-99eb-7c04efa2d036.png)

1. `localhost:9090/hello-static.html` 접속하면
2. 내장 톰캣 서버가 요청을 받고 요청 받은 걸 스프링한테 넘긴다.
3. 먼저 컨트롤러에 hello-static이 존재하는 확인 (컨트롤러가 먼저 우선순위를 가진다.)
4. 맵핑이 된 컨트롤러가 없으므로 내부에 있는 resource: static / hello-static.html을 찾고 반환해준다.

