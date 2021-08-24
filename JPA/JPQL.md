### JPQL이란?
한마디로 객체 지향적 SQL이라고 생각하면 됨          
테이블이 아닌 객체를 대상으로 검색하는 쿼리           
SQL을 추상화해서 특정 데이터베이스 SQL에 의존하지 않는다.          

### JPQL 문법
 ```java 
select m from Member m where m.age>18
  ```
 - 엔티티와 속성은 대소문자 구분한다 (Member, username)             
 - JPQL 키워드는 대소문자 구분 안함(SELECT,FROM,where)          
 - 엔티티 이름을 사용, 테이블 이름이 아님           
 - 별칭은 필수(m)          

### 결과 조회 JPQL
- query.getResultList(); 결과가 하나 이상, 리스트 반환             
- query.getSingleResult(); 결과가 정확히 하나, 단일 객체 반환( 정확히 하나가 아니면 예외 발생)         

### 파라미터 바인딩 - 이름 기준, 위치 기준
```java
SELECT m FROM Member m where m.username =:username 
```
**query.setParameter("username",usernameParam);**                     
```java
SELECT m FROM Member m where m.username=?1
  ```
**query.setParameter(1,usernameParam);**        
**이름을 기준으로 하는 것을 추천**             

### 프로젝션 
SELECT 구절을 잡는 것           
- SELECT m FROM Member m : 엔티티 프로젝션           
- SELECT m.team FROM Member m -> 엔티티 프로젝션           
- SELECT m.username, m.age FROM Member m -> 단순 값 프로젝션           
- new 명령어: 단순 값을 DTO로 바로 조회           
```java
SELECT new jpabook.jqpl.UserDTO(m.username, m.age) FROM Member m 
```
 - DISTINCT는 중복 제거            

### 페이징 API
 - setFirstResult           
 - setMaxResults               

### 패치조인 
엔티티 객체 그래프를 한번에 조회하는 방법           
select m from Member m join fetch m.team           
SQL) SELECT M.*,T.*,FROM Member t INNER JOIN Team t on m.team_id =t.id

