## ResonponseEntity
사용자의 HttpRequest에 대한 응답 데이터를 포함하는 클래스이다. 따라서 **HttpStatus, HttpHeaders, HttpBody**를 포함한다.   
> - HttpStatus : 상태코드
> - header: httpheader 객체
> - ResponseData: 담을 entity 객체 (ex. member, post, event 등)

### ResponseEntity 활용하기
#### 1. Message 라는 클래스를 만들어 상태코드, 메세지, 데이터를 담을 필드를 추가한다. 
```java
mport lombok.Data;

@Data
public class Message {

    private StatusEnum status;
    private String message;
    private Object data;

    public Message() {
        this.status = StatusEnum.BAD_REQUEST;
        this.data = null;
        this.message = null;
    }
}
```

#### 2. 상태코드 enum을 생성한다. 
```java
public enum StatusEnum {

    OK(200, "OK"),
    BAD_REQUEST(400, "BAD_REQUEST"),
    NOT_FOUND(404, "NOT_FOUND"),
    INTERNAL_SERER_ERROR(500, "INTERNAL_SERVER_ERROR");

    int statusCode;
    String code;

    StatusEnum(int statusCode, String code) {
        this.statusCode = statusCode;
        this.code = code;
    }
}
```
### Controller 에서 위의 ResponseEntity 활용 예시
```java
@RestController
public class UserController {
    private UserDaoService userDaoService;

    public UserController(UserDaoService userDaoService) {
        this.userDaoService = userDaoService;
    }

    @GetMapping(value = "/user/{id}")
    public ResponseEntity<Message> findById(@PathVariable int id) {
        User user = userDaoService.findOne(id);
        Message message = new Message();
        HttpHeaders headers= new HttpHeaders();
        headers.setContentType(new MediaType("application", "json", Charset.forName("UTF-8")));

        message.setStatus(StatusEnum.OK);
        message.setMessage("성공 코드");
        message.setData(user);

        return new ResponseEntity<>(message, headers, HttpStatus.OK);
    }


}
```
id를 통해서 User를 가져오고 Message 클래스를 통해서 StatusCode, ResponseMessage, ResponseData를 담는다.        
클라이언트에게 응답을 보내는 코드에 위의 세 가지를 담아서 보낸다.         
