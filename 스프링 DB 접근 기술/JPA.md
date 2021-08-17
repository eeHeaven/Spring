@SpringBootTest : 스프링 컨테이너와 테스트를 함께 실행한다.                
@Transactional : 테스트 케이스에 이 애노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고,                           
테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.           

## JPA
JPA는 기존의 반복코드는 물론이고 기본적인 SQL도 JPA 가 직접 만들어서 실행해준다.         
JPA를 사용하면 SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환할 수 있다             
JPA를 사용하면 개발 생산성을 크게 높일 수 있다.             

### build.gradle 파일에 JPA,h2 데이터베이스 관련 라이브러리 추가 
spring-boot-starter-data-jpa 는 내부에 jdbc 관련 라이브러리를 포함한다. 따라서 jdbc는 제거해도 된다.          
```java
dependencies {
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
implementation 'org.springframework.boot:spring-boot-starter-web'
//implementation 'org.springframework.boot:spring-boot-starter-jdbc'
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
runtimeOnly 'com.h2database:h2'
testImplementation('org.springframework.boot:spring-boot-starter-test') {
exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
}
}
```
### 스프링부트에 JPA 설정 추가 (application.properties)
```java
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.jpa.show-sql=true // JPA가 생성하는 SQL을 출력한다.
spring.jpa.hibernate.ddl-auto=none // JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 none 를 사용하면 해당 기능을 끈다.

```
**주의!: 스프링부트 2.4부터는 spring.datasource.username=sa 를 꼭 추가해주어야 한다. 그렇지 않으면 오류가 발생한다.**       

### JPA 엔티티 매핑
매핑할 엔티티 클래스에 @Entity 붙여주면 됨           
```java
package hello.hellospring.domain;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
@Entity
public class Member {
 @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
 private Long id;
 private String name;
 public Long getId() {
 return id;
 }
 public void setId(Long id) {
 this.id = id;
 }
 public String getName() {
 return name;
 }
 public void setName(String name) {
 this.name = name;
 }
}
```
### JPA 회원 리포지토리 
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import javax.persistence.EntityManager;
import java.util.List;
import java.util.Optional;

public class JpaMemberRepository implements MemberRepository {
  
 private final EntityManager em;
  
 public JpaMemberRepository(EntityManager em) {
 this.em = em;
 }
  
 public Member save(Member member) {
 em.persist(member);
 return member;
 }
  
 public Optional<Member> findById(Long id) {
 Member member = em.find(Member.class, id);
 return Optional.ofNullable(member);
 }
  
 public List<Member> findAll() {
 return em.createQuery("select m from Member m", Member.class)
 .getResultList();
 }
  
 public Optional<Member> findByName(String name) {
 List<Member> result = em.createQuery("select m from Member m where
m.name = :name", Member.class)
 .setParameter("name", name)
 .getResultList();
 return result.stream().findAny();
 }
}
```

### 서비스 계층에 트랜잭션 추가 
```java
import org.springframework.transaction.annotation.Transactional
@Transactional
public class MemberService {}
```
**JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.**            

### JPA를 사용하도록 스프링 설정 변경
```java
package hello.hellospring;

import hello.hellospring.repository.*;
import hello.hellospring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import javax.persistence.EntityManager;
import javax.sql.DataSource;

@Configuration
public class SpringConfig {
 private final DataSource dataSource;
 private final EntityManager em;
  
 public SpringConfig(DataSource dataSource, EntityManager em) {
 this.dataSource = dataSource;
 this.em = em;
}
  
 @Bean
 public MemberService memberService() {
 return new MemberService(memberRepository());
 }
  
 @Bean
 public MemberRepository memberRepository() {
// return new MemoryMemberRepository();
// return new JdbcMemberRepository(dataSource);
// return new JdbcTemplateMemberRepository(dataSource);
 return new JpaMemberRepository(em);
 }
}

```
DataSource는 데이터베이스 커넥션을 획득할 때 사용하는 객체다. 스프링 부트는 데이터베이스 커넥션 정보를 바탕으로 DataSource를 생성하고 스프링 빈으로 만들어둔다.       
그래서 DI를 받을 수 있다.               
