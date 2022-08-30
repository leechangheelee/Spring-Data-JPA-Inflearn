## **쿼리 메소드 기능**

![image](https://user-images.githubusercontent.com/79301439/187471655-65d98abe-2b7b-4bd6-8099-a902ff5b535e.png)

```java
...
public interface MemberRepository extends JpaRepository<Member, Long> {

    ...
    
    Page<Member> findByUsername(String name, Pageable pageable); //count 쿼리 사용
    Slice<Member> findByUsername(String name, Pageable pageable); //count 쿼리 사용안함
    List<Member> findByUsername(String name, Pageable pageable); //count 쿼리 사용안함
    List<Member> findByUsername(String name, Sort sort);
}
```

![image](https://user-images.githubusercontent.com/79301439/187472055-1b5e0a4b-d591-4812-b38c-a2eaa72e4f51.png)

```java
public interface MemberRepository extends Repository<Member, Long> {
    Page<Member> findByAge(int age, Pageable pageable);
}
```

![image](https://user-images.githubusercontent.com/79301439/187472321-53fcbd22-53b0-4f2b-98fc-e8e87b433529.png)

```java
...
class MemberRepositoryTest {
    ...
    
    //페이징 조건과 정렬 조건 설정
    @Test
    public void paging() {
        //given
        memberRepository.save(new Member("member1", 10));
        memberRepository.save(new Member("member2", 10));
        memberRepository.save(new Member("member3", 10));
        memberRepository.save(new Member("member4", 10));
        memberRepository.save(new Member("member5", 10));

        //when
        PageRequest pageRequest = PageRequest.of(0, 3, Sort.by(Sort.Direction.DESC, "username"));
        Page<Member> page = memberRepository.findByAge(10, pageRequest);

        Page<MemberDto> toMap = page.map(m -> new MemberDto(m.getId(), m.getUsername(), null));

        //then
        List<Member> content = page.getContent(); //조회된 데이터
        assertThat(content.size()).isEqualTo(3); //조회된 데이터 수
        assertThat(page.getTotalElements()).isEqualTo(5); //전체 데이터 수
        assertThat(page.getNumber()).isEqualTo(0); //페이지 번호
        assertThat(page.getTotalPages()).isEqualTo(2); //전체 페이지 번호
        assertThat(page.isFirst()).isTrue(); //첫 번째 항목인가?
        assertThat(page.hasNext()).isTrue(); //다음 페이지가 있는가?
    }
}
```

![image](https://user-images.githubusercontent.com/79301439/187473105-391e9ec8-cc63-4493-9ce1-f850d94e210a.png)

![image](https://user-images.githubusercontent.com/79301439/187473219-64925eb0-1464-4cb7-819f-d61e5a3df21b.png)

```java
public interface Page<T> extends Slice<T> {
    int getTotalPages(); //전체 페이지 수
    long getTotalElements(); //전체 데이터 수
    <U> Page<U> map(Function<? super T, ? extends U> converter); //변환기
}
```

![image](https://user-images.githubusercontent.com/79301439/187473359-b9723556-9070-4f6b-b991-0fcbe9d7e2a9.png)

```java
public interface Slice<T> extends Streamable<T> {
    int getNumber(); //현재 페이지
    int getSize(); //페이지 크기
    int getNumberOfElements(); //현재 페이지에 나올 데이터 수
    List<T> getContent(); //조회된 데이터
    boolean hasContent(); //조회된 데이터 존재 여부
    Sort getSort(); //정렬 정보
    boolean isFirst(); //현재 페이지가 첫 페이지 인지 여부
    boolean isLast(); //현재 페이지가 마지막 페이지 인지 여부
    boolean hasNext(); //다음 페이지 여부
    boolean hasPrevious(); //이전 페이지 여부
    Pageable getPageable(); //페이지 요청 정보
    Pageable nextPageable(); //다음 페이지 객체
    Pageable previousPageable();//이전 페이지 객체
    <U> Slice<U> map(Function<? super T, ? extends U> converter); //변환기
}
```

![image](https://user-images.githubusercontent.com/79301439/187473520-aeddc6ac-4d43-47c1-b3a8-4f47708ac668.png)

```java
@Query(value = “select m from Member m”,
    countQuery = “select count(m.username) from Member m”)
Page<Member> findMemberAllCountBy(Pageable pageable);
```

![image](https://user-images.githubusercontent.com/79301439/187473716-4a04be9f-4231-4b05-998f-1ff1d4053145.png)

```java
...
class MemberRepositoryTest {
    ...
    
    @Test
    public void paging() {
        ...
        
        Page<Member> page = memberRepository.findByAge(10, pageRequest);
        Page<MemberDto> toMap = page.map(m -> new MemberDto(m.getId(), m.getUsername(), null));
        
        ...
    }     
}
```

![image](https://user-images.githubusercontent.com/79301439/187474233-1dc8dc00-c5e6-42d4-93b1-d7ea1a20dc01.png)
