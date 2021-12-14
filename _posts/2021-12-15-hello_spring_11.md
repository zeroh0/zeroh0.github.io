---
title: "[hello-spring] 11. 회원서비스 개발"
excerpt: "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술"

categories:
    - java
tags:
    - [java]

toc: false
toc_sticky: false

date: 2021-12-15
last_modified_at: 2021-12-15
---

회원 서비스는 회원 리포지토리와 도메인을 활용해서 실제 비즈니스 로직을 작성  

###### hello.hellospring.service.MemberService
```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;

import java.util.List;
import java.util.Optional;

public class MemberService {

    private final MemberRepository memberRepository = new MemoryMemberRepository();

    /**
     * 회원 가입 
     */
    public Long join(Member member) {
        validateDuplicateMember(member); // 중복 회원 검증
        memberRepository.save(member); // 저장
        return member.getId();
    }

    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                .ifPresent(m -> {
                    throw new IllegalStateException("이미 존재하는 회원입니다.");
                });
    }

    /**
     * 전체 회원 조회
     * 서비스 클래스는 비즈니스에 가까운 용어를 써야한다. 비즈니스에 의존적으로 설계
     */
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId) {
        return memberRepository.findById(memberId);
    }

}
```

<br>

```java
Optional<Member> result = memberRepository.findByName(member.getName());
Member member1 = result.get();
result.ifPresent(m -> {
    throw new IllegalStateException("이미 존재하는 회원입니다."); // null이 아닌 어떤 값이 있는 경우 (Optional이기 때문에 가능)
```
**Optional을 바로 반환하는 건 권장하지 않는다.**    
❌ &nbsp;Optional&lt;Member&gt; result  
❌ &nbsp;result.get()  

`orElseGet()`: 값이 있으면 꺼내고 값이 없으면 어떤 메소드를 실행 or default 값을 넣어서 꺼냄  

```java
private void validateDuplicateMember(Member member) {
    memberRepository.findByName(member.getName())
        .ifPresent(m -> {
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        });
}
```
`ctrl + t` 단축키로 Extra Method(`shift + cmd + m`)로 메서드를 생성해서 바로 반환하지 않고 명령문을 한 번에 적어준다.

<br>

```java
public List<Member> findMembers() {
        return memberRepository.findAll();
}
```
전체 회원을 조회하는 메서드