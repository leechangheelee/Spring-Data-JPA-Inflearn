## **나머지 기능들**

![image](https://user-images.githubusercontent.com/79301439/188312403-56aaeda6-a524-4e3c-bb44-bfab28dbdf2a.png)

![image](https://user-images.githubusercontent.com/79301439/188312483-bf1dc4a2-e7a9-4ab9-9c79-c593c28da4e1.png)

```java
@Test
public void projections() throws Exception {
    //given
    Team teamA = new Team("teamA");
    em.persist(teamA);

    Member m1 = new Member("m1", 0, teamA);
    Member m2 = new Member("m2", 0, teamA);
    em.persist(m1);
    em.persist(m2);
    em.flush();
    em.clear();

    //when
    List<UsernameOnly> result = memberRepository.findProjectionsByUsername("m1");

    //then
    Assertions.assertThat(result.size()).isEqualTo(1);
}
```

```sql
select m.username from member m
where m.username='m1';
```

![image](https://user-images.githubusercontent.com/79301439/188312552-19237876-1ce5-4dfc-8fe5-c08046cedfbb.png)

![image](https://user-images.githubusercontent.com/79301439/188312578-f5ac62ce-51de-4902-893a-25426214d2a7.png)

```java
public interface UsernameOnly {
    String getUsername();
}
```

![image](https://user-images.githubusercontent.com/79301439/188312603-8aa04426-f8c4-44e9-83ad-a438c371920b.png)

```java
public interface UsernameOnly {

    @Value("#{target.username + ' ' + target.age + ' ' + target.team.name}")
    String getUsername();
}
```

![image](https://user-images.githubusercontent.com/79301439/188312644-649575a6-8085-413c-afda-271606d94cac.png)

```java
public class UsernameOnlyDto {

    private final String username;

    public UsernameOnlyDto(String username) {
        this.username = username;
    }

    public String getUsername() {
        return username;
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/188312678-50f0772d-66a6-49b3-8bd4-8c00c7565114.png)

```java
...
public interface MemberRepository extends JpaRepository<Member, Long> {
    ...
    
    <T> List<T> findProjectionsByUsername(String username, Class<T> type);
}
```

![image](https://user-images.githubusercontent.com/79301439/188312720-ed6a5aba-8e07-4238-8d35-f8bf9428bc9e.png)

```java
List<UsernameOnly> result = memberRepository.findProjectionsByUsername("m1", UsernameOnly.class);
```

![image](https://user-images.githubusercontent.com/79301439/188312769-18b922c2-a385-48a7-8641-49ef2b8cd4be.png)

```java
public interface NestedClosedProjection {

    String getUsername();
    TeamInfo getTeam();

    interface TeamInfo {
        String getName();
    }
}
```

```sql
select
    member0_.username as col_0_0_,
    team1_.team_id as col_1_0_,
    team1_.team_id as team_id1_2_,
    team1_.name as name2_2_ 
from
    member member0_ 
left outer join
    team team1_ 
        on member0_.team_id=team1_.team_id 
where
    member0_.username=?
```

![image](https://user-images.githubusercontent.com/79301439/188312854-1779ccdf-7752-4806-9c87-2149914fdb6c.png)
