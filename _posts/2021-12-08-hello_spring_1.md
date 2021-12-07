---
title: "hello-spring 프로젝트 생성"
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

## 사전 준비물

- [Java 11 설치](https://www.oracle.com/kr/java/technologies/javase/jdk11-archive-downloads.html)
- IDE: [intelliJ 설치](https://www.jetbrains.com/ko-kr/idea/download/)

## 스프링 부트 스타터 사이트로 이동해서 스프링 프로젝트 생성
[http://start.spring.io](http://start.spring.io)  
스프링 부트 기반으로 스프링 관련 프로젝트를 생성해주는 사이트


- 프로젝트 선택
  - Project: Gradle Project
  - Spring Boot: 2.6.1
  - Language: Java
  - Package: Jar
  - Java: 11
- Project Metadata
  - groupId: hello
  - artifactId: hello-spring
- Dependencies: Spring Web, Thymeleaf

###### Project
프로젝트에는 Gradle과 Maven이 있는데 Gradle로 진행  
필요한 라이브러리 가져오고 빌드하는 라이프사이클까지 관리해주는 툴  
과거에는 레거시 프로젝트나 과거 프로젝트에는 maven을 많이 사용했지만  
요즘 추세는 Gradle로 넘어오는 추세!  
스프링 라이브러리 관리 자체도 Gradle로 넘어온 상황  

###### Spring Boot
스프링부트 버전은 가장 최신의 정식 릴리즈 버전으로 진행!  

###### Project Metadata  
Group에는 보통 기업 도메인 명을 적어준다.  
Artifact는 빌드의 결과물 (프로젝트 명)

###### Dependendies
스프링 부트 기반의 프로젝트를 만들 때 어떤 라이브러리를 가져와서 쓸 지  
웹 프로젝트를 만들기 위해서는 <b>Spring Web</b>  
HTML을 만들어 주는 템플릿 엔진 <b>Thymeleaf</b>  

<br>

선택을 완료했으면 <b>GENERATING</b>  
다운로드 받은 파일을 압축해제 후  
인텔리제이에서 Open or Import - build.Gradle 선택 후 open - open as Project  

###### 파일 구조
<b>gradle</b>  
gradle과 관련되서 gradle를 사용하는 디렉토리
 
<b>src - test, resources</b>    
main과 test라는 폴더가 따로 나누어져있다.  
 
<b>src - main - java, resources</b>      
java 폴더에는 실제 패키지와 소스 파일들이 존재  
resources 폴더에는 실제 자바 코드 파일을 제외한 html, xml, properties 등등 설정 파일이 존재  

<b>src - test</b>  
테스트와 관련된 소스들이 존재  
test 코드라는 것이 개발 트렌드에 있어서는 정말 중요!  

<b>**build.gradle*</b>  
gradle이 버전 설정하고 라이브러리를 가져오는 역할!  
mavenCentral(): 선택한 라이브러리를 다운로드  
dependencies: 선택한 Dependendies  
gitignore: 소스코드 관리, 깃에는 필요한 소스코드만 올라가도록 설정, 빌드된 결과물은 올라가면 안되는 데 start.spring.io에서 자동으로 설정  
gradlew, gradle.bat: gradle로 빌드할 때 사용  

[comment]: <> (gradle의 역할, 코드 분석 추가 포스팅 예정)

###### 생성한 hello-spring 프로젝트의 tree 구조
![hello-spring_tree](https://user-images.githubusercontent.com/89443479/145071665-378c2eaa-eca8-43df-9ef3-a71f07f73aef.png)

[comment]: <> (스프링 프로젝트의 tree 구조는 추가 포스팅 예정)

<br>

###### HelloSpringApplication.java
```java
package hello.hellospring;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class HelloSpringApplication {

    public static void main(String[] args) {
        SpringApplication.run(HelloSpringApplication.class, args); // 내장된 톰캣 서버 자체적으로 띄우면서 스프링부트가 같이 올라옴.
    }

}
```
![HelloSpringApplicationRun](https://user-images.githubusercontent.com/89443479/145071819-4f81f567-d80f-4b0a-aab6-7c092dc714e5.png)
Tomcat started on port(s): 9090 (http) with context path ''  
주소 창에 localhost:9090 입력  

![localhost9090](https://user-images.githubusercontent.com/89443479/145071902-aed40f4a-3829-49d9-9c8a-9b10832b738a.png)  
해당 페이지가 보인다면 성공!

<br>

###### application.properties
```properties
server.port = 9090
```
필자는 8080 포트가 겹쳐서 에러가 발생했기 때문에 application.properties 파일에 port 번호를 변경해주었다.

<br>

##### invalid source release 11 에러 발생 시  
`File -> Project Structure -> Project Settings -> Project` 탭의 `Project SDK`,`Project Language Level` 확인  
![ProjectStructure_Project](https://user-images.githubusercontent.com/89443479/145072095-3c59dc65-ddf3-40c8-8138-ce1887ee68c6.png)

`File -> Project Structure -> Project Settings -> Modules` 탭의 `Language level` 확인
![ProjectStructure_Modules](https://user-images.githubusercontent.com/89443479/145072049-41803370-c457-433d-b869-750b3d67fbea.png)

`Preferences -> Build,Execution,Deployment -> Gradle` 탭의 `Gradle JVM` 확인
![Preference_Gradle](https://user-images.githubusercontent.com/89443479/145071945-21c6bfad-510a-45ea-bdc1-cb66aaa6e744.png)

`Preferences -> Build,Execution,Deployment -> Complier -> Java Complier` 탭의 `Project bytecode version` 확인  
![Preference_JavaComplier](https://user-images.githubusercontent.com/89443479/145072003-13e2b156-fe71-4df8-b77f-503f4f72ba00.png) 
