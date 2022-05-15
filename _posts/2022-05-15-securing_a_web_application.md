---
title: "Securing a Web Application"
excerpt: "spring-guides"

categories:
    - spring
  
tags:
    - [spring]

toc: true
toc_sticky: true

date: 2022-05-15
last_modified_at: 2022-05-15
---

<br>

# Securing a Web Application
- <https://spring.io/guides/gs/securing-web/>{:target="_blank"}
- <https://docs.spring.io/spring-boot/docs/2.4.3/reference/htmlsingle/#boot-features-security>{:target="_blank"}
- <https://godekdls.github.io/Spring%20Boot/security/>{:target="_blank"}


## Starting with Spring Initializr
![start](https://user-images.githubusercontent.com/89443479/168457186-e717d2cd-3362-4e2c-be49-421c33895ffc.png)


<br>


## Create an Unsecured Web Application

### home.html
``` html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="https://www.thymeleaf.org"
      xmlns:sec="https://www.thymeleaf.org/thymeleaf-extras-springsecurity3">
<head>
    <title>Spring Security Example</title>
</head>
<body>
    <h1>Welcome!</h1>
    <p>Click <a th:href="@{/hello}">here</a> to see a greeting.</p>
</body>
</html>
```


<br>


### MvcConfig.java
``` java
package com.example.securingweb;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration // 스프링 설정 클래스를 선언하는 어노테이션
public class MvcConfig implements WebMvcConfigurer {
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/home").setViewName("home");
        registry.addViewController("/").setViewName("home");
        registry.addViewController("/hello").setViewName("hello");
        registry.addViewController("/login").setViewName("login");
    }
}
```
- Spring MVC를 구성하고 view controller 설정
  - `/home` &rarr; home.html
  - `/` &rarr; home.html
  - `/hello` &rarr; hello.html
  - `/login` &rarr; login.html


<br>


## Set up Spring Security

### build.gradle
```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.springframework.security:spring-security-test'
}
```
- 클래스패스에 스프링 시큐리티가 있으면 디폴트로 웹 애플리케이션을 보호해준다.
- Spring Security를 사용하기 위해 dependencies에 애플리케이션용과 테스트용을 추가


<br>


### WebSecurityConfig
``` java
package com.example.securingweb;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Configuration // 스프링 설정 클래스를 선언하는 어노테이션
@EnableWebSecurity // 웹 보안 지원 활성화
public class WebSecurityConfig extends WebSecurityConfigurerAdapter { // 웹 보안 구성

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests() // 요청에 의한 보안 검사 시작
                    .antMatchers("/", "/home").permitAll() // 어떤 사용자든지 특정 경로를 접근 가능
                    .anyRequest().authenticated() // 설정한 경로 외의 모든 경로는 인증된 사용자만이 접근 가능
                    .and()
                .formLogin() // form 기반의 로그인
                    .loginPage("/login") // 로그인 페이지의 URL을 설정
                    .permitAll() // 어떤 사용자든지 접근 가능
                    .and()
                .logout() // 로그아웃에 대해 설정
                    .permitAll(); // 어떤 사용자든지 접근 가능
    }

    @Bean 
    @Override
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withDefaultPasswordEncoder()
                .username("user")
                .password("password")
                .roles("USER")
                .build();
        return new InMemoryUserDetailsManager(user);
    }
}
```
- `@EnableWebSecurity`: Spring Security의 웹 보안 지원 활성화
- `WebSecurityConfigurerAdapter`: 웹 보안 구성의 세부 사항
- `configure(HttpSecurity http)`: 보안되어야 하는 URL 경로와 보안되지 않아야 하는 URL 경로를 정의
  - `/`와 `/home`경로에 인증이 필요하지 않도록 구성
- 사용자가 성공적으로 로그인하면 이전에 요청했던 인증 페이지로 리다이렉트
- `userDetailsService()`: 단일 사용자로 메모리 내 사용자 저장소를 설정
  - 해당 사용자에게 사용자 이름을 `user`, 암호를 `password`, 역할을 `USER` 지정


<br>


### login.html
``` html
<!DOCTYPE html>
<html
  xmlns="http://www.w3.org/1999/xhtml"
  xmlns:th="https://www.thymeleaf.org"
  xmlns:sec="https://www.thymeleaf.org/thymeleaf-extras-springsecurity3">
  <head>
    <meta charset="UTF-8" />
    <title>Spring Security Example</title>
  </head>
  <body>
    <div th:if="${param.error}">
        Invalid username and password
    </div>
    <div th:if="${param.logout}">
        You have been logged out.
    </div>
    <form th:action="@{login}" method="post">
      <div>
        <label> UserName: <input type="text" name="username" /></label>
      </div>
      <div>
        <label> Password: <input type="password" name="password" /></label>
      </div>
      <div>
        <input type="submit" value="Sign In" />
      </div>
    </form>
  </body>
</html>
```
- Spring Security는 해당 요청을 가로채 사용자를 인증하는 필터를 제공
- 사용자가 인증에 실패하면 `/login?error`로 리다이렉트되고, 페이지에 오류 메시지 출력
- 로그아웃에 성공하면 `/login?logout`으로 보내 로그아웃 메시지 출력


<br>


### hello.html
``` html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="https://www.thymeleaf.org"
      xmlns:sec="https://www.thymeleaf.org/thymeleaf-extras-springsecurity3">
<head>
    <meta charset="UTF-8">
    <title>Hello World!</title>
</head>
<body>
    <h1 th:inline="text">Hello [[${#httpServletRequest.remoteUser}]]!</h1>
    <form th:action="@{/logout}" method="post">
        <input type="submit" value="Sign Out" />
    </form>
</body>
</html>
```
- **getRemoteUser()**: HTTP 기본 인증의 경우 사용자 ID 반환
- 로그아웃에 성공하면 `/login?logout`으로 리다이렉트


<br>


## Run the Application
``` java
package com.example.securingweb;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/* Run the Application */
@SpringBootApplication
public class SecuringwebApplication {

	public static void main(String[] args) {
		SpringApplication.run(SecuringwebApplication.class, args);
	}

}
```

### Build an executable JAR
- gradle을 사용한다면 `./gradlew bootRun`으로 애플리케이션을 실행할 수 있다.

- 또는 `./gradlew build`로 jar 파일 빌드하고 다음과 같이 jar 파일 실행

```
java -jar /build/libs/securingweb-0.0.1-SNAPSHOT.jar
```

<br>

- 브라우저에 `http://localhost:8080` 입력

![home](https://user-images.githubusercontent.com/89443479/168470914-26b4a212-9018-4320-9a8c-882c3639cfb6.png)


- 링크를 클릭하면 `/hello`로 이동
- 해당 페이지는 보안되어 있고 아직 로그인하지 않았으므로 로그인 페이지로 이동

![hello](https://user-images.githubusercontent.com/89443479/168470889-928b72c6-808b-4b05-b3d6-ad13687df2dc.png)

- 로그인 페이지에서 사용자 이름 `user` 암호 `password` 입력
- 로그인 양식을 제출하면 인사말 페이지로 이동

![login](https://user-images.githubusercontent.com/89443479/168470902-f7816e72-7594-40c6-b098-d607e563dacd.png)

- 그 외에 다른 사용자 이름과 암호를 입력하게 되면 메시지를 출력

![invalid](https://user-images.githubusercontent.com/89443479/168470910-cd97973f-ef8a-409f-9596-25b8e935d3c9.png)

- 로그아웃 버튼을 클릭하면 인증이 취소되고 로그아웃되었음을 알리는 메시지와 함께 로그인 페이지로 돌아간다.

![logout](https://user-images.githubusercontent.com/89443479/168470908-40dbbe00-2593-4706-8521-491166f516ca.png)
