---
title: "[hello-spring] 03. View 환경설정"
excerpt: "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술"

categories:
    - java
tags:
    - [java]

toc: false
toc_sticky: false

date: 2021-12-08
last_modified_at: 2021-12-09
---

## [Welcome Page](https://docs.spring.io/spring-boot/docs/current/reference/html/web.html#web.servlet.spring-mvc.welcome-page) 생성    

###### main / resources / static / index.html
```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    Hello
    <a href="/hello">hello</a>
</body>
</html>
```

#### thymeleaf 템플릿 엔진

- [thymeleaf](https://www.thymeleaf.org/)
- [스프링 공식 튜토리얼](https://spring.io/guides/gs/serving-web-content/)
- [스프링부트 메뉴얼](https://docs.spring.io/spring-boot/docs/current/reference/html/web.html#web.servlet.spring-mvc.template-engines)

###### hello.hellospring.controller.HelloController
```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller // Controller 적어
public class HelloController {

    @GetMapping("hello") // 웹 어플리케이션에서 "/hello" 들어오면 현재 메소드 호출
    public String hello(Model model) { // MVC의 Model을 말한다.
        model.addAttribute("data", "hello!!"); // data : hello!!
        return "hello"; // hello.html
    }
}
```

###### main / resources / templates / hello.html
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
<!-- th는 thymeleaf template -->
<!-- HelloController의 data의 value로 치환 -->
</body>
</html>
```

<br>

1. localhost:9090/hello  
2. 스프링부트는 톰캣이라는 웹 서버를 내장하고 있는데 서버에서 받아서 스프링한테 물어본다.  
3. helloController에 @GetMapping("hello") url에 매칭
4. 컨트롤러에 있는 hello(model) 메소드 실행
5. 스프링이 Model이라는 걸 만들어서 model에 data:hello!! 넣어준다.
6. return의 이름이 hello -> resources / templates / hello.html 스프링(viewResolver)이 경로를 찾아서 Thymeleaf 템플릿 엔진 처리 (화면 렌더링, 화면 출력)

<br>

![hello](https://user-images.githubusercontent.com/89443479/145245740-8f7eaf05-0702-42d4-b377-453f5a01da55.png)

- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버`viewResolver`가 화면을 찾아서 처리한다.
  - 스프링 부트 템플릿엔진 기본 viewName 매핑
  - `resources:templates/` + {ViewName} + `.html`

