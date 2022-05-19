---
title: "Building a RESTful Web Service"
excerpt: "spring-guides"

categories:
    - spring
  
tags:
    - [spring]

toc: true
toc_sticky: true

date: 2022-05-17
last_modified_at: 2022-05-19
---


<br>


# Building a RESTful Web Service
- <https://spring.io/guides/gs/rest-service/>{:target="_blank"}  

## What You Will Build
**HTTP GET 요청을 수락하는 서비스 구축**  
`http://localhost:8080/greeting`

JSON 표현으로 인사말을 응답
``` json
{ "id" : 1, "content" : "Hello, World!" }
```

쿼리스트링 `name` 파라미터를 선택사항으로 인사말을 사용자 정의할 수 있다.  
`http://localhost:8080/greeting?name=User`
``` json
{ "id" : 1, "content" : "Hello, User!" }
```


<br>


## Starting with Spring Initializr
![start](https://user-images.githubusercontent.com/89443479/168815717-2dc9693a-42c6-4273-a07e-ce05a0ed4f69.png)


<br>


## Create a Resource Representation Class
- `/greeting`에 대한 `GET` 요청 처리  
- 선택사항으로 쿼리스트링 `name` 파라미터 사용  
- `GET` 요청이 인사말을 나타내는 JSON `200 OK` 응답 반환

``` json
{ 
    "id" : 1,
    "content" : "Hello, World!"
}
```
`id`는 인사말의 고유 식별자이며, `content`는 인사말의 텍스트 표현


### Greeting.java
``` java
package com.example.restservice;

public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }

}
```

<div class="notice" markdown="1">
애플리케이션은 Jackson JSON 라이브러리를 사용하여 `Greeting`의 인스턴스 유형을 JSON으로 자동 정렬<br>
Jackson은 기본적으로 web starter에 포함
</div>


<br>


## Create a Resource Controller
- RESTful 웹 서비스를 구축하는 Spring 접근 방식에서 HTTP 요청은 컨트롤러에 의해 처리
- 이러한 구성 요소는 `@RestController` 어노테이션으로 식별되며, `GreetingController`는 `Greeting` 클래스의 새 인스턴스를 반환하여 `/greeting`에 대한 `GET` 요청을 처리

### GreetingController.java
``` java
package com.example.restservice;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.util.concurrent.atomic.AtomicLong;

@RestController
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @GetMapping("/greeting")
    public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
        return new Greeting(counter.incrementAndGet(), String.format(template, name));
    }

}
```
`@GetMapping` 어노테이션을 사용하여 `/greeting`에 대한 HTTP GET 요청이 `greeting()` 메소드에서 매핑

<div class="notice" markdown="1">
다른 HTTP 요청 (POST) `@PostMapping` 있습니다.<br>
또한 `@RequestMapping` 어노테이션도 있는데, `@GetMapping`은 `@RequestMapping(method=GET)`과 같은 역할을 할 수 있습니다.  
</div>

`@RequestParam`은 쿼리 스트링 파라미터 `name` 값을 `greeting()` 메소드의 `name` 매개 변수에 바인딩합니다.  
요청에 `name` 매개 변수가 없는 경우 기본값인 `World` 사용됩니다.  

메소드의 구현은 `counter`의 다음 값의 기준과 인사말 `template`을 사용하여 주어진 `name` 형식의 속성을 가진 `id`와 `content`의 새 `Greeting` 객체를 만들고 반환합니다.  

기존의 MVC 컨트롤러와 앞에서 설명한 RESTful 웹 서비스 컨트롤러의 주요 차이점은 HTTP 응답 본문을 만드는 방법입니다.    
RESTful 웹 서비스 컨트롤러는 view technology에 의존하지 않고 인사말 데이터를 HTML로 서버 측에서 렌더링하는 `Greeting` 객체를 채우고 반환합니다.  
객체 데이터는 HTTP 응답에 JSON으로 직접 기록됩니다.  

이 코드는 스프링 `@RestController` 어노테이션을 사용하여 클래스의 모든 메소드가 뷰 대신 도메인 객체를 반환하는 컨트롤러로 표시합니다.  
@Controller와 @ResponseBody를 모두 포함하는 약자입니다.  

`Greeting` 객체를 JSON으로 변환해야 합니다.  
스프링의 HTTP 메시지 변환 지원 덕분에 이 변환을 수동으로 수행할 필요가 없습니다.  
Jackson2가 클래스 패스이기 때문에 `Greeting` 인스턴스를 JSON으로 변환하려면 스프링의 `MappingJackson2HttpMessageConverter`가 자동으로 선택됩니다.  

`@SpringBootApplication`은 다음 사항을 모두 추가한 편리한 어노테이션
  - `@Configuration`: 설정 파일을 만들기 위한 어노테이션또는 빈을 등록하기 위한 어노테이션
  - `@EnableAutoConfiguration`: 클래스 경로 설정, 다른 빈, 다양한 속성 설정에 따라 빈 추가를 하도록 스프링부트에 지시합니다.  
  예를 들어 `spring-webmvc`가 클래스 경로에 있는 경우 이 어노테이션은 애플리케이션을 웹 애플리케이션으로 플래그를 지정하고 `DispatcherServlet` 설정과 같은 주요 동작을 활성화합니다.  
  - `@ComponentScan`: 스프링에게 `com/example` 패키지에서 다른 구성 요소, 구성 및 서비스를 찾아 컨트롤러를 찾도록 지시합니다.

`main()` 메소드는 스프링부트의 `SpringApplication.run()` 메소드를 사용하여 애플리케이션을 시작합니다.  
XML의 줄이 한 줄도 없다는 것을 알아차리셨나요? `web.xml` 파일도 없습니다.  
이 웹 애플리케이션은 100% 순수 자바이므로 연결이나 환경 구축을 할 필요가 없습니다.    


<br>


## Build an executable JAR
- gradle을 사용한다면 `./gradlew bootRun`으로 애플리케이션을 실행할 수 있다.
- 또는 `./gradlew build`로 jar 파일 빌드하고 다음과 같이 jar 파일 실행
  
```
java -jar build/libs/restservice-0.0.1-SNAPSHOT.jar
```


<br>


## Test the Service
`http://localhost:8080/greeting` 접속
``` json
{ "id" : 1, "content" : "Hello, World!" }
```

`http://localhost:8080/greeting?name=User` 접속하여 `name` 쿼리 스트링 파라미터를 제공  
다음 목록과 같이 `content` 속성의 값이 `Hello, World!`에서 `Hello, User!`로 어떻게 변경되는지 주목  
``` json
{ "id" : 2, "content" : "Hello, User!" }
```

이러한 변경은 `GreetingController`의 `@RequestParam` 매개 변수가 예상대로 작동하고 있을을 나타냅니다.  
`name` 파라미터에 `World`라는 기본값이 지정되었지만 쿼리 스트링을 통해서 명시적으로 재정의할 수 있습니다.  

또한 id 속성이 1에서 2로 어떻게 변경되었는지 주목하십시오.  
이렇게 하면 여러 요청에 걸쳐 동일한 `GreetingController` 인스턴스에 대해 작업 중이고 각 호출에 대해 `counter`가 예상대로 증가하고 있음을 알 수 있습니다.  
