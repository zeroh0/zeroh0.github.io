---
title: "Consuming a RESTful Web Service"
excerpt: "spring-guides"

categories:
    - spring
  
tags:
    - [spring]

toc: true
toc_sticky: true

date: 2022-05-20
last_modified_at: 2022-05-21
---


<br>


# Consuming a RESTful Web Service

## What You Will Build
스프링의 `RestTemplate`을 사용하여 <https://zeroh0.github.io/json-server-api/quote.json>에서 스프링부트 인용문을 찾는 애플리케이션을 구축합니다.


<br>


## Starting with Spring Initializr
![start](https://user-images.githubusercontent.com/89443479/169416923-8fe3cf03-1fe7-45ba-8de1-12fddfb5e01a.png)


<br>


## Fetching a REST Resource
프로젝트 설정이 완료되면 RESTful 서비스를 사용하는 간단한 애플리케이션을 만들 수 있습니다.  

RESTful 서비스는 <https://zeroh0.github.io/json-server-api/quote.json>에서 제공됩니다.  
스프링부트에 대한 인용문을 가져와서 JSON 문서로 반환합니다.  

웹 브라우저나 컬 통해 해당 URL을 요청하면 다음과 같은 JSON 문서를 받게 됩니다.  
``` json
{
    "type": "success",
    "value": {
      "id": 10,
      "quote": "Really loving Spring Boot, makes stand alone Spring apps easy."
    }
}
```
이 방법은 쉽지만 브라우저나 컬을 통해 가져올 때는 그다지 유용하지 않다.  

REST 웹 서비스를 소비하는 더 유용한 방법은 프로그래밍 방식이다.  
이 작업은 쉽게 수행할 수 있도록 스프링에서는 `RestTemplate`라는 편리한 템플릿 클래스를 제공합니다.  
`RestTemplate`는 대부분의 Restful 서비스와 상호 작용하는 것을 한 줄 주문으로 만듭니다.  
또한 이 데이터를 사용자 지정 도메인 유형에 바인딩할 수도 있습니다.  

먼저 필요한 데이터를 포함하는 도메인 클래스를 만들어야 합니다.  
다음 목록에는 도메인 클래스로 사용할 수 있는 `Quote` 클래스가 나와 있습니다.  

### Quote.java
``` java
package com.example.consumingrest;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Quote {

    private String type;
    private Value value;

    public Quote() {

    }

    public String getType() {
        return type;
    }

    public void setType(String type) {
        this.type = type;
    }

    public Value getValue() {
        return value;
    }

    public void setValue(Value value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Quote{" +
                "type='" + type + '\'' +
                ", value=" + value +
                '}';
    }

}
```
이 간단한 자바 클래스는 몇 가지 속성과 일치하는 getter 메소드를 가지고 있다.  
Jackson JSON 프로세싱 라이브러리의 `@JSONIgnoreProperties`로 어노테이션을 달아 이 유형에 바인딩되지 않는 속성은 무시되어야 함을 나타낸다.  


데이터를 사용자 정의 유형에 직접 바인딩하려면 변수 이름을 API에서 반환된 JSON 문서와 키와 정확히 동일하게 지정해야 합니다.  
JSON 문서의 변수 이름과 키가 일치하지 않는 경우 `@JsonProperty`를 사용할 수 있습니다.  
어노테이션은 JSON 문서의 정확한 키를 지정합니다.(이 예에서는 각 변수 이름을 JSON 키와 일치시키므로 여기서는 어노테이션을 달 필요가 없습니다.)  

또한 내부 따옴표 자체를 포함하려면 추가 클래스가 필요합니다.  
`Value` 클래스는 해당 니즈를 충족하며 다음 목록에 표시됩니다.  

### Value.java
``` java
package com.example.consumingrest;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Value {

    private Long id;
    private String quote;

    public Value() {

    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getQuote() {
        return quote;
    }

    public void setQuote(String quote) {
        this.quote = quote;
    }

    @Override
    public String toString() {
        return "Value{" +
                "id=" + id +
                ", quote='" + quote + '\'' +
                '}';
    }

}
```
동일한 어노테이션을 사용하지만 다른 데이터 필드에 매핑됩니다.  


<br>


## Finishing the Application
Initializer는 `main()` 메서드를 사용하여 클래스를 만듭니다.   
다음 목록은 Initializer가 만드는 클래스를 보여줍니다.  

### ConsumingrestApplication.java
``` java
package com.example.consumingrest;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ConsumingRestApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConsumingRestApplication.class, args);
	}

}
```
이제 RESTful 소스에서 인용문을 표시하려면 `ConsumingRestApplication` 클래스에 몇 가지 다른 항목을 추가합니다.
- 로거: 출력을 로그로 보냅니다.(이 예에서는 콘솔)
- `RestTemplate`: Jackson JSON 처리 라이브러리를 사용하여 수신 데이터를 처리합니다.
- 시작할 때 `RestTemplate`를 실행하고 결과적으로 견적을 가져오는 `CommandLineRunner`  

다음 목록은 완료된 `ConsumingRestApplication` 클래스를 보여줍니다.  
``` java
package com.example.consumingrest;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
public class ConsumingrestApplication {

	private static final Logger log = LoggerFactory.getLogger(ConsumingrestApplication.class);

	public static void main(String[] args) {
		SpringApplication.run(ConsumingrestApplication.class, args);
	}

	@Bean
	public RestTemplate restTemplate(RestTemplateBuilder builder) {
		return builder.build();
	}

	@Bean
	public CommandLineRunner run(RestTemplate restTemplate) throws Exception {
		return args -> {
			Quote quote = restTemplate.getForObject("https://zeroh0.github.io/json-server-api/quote.json", Quote.class);
			log.info(quote.toString());
		};
	}

}
```


<br>


## Running the Application
- gradle을 사용한다면 `./gradlew bootRun`으로 애플리케이션을 실행할 수 있다.
- 또는 `./gradlew build`로 jar 파일 빌드하고 다음과 같이 jar 파일 실행

```
java -jar build/libs/consumingrest-0.0.1-SNAPSHOT.jar
```

출력은 다음과 같다
```
2022-05-21 20:27:57.233  INFO 48561 --- [           main] c.e.c.ConsumingrestApplication           : Quote{type='success', value=Value{id=10, quote='Really loving Spring Boot, makes stand alone Spring apps easy.'}}
```