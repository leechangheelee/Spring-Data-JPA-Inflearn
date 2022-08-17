## **프로젝트 환경설정**

![image](https://user-images.githubusercontent.com/79301439/185174175-5e9d3ec2-5032-4f90-a5b4-e4e0d263dde8.png)

```yaml
spring:
  datasource:
    url: jdbc:h2:tcp://localhost/~/datajpa
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        #show_sql: true
        format_sql: true
logging.level:
  org.hibernate.SQL: debug
#  org.hibernate.type: trace
```

![image](https://user-images.githubusercontent.com/79301439/185176696-23719fec-eea2-4204-aca3-17a6a7412aad.png)

![image](https://user-images.githubusercontent.com/79301439/185174707-8d5cbad7-4935-42b0-ac85-782cf307ff08.png)

![image](https://user-images.githubusercontent.com/79301439/185174761-f24c7225-44e7-433f-aeb6-82b2530e97ec.png)

```java
package study.datajpa.entity;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue
    private Long id;
    private String username;

    protected Member() {
    }

    public Member(String username) {
        this.username = username;
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/185174967-c7bda925-a9e5-4cd8-9f9e-73838b507bb8.png)

```java
package study.datajpa.repository;

import org.springframework.stereotype.Repository;
import study.datajpa.entity.Member;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

@Repository
public class MemberJpaRepository {

    @PersistenceContext
    private EntityManager em;

    public Member save(Member member) {
        em.persist(member);
        return member;
    }

    public Member find(Long id) {
        return em.find(Member.class, id);
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/185175112-d7a86dbc-1aab-44dc-aa55-4845b2a2c3a6.png)

```java
package study.datajpa.repository;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.annotation.Rollback;
import org.springframework.transaction.annotation.Transactional;
import study.datajpa.entity.Member;

import static org.assertj.core.api.Assertions.*;

@SpringBootTest
@Transactional
@Rollback(false) //테스트 코드 실행 후 롤백 안함 --> 커밋 되어서 DB에 반영되어버림
class MemberJpaRepositoryTest {

    @Autowired MemberJpaRepository memberJpaRepository;

    @Test
    public void testMember() {
        Member member = new Member("memberA");
        Member savedMember = memberJpaRepository.save(member);

        Member findMember = memberJpaRepository.find(savedMember.getId());

        assertThat(findMember.getId()).isEqualTo(member.getId());
        assertThat(findMember.getUsername()).isEqualTo(member.getUsername());
        assertThat(findMember).isEqualTo(member); //JPA 엔티티 동일성 보장
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/185175263-7cce4189-6309-4e05-90bf-7e642c3e4cc3.png)

```java
package study.datajpa.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import study.datajpa.entity.Member;

public interface MemberRepository extends JpaRepository<Member, Long> {
}
```

![image](https://user-images.githubusercontent.com/79301439/185175415-19f875e3-d35e-4e82-b513-6f534765b1a2.png)

```java
package study.datajpa.repository;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.annotation.Rollback;
import org.springframework.transaction.annotation.Transactional;
import study.datajpa.entity.Member;

import static org.assertj.core.api.Assertions.assertThat;

@SpringBootTest
@Transactional
@Rollback(false)
class MemberRepositoryTest {

    @Autowired MemberRepository memberRepository;

    @Test
    public void testMember() {
        Member member = new Member("memberA");
        Member savedMember = memberRepository.save(member);

        Member findMember = memberRepository.findById(savedMember.getId()).get();

        assertThat(findMember.getId()).isEqualTo(member.getId());
        assertThat(findMember.getUsername()).isEqualTo(member.getUsername());
        assertThat(findMember).isEqualTo(member); //JPA 엔티티 동일성 보장
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/185175646-a91335aa-48b9-4e54-8dfc-1016e6550f2f.png)

![image](https://user-images.githubusercontent.com/79301439/185175934-fc9efd3e-2010-481d-815b-7886f64718a4.png)
