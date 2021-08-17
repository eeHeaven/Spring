## 스프링 데이터 JPA
스프링 부트와 JPA만 사용해도 개발 생산성이 정말 많이 증가하고 개발해야 할 코드도 확연히 줄어든다.           
여기서 스프링 데이터 JPA를 사용하면 기존의 한계를 넘어 **리포지토리에 구현 클래스 없이 인터페이스 만으로 개발을 완료할 수 있다.**           
그리고 반복 개발해온 기본 CRUD 기능도 스프링 데이터 JPA가 모두 제공한다            

### 스프링 데이터 JPA 회원 리포지토리 
리포지토리에 단순히 JpaRepository와 기존 repository 인터페이스를 extends 하도록        
JpaRepository가 SpringDataMemberRepository 구현체를 자동으로 생성하고 자동으로 스프링 빈으로 등록해놓는다.           
```java
package hello.hellospring.repository;
import hello.hellospring.domain.Member;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

public interface SpringDataJpaMemberRepository extends JpaRepository<Member,Long>, MemberRepository {
 Optional<Member> findByName(String name);
}
```
***************
findByName에 대하여            
JPA 에서 기본적인 findAll, save, delete 등은 지원을 해주지만        
name과 같이 entity 클래스 변수는 유동적이라서 해당 변수로 무언가 조회할 때는 개발자가 직접 설정을 해주어야 하는데           
이것도 findByOOO 형태로 맞춰서 설정해주면 굳이 메서드 다 구현하지 않고 위와 같이 메서드 명만 적어주면               
알아서 JPQL 언어로 select m from Member m where m.name =? 이렇게 변환해서 메서드를 만들어준다.                
이 경우 인터페이스 이름 만으로도 개발이 끝나게 된다! 완전 간편               
***********************


### 스프링 데이터 JPA 회원 리포지토리를 사용하도록 스프링 설정 변경 
memberRepository에 SpringDataJpaMemberRepository 구현체가 자동 등록이 된다.          
```java
package hello.hellospring;
import hello.hellospring.repository.*;
import hello.hellospring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {

 private final MemberRepository memberRepository;
 
 public SpringConfig(MemberRepository memberRepository) {
 this.memberRepository = memberRepository;
 }
 
 @Bean
 public MemberService memberService() {
 return new MemberService(memberRepository);
 }
}
```

