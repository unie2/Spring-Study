### 1. 프로젝트 생성
![프로젝트 생성](https://user-images.githubusercontent.com/54324782/177120595-bd39d343-84a6-4bb0-9598-c66c97ad2a3b.png)
----------------------------

### 2. 라이브러리 살펴보기
 : Gradle은 의존관계가 있는 라이브러리를 함께 다운로드한다.
#### "스프링 부트 라이브러리"
- spring-boot-starter-web
  - spring-boot-starter-tomcat: 톰캣 (웹서버)
  - spring-webmvc: 스프링 웹 MVC
- spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
- spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
  - spring-boot
    - spring-core
  - spring-boot-starter-logging
    - logback, slf4j
#### "테스트 라이브러리 "
- spring-boot-starter-test
  - junit: 테스트 프레임워크
  - mockito: 목 라이브러리
  - assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
  - spring-test: 스프링 통합 테스트 지원
----------------------------

### 3. Welcome Page 만들기
- 스프링 부트가 제공하는 Welcome Page 기능
  - `resources/static/index.html` 을 올려두면 Welcome page 기능을 제공한다.
- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(`viewResolver`)가 화면을 찾아서 처리한다.
  - 스프링 부트 템플릿엔진 기본 viewName 매핑
  - `resources:templates/`+{ViewName}+`.html`  
----------------------------

### 4. 빌드하고 실행하기 (윈도우)  
1. gradlew 파일있는 폴더로 이동  
2. `.\gradlew build`  
3. build/libs 폴더 이동  
4. `java -jar hello-spring-0.0.1-SNAPSHOT.jar` 실행  
(+추가) build파일 제거 : `.\gradlew clean`  
----------------------------
