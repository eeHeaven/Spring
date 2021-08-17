### 1. 홈화면 
#### 홈 컨트롤러 추가 
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
#### 회원 관리용 홈
```java
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
</div> <!-- /container -->
</body>
</html>
```
### 2. 회원 등록

#### 회원 등록 폼 컨트롤러 
```java
@Controller
public class MemberController {
 private final MemberService memberService;
 @Autowired
 public MemberController(MemberService memberService) {
 this.memberService = memberService;
 }
 @GetMapping(value = "/members/new")
 public String createForm() {
 return "members/createMemberForm";
 }
}
```
#### 회원 등록 폼 html (resources/templates/members/createMemberForm)
```java
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
 <form action="/members/new" method="post">
 <div class="form-group">
 <label for="name">이름</label>
 <input type="text" id="name" name="name" placeholder="이름을
입력하세요">
 </div>
 <button type="submit">등록</button>
 </form>
</div> <!-- /container -->
</body>
</html>
```

#### 회원 등록 컨트롤러
**웹 등록 화면에서 데이터를 전달 받을 폼 객체**           
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
#### 회원 컨트롤러에서 회원을 실제로 등록하는 기능 
```java
@PostMapping(value = "/members/new")
public String create(MemberForm form) {
 Member member = new Member();
 member.setName(form.getName());
 memberService.join(member);
 return "redirect:/";
}
```
*********************
### 폼 컨트롤러와 폼 객체, 그리고 컨트롤러에서 회원 등록 기능을 따로 두는 원리와 이유
이유: 파라미터 간소화+ 폼객체 검증 용이             
원리: POST 요청이 넘어오면 form 객체가 생성되고 화면 단(회원 등록 폼 html)에서 input 속성의 name으로 넘어온 값과 같은 이름을 form 클래스의 변수에서 찾아 바인딩이 이루어짐        
**따라서 폼 객체의 변수들과 input 폼에서의 속성 name 의 값이 일치해야 바인딩이 가능함**       
********************
### 3. 회원 조회 
#### 회원 컨트롤러에서 조회 기능
Model 객체에 get해서 화면에 출력할 데이터를 담음            
model.addAttribute("model명", 받을 데이터 객체) 형태로 담으면 됨                 

```java
@GetMapping(value = "/members")
public String list(Model model) {
 List<Member> members = memberService.findMembers();
 model.addAttribute("members", members);
 return "members/memberList";
}

```
#### 회원 리스트 html (resources/templates/members/memberList)
```java
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
 <tr th:each="member : ${members}">// member는 members리스트에서 하나씩 꺼내올 member 객체의 임의 이름, ${members}는 model에 addAttribute 된 이름
 <td th:text="${member.id}"></td>
 <td th:text="${member.name}"></td>
 </tr>
 </tbody>
 </table>
 </div>
</div> <!-- /container -->
</body>
</html>
```
