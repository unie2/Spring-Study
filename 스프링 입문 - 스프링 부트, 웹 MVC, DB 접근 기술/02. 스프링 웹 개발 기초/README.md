### 1. 정적 컨텐츠
| resources/static/hello-static.html | 실행 | 
|:--------:|:--------:|
| ![html](https://user-images.githubusercontent.com/54324782/177135521-a43a07e0-1b2d-4047-9235-773fe120fee3.png)| ![실행](https://user-images.githubusercontent.com/54324782/177135598-bc06b5bf-b67f-4997-bfd5-ae474ff11106.png)


![image](https://user-images.githubusercontent.com/54324782/177135761-9287463f-f9d9-474a-9210-731b998e8e62.png)
1. 내장 톰캣 서버가 요청을 받고 스프링 컨테이너로 넘긴다.
2. 스프링은 먼저 컨트롤러 쪽에서 `hello-static`이 있는지 찾아본다.
3. 컨트롤러에 `hello-static`이 없으므로 `resources/static/hello-static.html`을 찾은 후 반환한다.
---------------------------------

### 2. MVC와 템플릿 엔진
| HelloController.java | resources/templates/hello-template.html |
|:--------:|:--------:|
![controller](https://user-images.githubusercontent.com/54324782/177139011-02bb5202-537f-419b-bbfb-e4cf028f3305.png)| ![html](https://user-images.githubusercontent.com/54324782/177138289-dc0c75fc-b539-417f-a0e6-be4eb08a29fd.png)| 
| 실행 |
![실행](https://user-images.githubusercontent.com/54324782/177138666-2dce9316-9b8c-42b7-8f41-feaf184df360.png) |

![image](https://user-images.githubusercontent.com/54324782/177139389-a5ba3c4b-c67f-46a7-9536-7edf36e0c0b2.png)

- 정적일 때는 변환하지 않고 그대로 반환해주었지만, 템플릿 엔진에서는 변환하여 웹 브라우저에 넘겨준다.
---------------------------------

### 3. API
"@ResponseBody 문자 반환"  
| HelloController.java | 실행 | 
|:--------:|:--------:|
![controller](https://user-images.githubusercontent.com/54324782/177232927-e2015eeb-d217-46b4-bdc9-b68b60c30df9.png)| ![실행](https://user-images.githubusercontent.com/54324782/177232964-688c8331-5ebb-4a5c-9b1e-6f992d8b7509.png)
  - `@ResponseBody`를 사용하면 뷰 리졸버(`viewResolver`)를 사용하지 않는다.
  - 대신에 HTTP의 BODY에 문자 내용을 집접 반환한다.


"@ResponseBody 객체 반환"
| HelloController.java | 실행 | 
|:--------:|:--------:|
| ![controller](https://user-images.githubusercontent.com/54324782/177232198-a8488550-17f5-41a0-a2aa-a5c2a2e79a8d.png)| ![실행](https://user-images.githubusercontent.com/54324782/177232213-9dbbd2ab-0d94-4d1f-b3d4-6cf3283d34da.png)

- @ResponseBody 사용 원리  
![@ResponseBody](https://user-images.githubusercontent.com/54324782/177232259-b0b62f37-6ce2-45c6-92a0-485032f4a292.png)
  - ResponseBody를 사용하고, 객체가 오면 기본적으로 JSON 방식으로 데이터를 만들어서 Http 응답에 반환한다.
  - `@ResponseBody`를 사용
    - HTTP의 BODY에 문자 내용을 직접 반환
    - `viewResolver` 대신에 `HttpMessageConverter`가 동작
    - 기본 문자처리 : `StringHttpMessageConverter`
    - 기본 객체처리 : `MappingJackson2HttpMessageConverter`
    - byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있다.
