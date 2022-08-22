## **쿼리 메소드 기능**

![image](https://user-images.githubusercontent.com/79301439/185924400-5a610ed8-5dd9-4491-8c1a-ad125f5c0d5a.png)

```java
package study.datajpa.entity;

import lombok.*;

import javax.persistence.*;

@Entity
@Getter @Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@ToString(of = {"id", "username", "age"})
@NamedQuery(
        name="Member.findByUsername",
        query="select m from Member m where m.username = :username"
)
public class Member {

    ...
    
}
```

![image](https://user-images.githubusercontent.com/79301439/185924571-434c1b6c-e581-438c-8b9d-9d12f2f3d327.png)

```java
package study.datajpa.repository;

import org.springframework.stereotype.Repository;
import study.datajpa.entity.Member;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import java.util.List;
import java.util.Optional;

@Repository
public class MemberJpaRepository {

    ...

    public List<Member> findByUsername(String username) {
        return em.createNamedQuery("Member.findByUsername", Member.class)
                .setParameter("username", username)
                .getResultList();
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/185924849-5a3aec89-ef92-4b6d-85e5-247c10db3149.png)

```java
@Query(name = "Member.findByUsername")
List<Member> findByUsername(@Param("username") String username);
```

![image](https://user-images.githubusercontent.com/79301439/185925090-75ff22c9-039b-4da0-8ba6-7359f4a524bb.png)

![image](https://user-images.githubusercontent.com/79301439/185925118-4691cb45-e15f-4d05-8432-ec123166f220.png)

```java
public interface MemberRepository extends JpaRepository<Member, Long> { //** 여기 선언한 Member 도메인 클래스

    List<Member> findByUsername(@Param("username") String username);
}
```

![image](https://user-images.githubusercontent.com/79301439/185925374-e8e91f3e-d77f-420f-a304-3bf85cc0a16a.png)
