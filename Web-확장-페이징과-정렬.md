## **확장 기능**

![image](https://user-images.githubusercontent.com/79301439/188302038-dd012cd1-aa7b-4233-a4ea-84963770f1d1.png)

```java
@GetMapping("/members")
public Page<Member> list(Pageable pageable) {
    Page<Member> page = memberRepository.findAll(pageable);
    return page;
}
```

![image](https://user-images.githubusercontent.com/79301439/188302064-fbfee650-38c4-4066-b158-cd497bd93431.png)

![image](https://user-images.githubusercontent.com/79301439/188302081-d2754339-9b9b-47dd-8603-adf4856acdcc.png)

```yml
#application.yml
spring:
...
  data:
    web:
      pageable:
        default-page-size: 10
        max-page-size: 2000
...
```

![image](https://user-images.githubusercontent.com/79301439/188302137-f52c6970-f481-498f-a5d4-0167d88a43c0.png)

```java
@RequestMapping(value = "/members_page", method = RequestMethod.GET)
public String list(@PageableDefault(size = 12, sort = "username",
                    direction = Sort.Direction.DESC) Pageable pageable) {
    ...
} 
```

![image](https://user-images.githubusercontent.com/79301439/188302209-424b37bb-ca72-4111-8c72-48e72fbae6ee.png)

![image](https://user-images.githubusercontent.com/79301439/188302265-3609a7a1-98a0-4661-9893-38f3a9c2ec8c.png)

![image](https://user-images.githubusercontent.com/79301439/188302275-abfbbb86-c581-490f-b1e0-ad3e6a722955.png)

```java
package study.datajpa.dto;

import lombok.Data;
import study.datajpa.entity.Member;

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

    public MemberDto(Member member) {
        this.id = member.getId();
        this.username = member.getUsername();
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/188302302-712403d2-d00a-4982-a85c-942a497a8ae7.png)

```java
@GetMapping("/members")
public Page<MemberDto> list(Pageable pageable) {
    Page<Member> page = memberRepository.findAll(pageable);
    Page<MemberDto> pageDto = page.map(MemberDto::new);
    return pageDto;
}
```

![image](https://user-images.githubusercontent.com/79301439/188302356-84374e6a-c245-461b-90a6-29a2aa697063.png)

```java
@GetMapping("/members")
public Page<MemberDto> list(Pageable pageable) {
    return memberRepository.findAll(pageable).map(MemberDto::new);
}
```

![image](https://user-images.githubusercontent.com/79301439/188302374-42f5fb5c-ed95-46eb-9efa-514de9978181.png)

![image](https://user-images.githubusercontent.com/79301439/188302380-1a88d23c-983a-442e-bb98-335f6418b8dd.png)

![image](https://user-images.githubusercontent.com/79301439/188302424-f6dcc2fc-4a15-4ba8-a5ac-b9a51a17c254.png)
