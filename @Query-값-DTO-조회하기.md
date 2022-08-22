## **쿼리 메소드 기능**

![image](https://user-images.githubusercontent.com/79301439/185932286-4078f43c-4415-445c-979e-aa535e5d4ba1.png)

```java
@Query("select m.username from Member m")
List<String> findUsernameList();
```
![image](https://user-images.githubusercontent.com/79301439/185932495-e9ff70ca-5d59-44fd-98e2-8b96e43c6a7e.png)

![image](https://user-images.githubusercontent.com/79301439/185932531-b19bc75c-d78c-4f9c-83e2-bef565122fc1.png)

```java
@Query("select new study.datajpa.dto.MemberDto(m.id, m.username, t.name) from Member m join m.team t")
List<MemberDto> findMemberDto();
```

![image](https://user-images.githubusercontent.com/79301439/185932645-e7d92b04-263a-4d75-bcab-8448b7aa2204.png)

```java
package study.datajpa.dto;

import lombok.Data;

@Data
public class MemberDto {

    private Long id;
    private String username;
    private String teamName;

    public MemberDto(Long id, String username, String teamName) {
        this.id = id;
        this.username = username;
        this.teamName = teamName;
    }
}
```
