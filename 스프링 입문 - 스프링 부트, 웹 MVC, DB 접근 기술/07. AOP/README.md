### 1. AOP가 필요한 상황

  - "모든 메소드의 호출 시간을 측정하고 싶을 경우"
#### MemberService.java
````java
/* 회원 가입 */
public Long join(Member member) {
    long start = System.currentTimeMillis();

    try {
        // 같은 이름이 있는 중복 회원 X
        validateDuplicateMember(member); // 중복 회원 검증
        memberRepository.save(member);
        return member.getId();
    } finally {
        long finish = System.currentTimeMillis();
        long timeMs = finish - start;
        System.out.println("join = " + timeMs + "ms");
    }

}
````

  - "문제"
    - 회원가입, 회원 조회에 시간을 측정하는 기능은 핵심 관심 사항이 아니다.
    - 시간을 측정하는 로직은 공통 관심 사항이다.
    - 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다.
    - 시간을 측정하는 로직을 별도의 공통 로직으로 만들기 매우 어렵다.
    - 시간을 측정하는 로직을 변경할 때 모든 로직을 찾아가면서 변경해야 한다.

- - -
### 2. AOP 적용
  - AOP: Aspect Oriented Programming
  - 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 분리
  ![AOP](https://user-images.githubusercontent.com/54324782/180955074-5d99e9cd-de63-400e-ac39-8ffbb291d368.png)

#### TimeTraceAop.java
````java
@Aspect
@Component
public class TimeTraceAop {

    @Around("execution(* hello.hellospring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toString());

        try {
            return joinPoint.proceed();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;

            System.out.println("END: " + joinPoint.toString() + " " + timeMs + "ms");
        }
    }
}

````
#### 출력
![출력](https://user-images.githubusercontent.com/54324782/180957056-02cf7669-2590-48f8-9ec3-c0316b9dd40f.png)

  - "해결"
    - 회원가입, 회원 조회 등 핵심 관심 사항과 시간을 측정하는 공통 관심 사항을 분리한다.
    - 시간을 측정하는 로직을 별도의 공통 로직으로 생성
    - 핵심 관심 사항을 깔끔하게 유지 가능
    - 변경이 필요하면 이 로직만 변경
    - 원하는 적용 대상을 선택할 수 있다.

- - -
![AOP](https://user-images.githubusercontent.com/54324782/180958282-da449669-7c00-4233-b960-87f237bcf712.png)
