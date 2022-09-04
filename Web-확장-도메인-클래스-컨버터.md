## **확장 기능**

![image](https://user-images.githubusercontent.com/79301439/188300472-0ce35e26-c07a-4ddc-bd78-1857406892ff.png)

```java
@RestController
@RequiredArgsConstructor
public class MemberController {

    private final MemberRepository memberRepository;

    @GetMapping("/members/{id}")
    public String findMember(@PathVariable("id") Long id) {
        Member member = memberRepository.findById(id).get();
        return member.getUsername();
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/188300493-ec82ff34-cb8d-40f6-9d41-adca60426fc4.png)

```java
@RestController
@RequiredArgsConstructor
public class MemberController {

    private final MemberRepository memberRepository;

    @GetMapping("/members/{id}")
    public String findMember(@PathVariable("id") Member member) {
        return member.getUsername();
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/188300531-083ea124-144c-48e8-a363-c74f6e718049.png)
