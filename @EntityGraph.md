## **쿼리 메소드 기능**

![image](https://user-images.githubusercontent.com/79301439/187687798-9cb9b10e-a99b-42d6-b2ff-d918385c5a5d.png)

```java
@Test
public void findMemberLazy() throws Exception {
    //given
    //member1 -> teamA
    //member2 -> teamB
    Team teamA = new Team("teamA");
    Team teamB = new Team("teamB");
    teamRepository.save(teamA);
    teamRepository.save(teamB);
    memberRepository.save(new Member("member1", 10, teamA));
    memberRepository.save(new Member("member2", 20, teamB));
    
    em.flush();
    em.clear();
    
    //when
    List<Member> members = memberRepository.findAll();
    
    //then
    for (Member member : members) {
        member.getTeam().getName();
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/187688040-8ef6345c-7cdd-4e26-9c72-1dbc440c7c37.png)

```java
//Hibernate 기능으로 확인
Hibernate.isInitialized(member.getTeam());

//JPA 표준 방법으로 확인
PersistenceUnitUtil util =
em.getEntityManagerFactory().getPersistenceUnitUtil();
util.isLoaded(member.getTeam());
```

![image](https://user-images.githubusercontent.com/79301439/187688228-33805a16-93cd-441e-9a22-12bd31921bf0.png)

```java
...
public interface MemberRepository extends JpaRepository<Member, Long> {
    ...
    
    @Query("select m from Member m left join fetch m.team")
    List<Member> findMemberFetchJoin();
}
```

![image](https://user-images.githubusercontent.com/79301439/187688831-9739f38f-fefe-4275-89a1-547b1adcb691.png)

![image](https://user-images.githubusercontent.com/79301439/187688889-9ed29ae1-f3dd-4658-bc96-8bb7daece206.png)

```java
...
public interface MemberRepository extends JpaRepository<Member, Long> {
    ...
    
    //공통 메서드 오버라이드
    @Override
    @EntityGraph(attributePaths = {"team"})
    List<Member> findAll();
    
    //JPQL + 엔티티 그래프
    @EntityGraph(attributePaths = {"team"})
    @Query("select m from Member m")
    List<Member> findMemberEntityGraph();
    
    //메서드 이름으로 쿼리에서 특히 편리하다.
    @EntityGraph(attributePaths = {"team"})
    List<Member> findByUsername(String username);
}
```

![image](https://user-images.githubusercontent.com/79301439/187689439-199ee7bf-baae-4cb6-9bcf-ea8b4da78204.png)

```java
@NamedEntityGraph(name = "Member.all", attributeNodes = @NamedAttributeNode("team"))
@Entity
public class Member {}
```

```java
...
public interface MemberRepository extends JpaRepository<Member, Long> {
    ...
    
    @EntityGraph("Member.all")
    List<Member> findEntityGraphByUsername(@Param("username") String username);
}
```

```java
...
class MemberRepositoryTest {
    ...
    
    @Test
    public void findMemberLazy() {
        //given
        //member1 -> teamA
        //member2 -> teamB

        Team teamA = new Team("TeamA");
        Team teamB = new Team("TeamB");
        teamRepository.save(teamA);
        teamRepository.save(teamB);
        Member member1 = new Member("member1", 10, teamA);
        Member member2 = new Member("member2", 10, teamB);
        memberRepository.save(member1);
        memberRepository.save(member2);

        em.flush();
        em.clear();

        List<Member> members = memberRepository.findEntityGraphByUsername("member1");

        for (Member member : members) {
            System.out.println("member = " + member.getUsername());
            System.out.println("member.teamClass = " + member.getTeam().getClass());
            System.out.println("member.team = " + member.getTeam().getName());
        }
    }
}
```
