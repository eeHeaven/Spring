### ModelMapper 
ModelMapper를 사용하면 Dto를 entity로 간편하게 변환할 수 있다.

#### 1. ModelMapper 의존성 추가하기 
```java
<dependency>
<groupId>org.modelmapper</groupId>

<artifactId>modelmapper</artifactId>
<version>2.3.1</version> </dependency>
```

#### 2. ModelMapper Bean 등록하기
```java
@Bean 
public ModelMapper modelMapper(){
return new ModelMapper() ;}
```

#### 3. Dto->Entity 매핑하기
```java
@Autowired
ModelMapper modelMapper;

Event event = modelMapper.map(eventDto,Event.class);
```
