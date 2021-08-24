## JpaRepository
코드 상에서 반복되는 CRUD를 하나로 묶어버린 interface          
![캡처](https://user-images.githubusercontent.com/84822464/130561553-ddb32e58-79d6-4f52-9b48-7e02604e6304.PNG)            

### 메서드 이름으로 쿼리 생성
메서드 이름만으로 JPQL 쿼리 생성        
```java
public interface MemberRepository extends JpaRepository<Member, Long>{
    List<Member> findByName(String username);
}
```
### 이름으로 검색+ 정렬+ 페이징
```java
Pageable page = new PageRequest(1,20,new Sort...);
Page<Member> result = memberRepository.findByName("hello",page);

int total = result.getTotalElements(); // 전체 수 
List<Member> members = result.getContent(); //데이터  

=========member Repository 에서=======
List<Member> findByName(String username, Pageable pageable);

```

### @Query
@Query를 사용해서 직접 JPQL 지정            
```java
public interface MemberRepository extends JpaRepository<Member, Long>{

    @Query("select m from Memeber m where m.username =?1")
    Member findByUsername(String username, Pagealbe pageable);
```

### 반환타입
List<Member> findByName(String name);                        
String findByName(String name);       
                      
### Web 페이징과 정렬기능 
컨트롤러에서 페이징 처리 객체를 바로 받을 수 있음             
 - page: 현재 페이지         
 - size: 한 페이지에 노출할 데이터 건수            
 - sort: 정렬 조건            
  
  /members?page=0&size=20&sort=name,desc
  ```java
  @RequestMapping(value ="/members",method = RequestMethod.GET)
  String list(Pageable pageable, Model model){}
  ```
  
  
