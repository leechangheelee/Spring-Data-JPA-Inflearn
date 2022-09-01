## **쿼리 메소드 기능**

![image](https://user-images.githubusercontent.com/79301439/187881641-ca84b7bb-652e-4ccc-b792-c8d00471894b.png)

![image](https://user-images.githubusercontent.com/79301439/187881673-3ab1c40a-bb82-46a8-a8ea-46d9989cf95a.png)

```java
...
public interface MemberRepository extends JpaRepository<Member, Long> {
    ...
    
    @QueryHints(value = @QueryHint(name = "org.hibernate.readOnly", value = "true"))
    Member findReadOnlyByUsername(String username);
}
```

![image](https://user-images.githubusercontent.com/79301439/187881928-3a200c4d-e655-4180-a3a0-4e2a6abdbf0b.png)


```java
@Test
public void queryHint() {
    //given
    Member member1 = new Member("member1", 10);
    memberRepository.save(member1);
    em.flush();
    em.clear();

    //when
    Member findMember = memberRepository.findReadOnlyByUsername("member1");
    findMember.setUsername("member2");

    em.flush(); //Update Query 실행X
}
```

![image](https://user-images.githubusercontent.com/79301439/187882199-3c624b79-39aa-4e62-81fc-63f55ac5206f.png)

```java
@QueryHints(value = { @QueryHint(name = "org.hibernate.readOnly", value = "true")}, forCounting = true)
Page<Member> findByUsername(String name, Pagable pageable);
```

![image](https://user-images.githubusercontent.com/79301439/187882446-aaf70784-6f1b-4175-8fe3-748f70b70ec3.png)

![image](https://user-images.githubusercontent.com/79301439/187882765-de8a8a6f-823c-49f3-a8d4-c997a27bf780.png)

```java
...
public interface MemberRepository extends JpaRepository<Member, Long> {
    ...
    
    @Lock(LockModeType.PESSIMISTIC_WRITE)
    List<Member> findLockByUsername(String username);
}
```

![image](https://user-images.githubusercontent.com/79301439/187882942-29a32f50-839a-466a-b753-cc3506c27b8a.png)

```java
@Test
public void lock() {
    //given
    Member member1 = new Member("member1", 10);
    memberRepository.save(member1);
    em.flush();
    em.clear();

    //when
    List<Member> result = memberRepository.findLockByUsername("member1");
}
```

![image](https://user-images.githubusercontent.com/79301439/187883116-55fcd7d1-d59b-497e-b276-06358f5da852.png)
