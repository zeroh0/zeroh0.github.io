---
title: "[hello-spring] 10. 회원도메인과 리포지토리 테스트"
excerpt: "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술"

categories:
    - java
tags:
    - [java]

toc: false
toc_sticky: false

date: 2021-12-14
last_modified_at: 2021-12-14
---

## 회원 리포지토리 테스트 케이스 작성
개발한 기능을 실행해서 테스트할 때 자바의 main 메서드를 통해서 실행하거나, 웹 어플리케이션의 컨트롤러를 통해서  
해당 기능을 실행한다. 이러한 방법은 준비하고 실행하는데 오래 걸리고, 반복 실행하기 어렵고 여러 테스트를 한 번에  
실행하기 어렵다는 단점이 있다. 자바는 JUnit이라는 프레임워크로 테스트를 실행해서 이러한 문제를 해결한다.  

**회원 리포지토리 메모리 구현체 테스트**
###### test / java / hello.hellospring.repository.MemoryMemberRepositoryTest
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Test;

import java.util.List;

import static org.assertj.core.api.Assertions.*;

class MemoryMemberRepositoryTest {

    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach() {
        repository.clearStore();
    }

    @Test
    public void save() {
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get(); // Optional의 값을 빼내는 방법은 get() 이지만 바로 get()을 사용하는 건 좋은 방법은 아니라고 한다.
        /* 검증 */
        // System.out.println("result = " + (result == member));
        // Assertions.assertEquals(member, result);
        assertThat(member).isEqualTo(result); 
    }

    @Test
    public void findByName() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result =  repository.findByName("spring1").get();

        assertThat(result).isEqualTo(member1);
    }

    @Test
    public void findAll() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        List<Member> result = repository.findAll();

        assertThat(result.size()).isEqualTo(2);
    }

    public void clearStore() {
      store.clear();
    }
}
```
@Test: 테스트로 실행할 수 있다.  

**테스트는 순서는 보장이 안된다. 모든 테스트는 순서랑 상관없이 메소드별로 따로 설계해야 된다.(순서에 의존적 x)**  
테스트가 끝나면 데이터를 클리어해주어야 함!  
```java
@AfterEach
public void afterEach() {
    repository.clearStore();
}
```
@AfterEach: 메소드가 실행이 끝날 때마다 어떤 동작을 하는 콜백 메소드  

```java
public void clearStore() {
    store.clear();
}
```
테스트가 실행이 되고 끝날 때마다 한 번씩 리포지토리(저장소)를 클리어  

지금 진행한건 **메모리 리포지토리 개발 -> 테스트 작성**  

**테스트 작성 -> 메모리 리포지토리 개발**    
미리 검증할 수 있는 틀을 만들어두고 작품이 완성되면 검증 : `테스트 주도 개발 (TDD)`