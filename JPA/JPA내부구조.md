## 영속성 컨텍스트
1차 캐시에서 관리되는 것을 영속성 상태라고 한다             
### 엔티티 조회, 1차 캐시 
1차 캐시는 글로벌 캐시가 아니기 때문에 쓰레드 하나 시작하고 끝날 때까지 잠깐 쓰는 캐시          
트렌젝션이 생기고 끝나기 전까지만 유지되는 캐시      
엔티티를 조회하면 1차 캐시에서 우선적으로 해당 엔티티를 찾는다.          
만약 1차 캐시에 해당 엔티티가 없다면 그제서야 DB에서 해당 값을 찾고 이를 그냥 반환하는 것이 아니라,       
1차 캐시에 다시 넣었다가 반환한다.         

### 영속 엔티티의 동일성 보장 
```java
Member a = em.find(Member.class, "member1");
Member b = em.find(Member.class, "member1");

System.out.println(a==b); //true 나옴 (동일성 보장)
```
### 엔티티 수정 - 변경감지
어떤 필드의 데이터가 변경되었는지 전부 자동으로 감지함               
```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
transaction.begin();

Member memberA = em.find(Member.class,"memberA");

memberA.setname("hi");
memberA.setage(10);

//em.update 같은거 안해줘도 됨!!
transaction.commit(); 
```
영속컨텍스트에서 엔티티와 스냅샷 비교             
변경된 값이 감지되면 update sql 생성 후 쓰기 지연 SQL 저장소에 보냄              
트렌젝션 commit시 flush되어 쓰기 지연 SQL 저장소에 있던 SQL이 DB로 날라감            
commit으로 1차 캐시에 있던 값이 DB로 날라감            

>flush: DB에 쿼리를 반영         
>clear: 영속컨텍스트 안에 있는 파일들을 다 지워버림            

### 트랜젝션을 지원하는 쓰기 지연

```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
//엔티티 매니저는 데이터 변경시 트랜젝션을 시작해야 한다. 
transaction.begin();

em.persist(memberA);
em.persist(memberB);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않음 

//커밋하는 순간 데이터베이스에 INSERT SQL이 flush 됨
transaction.commit();
```
### 플러시 발생 
 - 변경 감지          
 - 수정된 엔티티 쓰기 지연 SQL 저장소에 등록           
 - 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송(등록, 수정, 삭제 쿼리)           

### 영속성 컨텍스트를 플러시 하는 방법
 - em.flush() : 직접 호출          
 - 트랜젝션 커밋: 플러시 자동 호출          
 - JPQL 쿼리 실행: 플러시 자동 호출       

### 플러시 사용시 주의사항 
 - 영속성 컨텍스트를 비우지 않음       
 - 영속성 컨텍스트의 변경내용을 데이터베이스에 동기화              
 - 트랜젝션이라는 작업 단위가 중요 -> 커밋 직전에만 동기화 하면 됨            

## 준영속 상태
영속성 컨텍스트에서 관리가 안되는 상태(영속성 컨텍스트가 제공하는 기능을 사용 못함)        
 - em.detach(entity) : 특정 엔티티만 준영속 상태로 전환      
 - em.clear() : 영속성 컨텍스트 초기화         
 - em.close() : 영속성 컨텍스트 종료           

### 주의사항: 준영속 상태일때는 update나 lazy 지연이 적용 안되기 때문에 영속컨텍스트 상태를 확인하기 
