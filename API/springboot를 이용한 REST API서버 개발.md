## MVC 패턴을 이용하는 방법 
말그대로 Model View Controller로 이루어진 형태의 디자인 패턴으로 REST API 서버를 설계하겠다는 뜻        
![캡처](https://user-images.githubusercontent.com/84822464/129316191-2d28f04d-3dd7-4428-aa8c-6a03c14fe820.PNG)

Springboot로 본다면 Controller, Service(DAO), Repository로 나누어 데이터의 운반 처리를 세분화 하겠다는 것.        
기존의 Spring MVC와 차이가 있다면 반환 형식이 기존의 Spring MVC는 HTML 이었다면 REST API는 XML/JSON임    

### MVC 패턴을 이용한 API 개발 구현 방법

#### 1. 구현 환경 설정 
 - SpringWeb dependency 추가      
 - DB와의 연동을 위해 Spring Data JPA와 MySQL Driver 선택 (DB 선택에 따라 option 달라질 수 있음)         
 - Lombok 추가(선택)
 - MySQL 서버 구축 
 - Domain, Controller, Repository 패키지 만들기 

#### 2. REST API 구현 
##### Entity 구현
```java
package xyz.neonkid.restexample.rest.domain; //domain 패키지에 만들기 

import lombok.*;

import javax.persistence.*;
import java.io.Serializable;

@Getter 
@NoArgsConstructor(access = AccessLevel.PUBLIC) // 실제로 REST API를 이용하여 데이터를 가지고 올 때, 기본 생성자가 있어야 하는데 이를 생성하지 않고 어노테이션을 줌으로써 기본 생성자를 자동으로 만들어줌 
@Entity //Entity 선언 필요 
@Table
public class Item implements Serializable {
    @Id
    @Column
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long Id;

    @Column
    private String name;

    @Column
    private int price;

    @Builder //Setter의 경우, 멤버 변수별로 set 메소드를 일일이 불러와야 하기 때문에 코드가 지저분해지므로 생성자에 Builder 어노테이션을 줌 
    public Item(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public void update(Item item) {
        this.name = item.name;
        this.price = item.price;
    }
}
```

##### Repository 인터페이스 구현 후 JpaRepository 상속받기 
```java
package xyz.neonkid.restexample.rest.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import xyz.neonkid.restexample.rest.domain.Item;

public interface ItemRepository extends JpaRepository<Item, Long> {
}
```
Generic에는 위에서 만들어준 Item 클래스와 PK의 타입인 Long 타입으로 주면 됨             
**구현은 Repository 구현-> 서비스클래스 구현 -> Repository 인터페이스 주입 -> 서비스에서 데이터를 처리하는 방식으로 구현**       

##### Controller 구현 
```java
package xyz.neonkid.restexample.rest.controller;

import lombok.RequiredArgsConstructor;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.web.PageableDefault;
import org.springframework.hateoas.PagedModel;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import xyz.neonkid.restexample.rest.domain.Item;
import xyz.neonkid.restexample.rest.repository.ItemRepository;

import javax.xml.ws.Response;

@RequiredArgsConstructor
@RestController
@RequestMapping("/api/items")
public class ItemController {
    private final ItemRepository itemRepository;

    @GetMapping(produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<?> getItems(@PageableDefault Pageable pageable) {
        Page<Item> items = itemRepository.findAll(pageable);
        PagedModel.PageMetadata pageMetadata = new PagedModel.PageMetadata(pageable.getPageSize(),
                items.getNumber(), items.getTotalElements());
        PagedModel<Item> model = PagedModel.of(items.getContent(), pageMetadata);

        return ResponseEntity.ok(model);
    }

    @PostMapping
    public ResponseEntity<?> addItem(@RequestBody Item item) {
        itemRepository.save(item);
        return new ResponseEntity<>("{}", HttpStatus.CREATED);
    }

    @PutMapping("/{id}")
    public ResponseEntity<?> putItem(@PathVariable("id") Long id, @RequestBody Item item) {
        Item persistItem = itemRepository.getOne(id);
        persistItem.update(item);
        itemRepository.save(persistItem);
        return new ResponseEntity<>("{}", HttpStatus.OK);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<?> deleteItem(@PathVariable("id") Long id) {
        itemRepository.deleteById(id);
        return new ResponseEntity<>("{}", HttpStatus.OK);
    }
}
```
RestController 어노테이션 달기          
데이터베이스 IO 작업은 Repository 인터페이스에서 처리됨               
**기본적으로 JpaRepository에서는 save 메소드를 통해 INSERT 혹은 UPDATE 쿼리가 이루어지고 findOOO 메소드를 통해 SELECT가 이루어짐**         


## Springboot data rest를 사용하는 방법 
Repository 하나만 있다면 DB에서 CRUD(Create,Read,Update,Delete)를 모두 수행할 수 있도록하는 아주 간단한 라이브러리.          
MVC 패턴과는 달리 Controller,Service가 없고 레포지토리 내부 CRUD 메소드와 Mapping하여 처리.         


참고 링크 
https://blog.neonkid.xyz/220?category=814055
