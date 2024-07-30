---
title: 🍃 SpringBoot - User
date: 2024-07-20 00:00:00 +0800
toc: true
pin: false
published: false
categories: [SERVER, SPRINGBOOT]
tags: [springboot]
image: https://github.com/user-attachments/assets/a2dc1a88-2c9b-4d86-92da-1f37f30378f9
---

<br>

---

<br>

🌏 자바공화국에서 살아남기 ch.2

> ## Feat : User

### 비즈니스 요구사항 정리

- Data : `UserId`, `name`
- Feat : `CreateUser`, `getUser`

![image](https://github.com/user-attachments/assets/9331023b-ad89-410a-bc05-7278684d9d42)

- **Controller** : 웹 MVC의 컨트롤러 역할, 클라이언트가 이용할 엔드포인트
- **Service** : 핵심 비즈니스 로직 구현, DB와 Domain영역을 연결해주는 매개체
- **Repositroy** : DB 접근, 도메인 객체를 DB에 저장하고 관리, Domain의 CRUD역할 (DAO)
- **Domain** : Entity(DB 테이블과 맵핑되는 객체) 클래스라고도함, 로직을 갖지 않고 `getter`/`setter` 메소드를 가짐 (DTO)

<br>

### 회원 도메인과 리포지토리 생성

#### 📟 `domain/Member.java`

Member 클래스 정의 (필드 정의 및 반환)

```java
package helloGroup.hello_spring.domain;

public class Member {
    private Long id;
    private String name;

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

<br>

#### 📟 `reposiory/MemberRepository.java`

```java
package helloGroup.hello_spring.repository;

import helloGroup.hello_spring.domain.Member;

import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    Member save(Member member);

    Optional<Member> findById(Long id);
    Optional<Member> findByName(String name);

    List<Member> findAll();
}
```

- `MemeberRepository` 인터페이스는 Member Entity에 대한 DB 접근 계층 정의, CRUD 작업 진행
- `save(Member member)`는 주어진 회원 객체를 DB에 저장 혹은 업데이트 진행
- `Optional` 은 Member를 찾지 못하면 null 값을 리턴함

<br>

#### 📟 `reposiory/MemoryMemberRepository.java`

```java
package helloGroup.hello_spring.repository;

import helloGroup.hello_spring.domain.Member;

import java.util.*;

