---
title: "[hello-spring] 07. API"
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

##### hello.hellospring.controller.HelloController
```java
@GetMapping("hello-string") 
@ResponseBody // 무지성 꼬라박기 - html 태그 없음 그냥 내용만 return (데이터를 그대로 내려준다) 
public String helloString(@RequestParam("name") String name) {
    return "hello " + name; // "hello spring"
}
```
@ResponseBody: 요청한 클라이언트에 값을 그대로 내려준다.  

localhost:9090/hello-string?name=spring -> 소스코드 확인하면 return 값만 존재 (태그 존재 x)

##### hello.hellospring.controller.HelloController
```java
// 문자가 아닌 데이터 내놔!
@GetMapping("hello-api")
@ResponseBody //json 반환이 default
public Hello helloApi(@RequestParam("name") String name) {
    Hello hello = new Hello();
    hello.setName(name);
    return hello; // 객체를 넘겼을 때: 'JSON' - key:value 구조
}

static class Hello {
    private String name;
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
}
```
localhost:9090/hello-api?name=spring -> {"name" : "spring"}

`JSON`은 key:value 구조  
과거에는 xml 방식이 많이 쓰였다. xml 방식은 무겁고 열고 닫는 태그 두 번을 씀.  
JSON은 심플하다.  

**자바빈 표준 방식**  
private로 선언된 필드는 외부에서 접근 불가  
그래서 public 메소드를 통해서 접근 (property 접근 방식)  

<br>

![image](https://user-images.githubusercontent.com/89443479/145790764-856c87b6-1cf0-4274-a827-cf8c77708271.png)

1. localhost:9090/hello-api를 입력하면 
2. 톰캣 내장 서버에서 hello-api가 왔어! 스프링에 던진다.
3. 스프링은 hello-api가 있네! 그런데 ResponseBody가 있다면 http에 응답에 데이터를 그대로 넘겨야 되겠구라고 동작 그런데 문자가 아니고 객체(hello)인 경우에는 기본 JSON 방식으로 데이터를 만들어서 http 응답에 반환하겠다.
4. 객체를 넘기면 몇 가지 조건이 있다. HttpMessageConverter가 동작 (기존에는 viewResolver) 단순 문자면 StringConverter가 동작, 객체면 JSONConverter가 기본으로 동작해서 JSON으로 변환해서 
5. 요청한 웹 브라우저나 서버에 응답

- @ResponseBody 를 사용
  - HTTP의 BODY에 문자 내용을 직접 반환
  - viewResolver 대신에 HttpMessageConverter 가 동작
  - 기본 문자처리: StringHttpMessageConverter
  - 기본 객체처리: MappingJackson2HttpMessageConverter (JSON 변환 라이브러리)
  - byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음 (실무에서 거의 그래로 쓴다고 한다 ㅎㅎ)

참고: 클라이언트의 HTTP Accept 해더와 서버의 컨트롤러 반환 타입 정보 둘을 조합해서 HttpMessageConverter가 선택된다. (나는 꼭 어떤 포맷으로 받을거야!! 라고 하고 싶을 때 동작 - 예로는 XML로 반환받고 싶을 때) 