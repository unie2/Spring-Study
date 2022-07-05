### 1. 비즈니스 요구사항 정리
#### 1.1 일반적인 웹 애플리케이션 계층 구조
![계층 구조](https://user-images.githubusercontent.com/54324782/177292968-c0a00ad1-27de-4a3f-97c1-d0127a94237e.png)
  - 컨트롤러 : 웹 MVC의 컨트롤러 역할
  - 서비스 : 핵심 비즈니스 로직 구현
  - 리포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
  - 도메인 : 비즈니스 도메인 객체 `ex) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리된다.`

#### 1.2 클래스 의존관계
![의존관계](https://user-images.githubusercontent.com/54324782/177294552-0ccae657-76b7-4674-96f4-d78fcaf1d960.png)
  - 우선 인터페이스로 구현 클래스를 변경할 수 있도록 설계 (아직 데이터 저장소가 선정되지 않았으므로)
  - 데이터 저장소는 RDB, NoSQL 등등 다양한 저장소를 고민중인 상황으로 가정
  - 개발을 진행하기 위해서 초기 개발 단계에서는 구현체로 가벼운 메모리 기반의 데이터 저장소 사용

- - -

### 2. 회원 도메인과 리포지토리 만들기
| Member 도메인 | MemberRepository | MemoryMemberRepository | 
|:--------:|:--------:|:--------:|
![Member](https://user-images.githubusercontent.com/54324782/177299821-3710a97d-9e10-480e-ba0e-b241f77d6802.png)| ![MemberRepository](https://user-images.githubusercontent.com/54324782/177299906-bdb80ec5-f265-4a05-9869-d9c186275668.png)| ![MemoryMemberRepository](https://user-images.githubusercontent.com/54324782/177299994-180e0bda-16db-4bb6-8250-09d4ceb27135.png)

- - -

### 3. 회원 리포지토리 테스트 케이스 작성
- 개발한 기능을 실행해서 테스트할 때 자바의 main 메서드를 통해서 실행하거나, 웹 애플리케이션의 컨트롤러를 통해서 해당 기능을 실행한다.
- 이러한 방법은 준비하고 실행하는데 오래 걸리고, 반복 실행하기 어렵고 여러 테스트를 한번에 실행하기 어렵다.
- 자바는 `JUnit`이라는 프레임워크로 테스트를 실행해서 이러한 문제를 해결한다.
#### 3.1 회원 리포지토리 메모리 구현체 테스트
````java
public class MemoryMemberRepositoryTest {
    MemoryMemberRepository repository = new MemoryMemberRepository();

    @AfterEach // 메서드 실행이 끝날 때마다 어떠한 동작을 하는 것
    public void afterEach() {
        repository.clearStore(); // store 비우기
    }

    @Test
    public void save() {
        Member member = new Member();
        member.setName("spring");

        repository.save(member);

        Member result = repository.findById(member.getId()).get();
        // 일치하는지 확인 (불일치 시 failed)
        Assertions.assertThat(member).isEqualTo(result);
    }

    @Test
    public void findByName() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result = repository.findByName("spring1").get();
        // 일치하는지 확인 (불일치 시 failed)
        Assertions.assertThat(result).isEqualTo(member1);
    }

    @Test
    public void findAll() {
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        List<Member> result = repository.findAll();

        Assertions.assertThat(result.size()).isEqualTo(2);
    }
}

````
| Tests Passed | Tests Failed | 
|:--------:|:--------:|
![passed](https://user-images.githubusercontent.com/54324782/177305155-0d72a0d5-006d-457b-860e-dc86685b9cd0.png)| ![failed](https://user-images.githubusercontent.com/54324782/177305286-75f3d621-3a8e-48c7-8e26-2cb096857f82.png)

- - -

### 4. 회원 서비스 개발
  - 회원 리포지토리와 도메인을 활용해서 실제 비즈니스 로직을 작성



