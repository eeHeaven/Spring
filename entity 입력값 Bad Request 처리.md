## Entity 필드 입력에 제한을 두기 

#### 1. 입력 필드 자체를 제한하는 방법
단순하게 saveDto 필드에 입력받고 싶은 필드만 넣으면 된다!         

#### 2. 입력받는 필드의 값을 제한하는 방법
**1) 어노테이션 활용하기 (단순한 제한일 경우)**        
> @NotNull             
> @NotEmpty              
> @Min(최솟값)              
> @Max(최댓값)         
> ...         

**2) Validator 클래스 생성 후 활용하기**        
```java
@Component
public class EventValidator{

  public void validate(EventDto eventDto, Errors errors){ //스프링에서 지원하는 error import할 것
    if(eventDto.getOOO 조건에 맞지 않는 경우~~){
        errors.rejectValue(필드, 에러코드, 디폴트메세지); //ex) ("basePrice","WrongPrice","BasePrice is wrong");
        }
  }
}
```
```java
@Controller 에서

@Autowired
EventValidator eventvalidator;

@PostMapping
public ResponseEntity createEvent(@RequestBody @Valid EventDto eventDto, Errors errors){
  if(errors.hasErrors()){
    return ResponseEntity.badRequest().build();
  }
  eventValidator.validate(eventDto, errors);
  if(errors.hasErrors()){
    return ResponseEntity.badRequest().build();
  }
  Event event = modelMapper.map(eventDto, Event.class);
  Event newEvent = this.eventRepository.save(event);
  URI createUri = linkTo(EventController.class).slash(newEvent.getId()).toUri();
  return ResponseEntity.created(createUri).body(event);
}
```
