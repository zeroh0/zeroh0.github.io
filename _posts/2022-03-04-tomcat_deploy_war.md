---
title: "톰캣 서버 WAR 배포"
excerpt: "포트포워딩을 활용해 WAR 배포"

categories:
    - server
tags:
    - [server]

toc: false
toc_sticky: false

date: 2022-03-04
last_modified_at: 2022-03-04
---

스프링, 스프링부트을 통해서 만들어진 프로젝트는 보통 서버 구축을 AWS를 사용한다고 들었다.  
하지만 시간이 부족하기에 과거에 포트포워딩 방식으로 JSP 프로젝트를 배포한적이 있어 **포트포워딩**을 활용해 프로젝트를 배포 계획

# 사전작업

## 공유기 설정
`콘솔 창에서 ipconfig를 입력`

![ipconfig](https://user-images.githubusercontent.com/89443479/155833990-6f76f48a-3ba7-488f-8798-2d366d1afe73.png)  
기본 게이트웨이를 인터넷 브라우저에 입력해서 공유기 설정 페이지로 들어간다.  
필자는 iptime 공유기를 사용했지만 과거에 U+ 공유기로도 포트포워딩을 설정해보았는데 다른 공유기들도 설정 방법은 비슷하다.

<br>

`고급설정 - NAT 라우터 관리 - 포트포워드 설정`

![port](https://user-images.githubusercontent.com/89443479/155834270-56f43535-bc67-460b-b144-0b2a10a5196f.png)  
**규칙 이름을 입력**하고 **내부 ip 주소는 현재 접속된 ipv4의 주소를 입력**하거나 **현재 접속된 ip 주소 체크박스를 체크**하면 된다.  
그리고 **프로토콜은 TCP/UDP로 선택**, **외부 포트는 톰캣 서버를 필자는 9000으로 설정**했기 때문데 9000으로 외부/내부 포트 번호를 설정해주었다.

<br>

## 스프링 프로젝트 설정 (maven, war)

###### WedingApplication.java
``` java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class WedingApplication extends SpringBootServletInitializer {

	public static void main(String[] args) {
		SpringApplication.run(WedingApplication.class, args);
	}
	
	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
		return builder.sources(WedingApplication.class);
	}

}
```

SpringBootServletInitializer를 상속받아서  
SpringApplicationBuilder configure(SpringApplicationBuilder builder)를 오버라이딩 받은 뒤  
return 값을 `builder.sources(프로젝트명Application.class)`으로 변경

<br> 

##### 이미지 경로 처리

###### WebConfig.java
``` java
import org.springframework.beans.factory.annotation.Value; 
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    // application.yml에 resource.path
	@Value("${resource.path}")
	private String resourcePath;
	
    // application.yml에 upload.path
	@Value("${upload.path}")
	private String uploadPath;

	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler(uploadPath)
				.addResourceLocations(resourcePath);
	}
	
}
```

<br>

###### application.yml
``` yml
resource:
  path: file:///C:/Program Files/Apache Software Foundation/Tomcat 9.0/webapps/Weding/upload/
        
upload:
  path: /upload/**
```

실제로 업로드되는 파일의 경로를 설정한 것인데  
경로를 upload/test.jpg로 파일을 업로드하게 되면
실제로는 C:/Program Files/Apache Software Foundation/Tomcat 9.0/webapps/Weding/upload/test.jpg에 저장한다.

<br>

###### pom.xml
``` xml
<!-- 내장 톰캣 -->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <scope>provided</scope>
    </dependency>
</dependencies>

<packaging>war</packaging>

<build>
    <defaultGoal>install</defaultGoal>
</build>
```
pom.xml에 추가
- 내장톰캣은 기본적으로 추가되있어서 추가해줄 필요가 없지만 없을 수 도 있으니 적어놓았다.
- 패키징 방식 war로 배포해주기 위해 프로젝트를 생성할 때 war로 생성해주었다면 기본적으로 추가되어있다.
- `<build>` 태그 안에 `<defaultGoal>` 태그로 install을 적어주었는데 이걸 추가해주지 않고 빌드를 했을 때 실패를 해서 구글에 검색을 해보니 기본 값을 설정해주지 않아서 빌드를 실패한다고 해서 추가해주었다.

<br>

###### maven build
`프로젝트 우클릭 - Run As - Maven Build...`

![image](https://user-images.githubusercontent.com/89443479/155834994-a9f98967-959e-4c12-9df8-e80eca38f383.png)  
Goal을 pom.xml에서 defaultGoal로 지정해주었던 install를 적는다.  
Profiles는 빈 칸으로 둔다.  
그리고 Run 버튼을 클릭하게 되면 빌드가 되는데

![image](https://user-images.githubusercontent.com/89443479/155835187-79acd501-e090-4de8-b139-057e53e7cefe.png)
빌드가 성공이 되면 BUILD SUCCESS 메세지와 함께 빌드된 war 파일의 경로가 나온다.  
그 경로로 가게되면

![image](https://user-images.githubusercontent.com/89443479/155835227-bae08045-1048-48d2-a64d-87318872a796.png)  
빌드가 완료된 war 파일이 있는 걸 확인할 수 있다.

<br>

## 톰캣 설정
실제 톰캣이 있는 경로로 들어간 뒤 server.xml에서 포트 번호를 9000으로 변경해준다.  
*C:\Program Files\Apache Software Foundation\Tomcat 9.0\conf\server.xml*

![image](https://user-images.githubusercontent.com/89443479/155835611-e4a0e92c-4256-457b-9218-331f81a95f6e.png)

그리고 빌드가 완료된 war 파일을 wabapps 폴더에 넣어주고  
bin 폴더에 들어가 Tomcat9.exe나 Tomcat9w.exe로 톰캣을 실행해주게 되면

![image](https://user-images.githubusercontent.com/89443479/155835786-7e7bc31a-0a23-43d0-b406-32059119e5db.png)  
프로젝트 폴더가 생성이 된다.

<br>

## 마지막

네이버 ip 주소 확인을 검색해주면 공인 ip를 확인할 수 있는데  
![image](https://user-images.githubusercontent.com/89443479/155835910-78f80cbe-de00-4681-b684-69f550068089.png)

공인ip:톰캣서버포트/프로젝트Context를 입력해주면 된다.  
*예) 116.xx.xx.xxx:9000/Weding*

그렇게 입력해주게 되면

![weding](https://user-images.githubusercontent.com/89443479/155836027-462288fc-d98e-46d5-aea3-dfaa6226e7e0.png)

아주 잘 나온다! 👍