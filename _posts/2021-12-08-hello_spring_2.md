---
title: "[hello-spring] 02. 라이브러리 살펴보기"
excerpt: "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술"

categories:
    - java
tags:
    - [java]

toc: false
toc_sticky: false

date: 2021-12-08
last_modified_at: 2021-12-08
---

## 라이브러리 살펴보기
build.gradle 파일에 dependencies **thymeleaf**, **spring web**

하지만 실제로 라이브러리를 확인하면 엄청 많다!

gradle은 의존 관계를 관리해준다.
라이브러리를 가져오면 해당 라이브러리에 필요한 라이브러리를 모두 가져온다.

오른쪽 사이드에 Gradle 탭에서 라이브러리를 확인할 수 있다.

##### 스프링 부트 라이브러리
- spring-boot-starter-web
  - spring-boot-starter-tomcat: 톰캣 (웹서버)
  - spring-webmvc: 스프링 웹 MVC
- spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
- spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
  - spring-boot
    - spring-core
  - spring-boot-starter-logging
    - logback, slf4j

##### 테스트 라이브러리
- spring-boot-starter-test
  - junit: 테스트 프레임워크
  - mockito: 목 라이브러리
  - assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
  - spring-test: 스프링 통합 테스트 지원

<br>

![springboot](https://user-images.githubusercontent.com/89443479/145245289-890ebbe5-2259-489e-be9c-fadcc8503ce2.png)

![test](https://user-images.githubusercontent.com/89443479/145245182-346c3a04-94b0-4aa1-bf31-6e2be986c2b6.png)