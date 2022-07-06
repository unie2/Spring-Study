### ✔ "스프링 빈을 등록하는 2가지 방법"
  - 컴포넌트 스캔과 자동 의존관계 설정
  - 자바 코드로 직접 스프링 빈 등록하기

- - -
### 1. 컴포넌트 스캔과 자동 의존관계 설정
  - `@Component` 애노테이션이 있으면 스프링 빈으로 자동 등록된다.
  - `@Controller` 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔 때문이다.
  - `@Component` 를 포함하는 다음 애노테이션도 스프링 빈으로 자동 등록된다.
    ```
      @Controller       @Service      @Repository
    ```
#### 1.1 스프링 빈을 등록하고, 의존관계 설정하기
  - 회원 컨트롤러가 회원 서비스와 회원 리포지토리를 사용할 수 있게 의존관계 준비
#### 회원 컨트롤러에 의존관계 추가 
  - 생성자에 `@Autowired`가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다.
  - 이렇게 객체 의존 관계를 외부에서 넣어주는 것을 의존성 주입(DI, Dependency Injection) 이라고 한다.
  ````java
package hello.hellospring.controller;

import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class MemberController {
    // 스프링 컨테이너에 등록
    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}

  ````
  
#### 회원 서비스 의존관계 추가
````java
@Service
public class MemberService {

    private final MemberRepository memberRepository;

    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    .
    .
    .
}

````

#### 회원 리포지토리 의존관계 추가
````java
@Repository
public class MemoryMemberRepository implements MemberRepository {
  .
  .
  .
}

````

- - -
### 2. 자바 코드로 직접 스프링 빈 등록하기
  - 회원 서비스와 회원 리포지토리의 @Service, @Repository, @Autowired 애노테이션을 제거하고 진행
  ````java
  // Controller는 그대로
  @Controller
  public class MemberController {
      // 스프링 컨테이너에 등록
      private final MemberService memberService;

      @Autowired
      public MemberController(MemberService memberService) {
          this.memberService = memberService;
      }
  }
  
  ````
  ````java
  package hello.hellospring;

  import hello.hellospring.repository.MemberRepository;
  import hello.hellospring.repository.MemoryMemberRepository;
  import hello.hellospring.service.MemberService;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;

  @Configuration
  public class SpringConfig {
      @Bean
      public MemberService memberService() {
          return new MemberService(memberRepository());
      }

      @Bean
      public MemberRepository memberRepository() {
          return new MemoryMemberRepository(); // 구현체
     }
  }

  
  ````
  - XML로 설정하는 방식도 있지만 최근에는 잘 사용하지 않는다.
  - DI에는 필드 주입, Setter 주입, 생성자 주입 방법이 있지만, 의존관계가 실행 중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다.
  - 실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용한다. 
  - 또한, 정형화 되지 않거나 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록한다.
  - `@Autowired`를 통한 DI는 `helloController`, `MemberService` 등과 같이 스프링이 관리하는 객체에서만 동작한다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.

