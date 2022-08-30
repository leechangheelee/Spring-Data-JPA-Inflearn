## **쿼리 메소드 기능**

![image](https://user-images.githubusercontent.com/79301439/187449292-35252632-814e-480f-af03-1bd0304ef9b2.png)

```java
...

@Repository
public class MemberJpaRepository {

    ...

    public List<Member> findByPage(int age, int offset, int limit) {
        return em.createQuery("select m from Member m where m.age = :age order by m.username desc")
                .setParameter("age", age)
                .setFirstResult(offset) //몇 번째부터
                .setMaxResults(limit) //몇 개
                .getResultList();
    }

    public long totalCount(int age) {
        return em.createQuery("select count(m) from Member m where m.age = :age", Long.class)
                .setParameter("age", age)
                .getSingleResult();
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/187449601-15fa1876-84ad-4c99-8112-18fcc5d77ab5.png)

```java
...
class MemberJpaRepositoryTest {

    ...

    @Test
    public void paging() {
        //given
        memberJpaRepository.save(new Member("member1", 10));
        memberJpaRepository.save(new Member("member2", 10));
        memberJpaRepository.save(new Member("member3", 10));
        memberJpaRepository.save(new Member("member4", 10));
        memberJpaRepository.save(new Member("member5", 10));

        int age = 10;
        int offset = 0;
        int limit = 3;

        //when
        List<Member> members = memberJpaRepository.findByPage(age, offset, limit);
        long totalCount = memberJpaRepository.totalCount(age);

        //페이지 계산 공식 적용...
        // totalPage = totalCount / size ...
        // 마지막 페이지 ...
        // 최초 페이지 ..

        //then
        assertThat(members.size()).isEqualTo(3);
        assertThat(totalCount).isEqualTo(5);
    }
}
```
