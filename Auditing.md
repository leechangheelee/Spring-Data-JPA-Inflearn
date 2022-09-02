## **확장 기능**

![image](https://user-images.githubusercontent.com/79301439/188113393-28a7de6b-03bc-49ff-95c9-d6808598670d.png)

```java
package study.datajpa.entity;

import lombok.Getter;

import javax.persistence.Column;
import javax.persistence.MappedSuperclass;
import javax.persistence.PrePersist;
import javax.persistence.PreUpdate;
import java.time.LocalDateTime;

@Getter
@MappedSuperclass
public class JpaBaseEntity {

    @Column(updatable = false)
    private LocalDateTime createdDate;
    private LocalDateTime updatedDate;

    @PrePersist
    public void prePersist() {
        LocalDateTime now = LocalDateTime.now();
        createdDate = now;
        updatedDate = now;
    }

    @PreUpdate
    public void preUpdate() {
        updatedDate = LocalDateTime.now();
    }
}
```

```java
...
public class Member extends JpaBaseEntity {
    ...
}
```

![image](https://user-images.githubusercontent.com/79301439/188113772-bfa5f253-685d-4113-bce7-95d0ab133e5e.png)

```java
...
class MemberTest {
    ...
    
    @Test
    public void JpaEventBaseEntity() throws Exception {
        //given
        Member member = new Member("member1");
        memberRepository.save(member); //@PrePersist

        Thread.sleep(100);
        member.setUsername("member2");

        em.flush(); //@PreUpdate
        em.clear();

        //when
        Member findMember = memberRepository.findById(member.getId()).get();

        //then
        System.out.println("findMember.getCreatedDate = " + findMember.getCreatedDate());
        System.out.println("findMember.getUpdatedDate = " + findMember.getLastModifiedDate());
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/188114153-0c9b0a75-b42d-4f07-9384-82c315e6bbaa.png)

```java
...
@EnableJpaAuditing
@SpringBootApplication
public class DataJpaApplication {
    ...
}
```

![image](https://user-images.githubusercontent.com/79301439/188114474-2e03d9ea-4740-4886-b544-dab28375603e.png)

![image](https://user-images.githubusercontent.com/79301439/188114549-d9ba09d9-170c-4b7e-b972-006cd4b0df10.png)

```java
package study.datajpa.entity;

@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
@Getter
public class BaseEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime lastModifiedDate;

}
```

![image](https://user-images.githubusercontent.com/79301439/188114775-fc8dccab-2703-4a2c-a692-7e88b2edcda4.png)

```java
package study.datajpa.entity;

@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
public class BaseEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdDate;
    @LastModifiedDate
    private LocalDateTime lastModifiedDate;

    @CreatedBy
    @Column(updatable = false)
    private String createdBy;
    @LastModifiedBy
    private String lastModifiedBy;

}
```

```java
...
public class Member extends BaseEntity {
    ...
}
```

![image](https://user-images.githubusercontent.com/79301439/188115115-dcf90473-8097-4c86-9e8d-570a17c8f3b6.png)

```java
...
@EnableJpaAuditing
@SpringBootApplication
public class DataJpaApplication {
    ...
    
    @Bean
	public AuditorAware<String> auditorProvider() {
		return () -> Optional.of(UUID.randomUUID().toString());
	}
}
```

![image](https://user-images.githubusercontent.com/79301439/188115459-e1bfa17d-d4dc-48a1-99f7-d2cace885b3d.png)

```java
@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
@Getter
public class BaseTimeEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime lastModifiedDate;
}


@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
@Getter
public class BaseEntity extends BaseTimeEntity {

    @CreatedBy
    @Column(updatable = false)
    private String createdBy;

    @LastModifiedBy
    private String lastModifiedBy;
}
```

![image](https://user-images.githubusercontent.com/79301439/188115867-d25bb81c-7171-4daf-9a0a-aa3fa5375661.png)

![image](https://user-images.githubusercontent.com/79301439/188116024-8bb1b45a-2c90-492a-99f9-2f0e3a873ee6.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<entity-mappings xmlns="http://xmlns.jcp.org/xml/ns/persistence/orm"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence/orm http://xmlns.jcp.org/xml/ns/persistence/orm_2_2.xsd"
    version="2.2">

    <persistence-unit-metadata>
        <persistence-unit-defaults>
            <entity-listeners>
                <entity-listener
                class="org.springframework.data.jpa.domain.support.AuditingEntityListener"/>
            </entity-listeners>
        </persistence-unit-defaults>
    </persistence-unit-metadata>
    
</entity-mappings>
```
