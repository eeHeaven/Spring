## REST API란?
REST(Representational State Transfer)의 약자로 '대표적인 상태 전달' 이라는 의미를 담고 있음               
웹과 같은 분산 하이퍼미디어 시스템에서 사용하는 **통신 네트워크 아키텍처** 라고 생각하면 됨               

***********
**<참고>**           
URI: 인터넷에서 특정 자원을 나타내는 주솟값           
URL: 인터넷에서 특정 자원이나 파일의 위치를 나타내는 주솟값               
****************

## REST API 제약 조건
REST API 서버를 개발할 때 필요한 조건들    
**1. 클라이언트-서버 형태**                
관심사의 명확한 분리, 서버와 클라이언트의 역할을 분명하게 분리하는 것.        
서버의 구성요소가 단순화되고 확장성이 향상되어 여러 플랫폼을 지원할 수 있게 됨.              
**2. 무상태성**    
서버에 클라이언트 상태 정보를 일절 저장하지 않음을 말함.         
단순히 들어오는 요청 만을 처리하여 구현을 단순화 하되, 클라이언트의 모든 요청은 서버가 요청을 알아듣는 데 필요한 모든 정보를 알고 있어야 함.          
**3. 캐시기능**            
클라이언트의 요청을 캐싱. HTTP의 기능 중 캐시 기능을 적용한 것              
**4. 계층화 시스템**              
애플리케이션 서버가 중계서버(Proxy, Gateway)나 로드 밸런싱, 공유 캐시 등을 이용하여 확장성 있는 시스템을 구현해야 함.          
**5. 코드 온 디맨드**          
필수는 아님, JavaApplet/JavaScript 실행 코드를 전달받아 기능을 일시적으로 확장할 수 있어야 한다는 내용            
**6. 인터페이스 일관성**           
URI를 가능한 지정된 리소스에 균일하고 통일된 인터페이스를 제공해야 함.          
대표적으로 각 Entity 별로 지원을 식별하는 방법, 혹은 HATEOAS를 이용하는 방법이 있음.             
HATEOAS는 클라이언트에 응답하는 형식을 단순히 결과 데이터만 제공해주는 것이 아니라 URI를 같이 제공하는 것.        


#### @ResponseBody 객체 반환
```java
@Controller
public class HelloController {
 @GetMapping("hello-api")
 @ResponseBody
 public Hello helloApi(@RequestParam("name") String name) {
 Hello hello = new Hello();
 hello.setName(name);
 return hello;
 }
 static class Hello {
 private String name;
 public String getName() {
 return name;
 }
 public void setName(String name) {
 this.name = name;
 }
 }
}
```
RequestBody 어노테이션을 사용하고 객체를 반환하면 객체가 Json으로 변환됨           

#### @ResponseBody 사용원리 
![캡처](https://user-images.githubusercontent.com/84822464/129319526-20cc1929-1223-494a-9fa2-6bf113d3242b.PNG)