public class MemoryMemberRepository implements MemberRepository {

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
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


참고: `MemberRepository` 클릭 후 `alt + Enter` 하면 Implement Method들을 자동으로 가져올 수 있음.

<br>

### 회원 리포지토리 테스트 케이스

개발한 기능을 테스트할 때 Java의 `main` 메서드를 통해 실행하거나, 웹 어플리케이션의 컨트롤러를 통해서 해당 기능을 실행. 이러한 방법은 준비하고 실행하기 까지 어렵고 여러가지 테스트를 한 번에 하기 힘듦. 

그러므로, Java에서는 🔔 `Junit`이라는 프레임워크 사용하여 테스트 진행

#### 📟 `test/../repsoitory/MemoryMemberRepositoryTest.java`

테스트 케이스를 작성할 때는 **given**(어떤상황에서), **when**(~일 때), **then**(~이어야한다)으로 기준을 나누면 좋음

```java
    @Test
    public void save() {
        // given
        Member member = new Member();   // 새로운 Member 객체 생성
        member.setName("JngSu");        // 생성한 Member 객체의 이름 설정
        repository.save(member);        // Member 객체를 저장소에 저장

        // when
        Member result = repository.findById(member.getId()).get();

        // then
        // import static org.assertj.core.api.Assertions.*;
        assertThat(member).isEqualTo(result);
    }
```

<br>

```java
    @Test
    public void findByName() {
        // given
        Member member1 = new Member();
        member1.setName("name1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("name2");
        repository.save(member2);

        // when
        Member result = repository.findByName("name1").get();

        // then
        assertThat(result).isEqualTo(member1);
    }
```

<br>

```java
    @Test
    public void findAll() {
        Member member1 = new Member();
        member1.setName("deadpool");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("wolverene");
        repository.save(member2);

        List<Member> result = repository.findAll();
        assertThat(result.size()).isEqualTo(2);
    }
```

<br>

하지만, 테스트 케이스는 순서에 독립적으로 작성되어야하기 때문에 위 처럼은 아니지만 동명이인이 추가되면 찾고자하는 `name`과 중복되어 에러가 발생할 수 있음.
그러므로 `@AfterEach`를 사용하여 매번 테스트 케이스가 종료 시 store를 비워줘야함.

<br>

- 📟 `MemoryMemberRepository.java`

```java
    public void clearStore() {
        store.clear();
    }
```

- 📟 `MemoryMemberRepositoryTest.java`

```java
    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach() {
        repository.clearStore();
        System.out.println("----- Clear Store -----");
    }
```

![fxxingResult](https://github.com/user-attachments/assets/c23a7a5a-ec66-47c2-a6c9-1697590925fb){: width="50%" height"100%" .normal}


<br>

**전체 코드**

```java
package helloGroup.hello_spring.repository;

import helloGroup.hello_spring.domain.Member;
import static org.assertj.core.api.Assertions. *;

import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.Test;

import java.util.List;
import java.util.Optional;

public class MemoryMemberRepositoryTest {

    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach
    public void afterEach() {
        repository.clearStore();
        System.out.println("----- Clear Store -----");
    }

    @Test
    public void save() {
        Member member = new Member();
        member.setName("JngSu");
        repository.save(member);

        Member result = repository.findById(member.getId()).get();
        assertThat(member).isEqualTo(result);
        System.out.println("Test1 success");
    }

    @Test
    public void findByName() {
        Member member1 = new Member();
        member1.setName("name1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("name2");
        repository.save(member2);

        Member result = repository.findByName("name1").get();

        assertThat(result).isEqualTo(member1);
        System.out.println("Test2 success");
    }

    @Test
    public void findAll() {
        Member member1 = new Member();
        member1.setName("deadpool");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("wolverene");
        repository.save(member2);

        List<Member> result = repository.findAll();

        assertThat(result.size()).isEqualTo(2);
        System.out.println("Test3 success");
    }
}
```

<br>

### 회원 서비스 개발

서비스는 레포지토리와 도메인을 활용하여 실제 비즈니스 로직을 처리함

<br>

회원 서비스를 제작하기 위해 사용될 기능은 회원가입, 모든 회원 불러오기, 한 회원 불러오기
우선 Sevice Package를 생성하고 MemberService 클래스를 생성


#### 📟 `service/MemberService.java`

```java
    // 회원 서비스 자체가 회원 레포지토리를 활용
    private final MemberRepository memberRepository = new MemoryMemberRepository();

    public Long join(Member member) {
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }
    
    private void validateDuplicateMember(Member member) {
        memberRepository.findByName(member.getName())
                        .ifPresent(m -> {
                            throw new IllegalStateException("이미 존재하는 회원입니다.");
                        });
    }

    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    public Optional<Member> findOne(Long memberId) {
        return memberRepository.findById(memberId);
    }
```

- `validateDuplicateMember()`는 method로 따로 관리하는 게 좋기에 `ctrl + t` 로 분리하고 Extract Method를 선택
- `ifPresent()`는 실제 값이 존재하면 실행되는 데 이는 `Optional`이기에 가능함

<br>

### 회원 서비스 테스트

이제 작성한 회원 서비스를 테스트를 해야함.

`command + shift + t`로 테스트를 자동완성 시키고 Junit5를 사용

![image](https://github.com/user-attachments/assets/3a5ac0e3-a641-4c65-9058-f75d60178bb3){: .normal}

<br>

![image](https://github.com/user-attachments/assets/af4e4ef4-a3fc-409b-a244-c5b0713be9f0){: .normal}

보이는 것 처럼 본인은 맥북을 사용함. 구매한지 얼마 안됨. 💻 

무튼, 테스트하고자 하는 Method를 지정하고 OK로 생성

<br>

#### 📟 `test/../service/MemberServiceTest.java`

```java
package helloGroup.hello_spring.service;

import helloGroup.hello_spring.domain.Member;
import helloGroup.hello_spring.repository.MemoryMemberRepository;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

class MemberServiceTest {

    MemberService memberService;
    MemoryMemberRepository memberRepository;

    @BeforeEach
    public void beforeEach() {
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }

    @AfterEach
    public void afterEach() {
        memberRepository.clearStore();
    }

    @Test
    void join() {
        // given
        Member member = new Member();
        member.setName("spiderman");

        // when
        Long saveId = memberService.join(member);

        // then
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }

    @Test
    public void ExceptDuplicatedMember() {
        // given
        Member member1 = new Member();
        member1.setName("dr.strange");

        Member member2 = new Member();
        member2.setName("dr.strange");

        // when
        memberService.join(member1);
                IllegalStateException e = assertThrows(
                IllegalStateException.class, 
                () -> memberService.join(member2)
        );
        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");

        // then
    }

    @Test
    void findMembers() {
    }

    @Test
    void findOne() {
    }
}
```

<br>

#### Dependency Injection

1. MemoryService 클래스에서 new 키워드를 사용하여 MemoryMemberRepository의 인스턴스를 생성
    ```java
    MemoryMemberRepository memberRepository = new MemoryMemberRepository();
    ```
    이렇게 하면 MemoryService 클래스가 항상 새로운 MemoryMemberRepository 인스턴스를 사용
    

2. MemoryServiceTest 클래스에서는 MemberService의 인스턴스를 생성할 때, MemoryMemberRepository 인스턴스를 생성자 주입 방식으로 전달
    ```java
    private final MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
    }
    ```
    MemberService 클래스는 의존성을 외부에서 주입


3.  테스트 코드에서는 @BeforeEach 애노테이션을 사용하여 매 테스트 전에 MemoryMemberRepository와 MemberService 인스턴스를 생성하고 연결
    ```java
    MemberService memberService;
    MemoryMemberRepository memberRepository;

    @BeforeEach
    public void beforeEach() {
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }
    ```
    이렇게 하면 각 테스트마다 새로운 MemoryMemberRepository와 MemberService 인스턴스가 생성되고, MemberService는 생성된 MemoryMemberRepository 인스턴스를 사용
