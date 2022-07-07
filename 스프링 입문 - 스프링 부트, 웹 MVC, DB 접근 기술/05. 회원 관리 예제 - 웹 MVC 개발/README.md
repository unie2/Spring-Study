### 1. 회원 웹 기능 - 홈 화면 추가
  - 홈 컨트롤러 추가
```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {
    @GetMapping("/")
    public String home() {
        return "home";
    }
}

```
  - home.html
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div>
</div>
<!-- /container -->
</body>
</html>

```
  - 결과 페이지  
  ![결과](https://user-images.githubusercontent.com/54324782/177728787-ea936527-4f84-47ba-9258-fedbe8cc85d5.png)

- - -
### 2. 회원 웹 기능 - 등록
  - MemberController
  ```java
  @GetMapping("/members/new") // Get
  public String createForm() {
      return "members/createMemberForm";
  }

  @PostMapping("/members/new") // Post
  public String create(MemberForm form) {
      Member member = new Member();
      member.setName(form.getName());

      System.out.println("member = " + member.getName());
      memberService.join(member);

      return "redirect:/";
  }
  
  ```
  - createMemberForm.html
  ```html
  <!DOCTYPE HTML>
  <html xmlns:th="http://www.thymeleaf.org">
  <body>
  <div class="container">
      <form action="/members/new" method="post">
          <div class="form-group">
              <label for="name">이름</label>
              <input type="text" id="name" name="name" placeholder="이름을 입력하세요">
          </div>
          <button type="submit">등록</button>
      </form>
  </div>
  </body>
  <!-- /container -->
  </html>

  ```
  - MemberForm.java
  ```java
  package hello.hellospring.controller;

  public class MemberForm {
      private String name;

      public String getName() {
          return name;
      }

      public void setName(String name) {
          this.name = name;
      }
  }

  ```

- - -
### 3. 회원 웹 기능 - 조회
  - MemberController
  ```java
  @GetMapping("/members")
  public String list(Model model) {
      List<Member> members = memberService.findMembers();
      model.addAttribute("members", members);
      return "members/memberList";
  }
  
  ```
  - members/memberList.html
  ```html
  <!DOCTYPE HTML>
  <html xmlns:th="http://www.thymeleaf.org">
  <body>
  <div class="container">
      <div>
          <table>
              <thead>
              <tr>
                  <th>#</th>
                  <th>이름</th>
              </tr>
              </thead>
              <tbody>
              <tr th:each="member : ${members}">
                  <td th:text="${member.id}"></td>
                  <td th:text="${member.name}"></td>
              </tr>
              </tbody>
          </table>
      </div>
  </div>
  <!-- /container -->
  </body>
  </html>
  ```
 
- - -
#### 결과
| 홈 화면 | 회원가입(spring1, spring2 추가) | 회원 목록 | 
|:--------:|:--------:|:--------:|
| ![홈](https://user-images.githubusercontent.com/54324782/177728787-ea936527-4f84-47ba-9258-fedbe8cc85d5.png)| ![회원 가입](https://user-images.githubusercontent.com/54324782/177735792-bcc83767-c4a9-4c92-86f0-e524db947fc4.png) | ![회원 조회](https://user-images.githubusercontent.com/54324782/177735903-d4ca74a9-0b73-408d-8298-3032ebfd7099.png)
