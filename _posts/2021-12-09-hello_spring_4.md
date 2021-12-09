---
title: "[hello-spring] 04. 빌드하고 실행하기"
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

## 빌드 & 실행

터미널에 접속 후 hello-spring 프로젝트 디렉토리로 이동  
`./gradlew build`  
`cd build`  
`cd libs`  
`ls -arlth`    
`java -jar hello-spring-0.0.1-SNAPSHOT.jar`  

![build](https://user-images.githubusercontent.com/89443479/145372532-a7b2ad58-c73b-4c4d-ab0e-51edcde277ae.png)

localhost:9090 접속 후 확인!

**빌드가 안되는 경우**    
`./gradlew clean`실행하면 `ls`를 입락하면  
build 폴더가 사라지는 걸 확인할 수 있다. 그 후`./gradle clean build`  
그 다음 libs 폴더로 이동 후 `java -jar ~.jar` 실행!

