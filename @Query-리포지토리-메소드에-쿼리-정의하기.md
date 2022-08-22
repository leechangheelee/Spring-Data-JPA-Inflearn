## **쿼리 메소드 기능**

![image](https://user-images.githubusercontent.com/79301439/185927919-b59c0171-d82a-4b88-89d1-993013ca97f5.png)

```java
package study.datajpa.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import study.datajpa.entity.Member;

import java.util.List;

public interface MemberRepository extends JpaRepository<Member, Long> {

    @Query("select m from Member m where m.username = :username and m.age = :age")
    List<Member> findUser(@Param("username") String username, @Param("age") int age);
    
}
```

![image](https://user-images.githubusercontent.com/79301439/185928123-986c6353-6269-4eae-9229-d381b93934a9.png)
