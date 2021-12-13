---
title: "[hello-spring] 09. 회원도메인과 리포지토리 만들기"
excerpt: "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술"

categories:
    - java
tags:
    - [java]

toc: false
toc_sticky: false

date: 2021-12-13
last_modified_at: 2021-12-14
---

##### 회원 객체
###### hello.hellospring.domain.Member
```java
package hello.hellospring.domain;

public class Member {
    private Long id; // 임의의 값, 시스템이 저장하는 ID
    private String name; // 이름

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

##### 회원 리포지토리(저장소) 인터페이스
###### hello.hellospring.repository.MemberRepository
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    Member save(Member member); // 회원 저장
    /* Optional: null 처리하는 방법 중 하나  */
    Optional<Member> findById(Long id); // id로 회원 찾기
    Optional<Member> findByName(String name);
    List<Member> findAll(); // 모든 회원 리스트 반환
}
```

##### 구현체
###### hello.hellospring.repository.MemoryMemberRepository
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;

import java.util.*;

public class MemoryMemberRepository implements MemberRepository{

    private static Map<Long, Member> store = new HashMap<>(); // 메모리에 저장, 동시적 문제가 있을 수 있어서 공유되는 변수일 때는 ConcurrentHashMap을 사용하지만 단순하게 할거라
    private static long sequence = 0L; // 키 값을 생성, 동시적 문제를 고려해서 AtomicLong을 사용하지만 단순하게 할거라

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id)); // null이 반환될 가능성이 있을 때 Optional
    }

    @Override
    public Optional<Member> findByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }

    @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }
    
}
```
