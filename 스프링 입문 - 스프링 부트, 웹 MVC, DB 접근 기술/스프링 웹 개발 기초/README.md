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
