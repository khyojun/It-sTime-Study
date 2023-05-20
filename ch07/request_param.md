## CH 07. 요청 파라미터 획득하기
이번 장에서 뷰에서 입력한 값을 서버로 보내고 컨트롤러가 받는 방법에 대해 설명한다.

### 요청 파라미터란
> 서버에서 전송되는 값을 '요청 파라미터'라고 합니다.


### 요청 파라미터의 종류

| 요청 파라미터                           | 내용                                         | 
|-----------------------------------|--------------------------------------------|
| 요청 쿼리 스트링(query string)으로 보내지는 값  | 뷰에서 입력값 및 선택한 값이나 숨김 파라미터등에서 미리 뷰에 입력해둔 값 등 | 
| 요청 본문(body)에 저장되어 보내지는 값          | ...                                        |  
| 뷰에서 클릭한 버튼의 name 속성값              | 하나의 뷰에 버튼이 여러 개 있을 때 어느 버튼인지 판별할 수 있는 값    |
| URL 경로(path)의 일부로 보내지는 값          | 링크 등으로 URL의 일부로 보내지는 값                     | 


---
## 요청 파라미터 취득 방법

### 1. @RequestParam

> @RequestParam 어노테이션은 스프링이 쿼리, 양식 데이터 또는 임의의 사용자 정의 데이터로 전달될 수 있는 입력 데이터를 추출할 수 있도록 합니다. 

### 2. Form 클래스(태그 아님 ) 사용

> 스프링 MVC가 Form 클래스 내의 필드에 대해 값을 저장한다. 요청 파라미터를 모아서 하나의 객체로 받아들이기 때문에 자주 사용되는 방법이다. 받을 때 '형 변환', '포맷 지정'이 가능하다.


2 가지 방법이 이런 식으로 존재하게 되는데  링크나 URL의 일부로 포함된 값을 취득하고 싶을 때는 @PathVariable 어노테이션과 값을 저장할 인수를 지정한다.




---

## 추가적인 궁금증? Form 태그

스프링 공식 문서에 나오는 예제를 한 번 찾아보면서 진행하기로 하였다.



### GreetingController
```java
package com.example.handlingformsubmission;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

@Controller
public class GreetingController {

  @GetMapping("/greeting")
  public String greetingForm(Model model) {
    model.addAttribute("greeting", new Greeting());
    return "greeting";
  }

  @PostMapping("/greeting")
  public String greetingSubmit(@ModelAttribute Greeting greeting, Model model) {
    model.addAttribute("greeting", greeting);
    return "result";
  }

}
```



### Greeting
```java
package com.example.handlingformsubmission;

public class Greeting {

  private long id;
  private String content;

  public long getId() {
    return id;
  }

  public void setId(long id) {
    this.id = id;
  }

  public String getContent() {
    return content;
  }

  public void setContent(String content) {
    this.content = content;
  }

}
```

### greeting.html
```java
<!DOCTYPE HTML>
<html xmlns:th="https://www.thymeleaf.org">
<head> 
    <title>Getting Started: Handling Form Submission</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
	<h1>Form</h1>
    <form action="#" th:action="@{/greeting}" th:object="${greeting}" method="post">
    	<p>Id: <input type="text" th:field="*{id}" /></p>
        <p>Message: <input type="text" th:field="*{content}" /></p>
        <p><input type="submit" value="Submit" /> <input type="reset" value="Reset" /></p>
    </form>
</body>
</html>

```



### result.html
```java
<!DOCTYPE HTML>
<html xmlns:th="https://www.thymeleaf.org">
<head> 
    <title>Getting Started: Handling Form Submission</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
	<h1>Result</h1>
    <p th:text="'id: ' + ${greeting.id}" />
    <p th:text="'content: ' + ${greeting.content}" />
    <a href="/greeting">Submit another message</a>
</body>
</html>

```


이런 경우 코드가 어떻게 진행이 될까? 순서대로 한 번 말해보겠다.

1. /greeting으로 요청을 보낸다.
2. greeting.html로 이동한다.
3. greeting.html에서 form 태그로 간 다음 input 태그에 greeting에 대하여 id, content의 값을 input태그에 넣어서 보내준다.
4. 그러면 spring에서는 양식을 채워진 내용을 바탕으로 @ModelAttribute Greeting greeting에 바인딩한다.
```java
 @PostMapping("/greeting")
 public String greetingSubmit(@ModelAttribute Greeting greeting, Model model) {
```
5. 그렇게 안쪽에서 채워진 내용을 result.html로 보낸다.
6. 결과를 출력시키면 아까 입력시켰던 내용들이 보여진다.



이 과정 안에 많은 과정들이 들어있지만 일단 간단하게 설명하면 form 태그 안에 있던 내용을 바인딩 시켜서 다시 보낸 후 Greetinig에 바인딩 시켜서 출력이 되게 합니다.

---

## @RequestParam, @ModelAttribute

### @RequestParam
> @RequestParam 어노테이션은 클라이언트가 전송하는 파라미터를 1:1로 받기 위해 사용합니다.



### @ModelAttribute
> @ModelAttribute는 클라이언트가 전송하는 여러 파라미터를 1:1로 객체에 바인딩 한 후 다시 View로 데이터를 넘겨서 출력하기 위해 사용되는 어노테이션 입니다.










## 출처

- https://spring.io/guides/gs/handling-form-submission/
- https://zzang9ha.tistory.com/298
- https://www.geeksforgeeks.org/spring-mvc-form-tag-library/






