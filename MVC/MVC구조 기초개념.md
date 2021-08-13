## MVC 템플릿과 엔진
M:Model         
V:View                  
C:Controller               

### Controller
```java
@Controller
public class HelloController {
 @GetMapping("hello-mvc")
 public String helloMvc(@RequestParam("name") String name, Model model) {
 model.addAttribute("name", name);
 return "hello-template";
 }
}
```

### View
**경로: resources/template/hello-template.html**                
```java
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

![캡처](https://user-images.githubusercontent.com/84822464/129309151-e684e7e1-cde9-4aa7-a257-6e6d3ea52a39.PNG)
