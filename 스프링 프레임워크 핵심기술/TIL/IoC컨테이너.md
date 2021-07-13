### 스프링 IoC 컨테이너와 빈

Inversion of Control: 의존 관계 주입이라고도 하며 어떤 객체가 사용하는 의존 객체를 직접 만들어 사용하는게 아니라 주입받아 사용하는 방법                   

**의존관계란?**        
******************************
```java
public class classA{
 private classB b;
 
 public classA(classB b){
 this.b =b;
 }
}
```
이와 같이 하나의 클래스 안에 다른 클래스(객체)가 변수로 들어가고 생성자의 매개변수로 받는  의존하는 형태를 의미한다.           

**************************************
#### 스프링 IoC 컨테이너     
 - 빈들을 담고 있는 공간        
 - BeanFactory(최상위 interface)                     
 - 애플리케이션 컴포넌트의 중앙 저장소         
 - 빈 설정 소스로부터 빈 정의를 읽어들이고, 빈을 구성하고 제공    
 - singleton으로 객체를 관리하고 싶을 때도 유용하게 사용                      

#### 빈 
스프링 IoC 컨테이너가 관리하는(컨테이너에 들어가 있는) 객체 

#### 빈의 장점
 - scope
    * 싱글톤: 유일한 단 하나의 객체(프로퍼티가 공유됨, ApplicationContext 초기 구동시 인스턴스 생성)                                    
    * 프로토타입: 매번 다른 객체(request: /Session: /WebSocket: )         
    프로토타입 빈이 싱글톤 빈을 참조하면 아무 문제 없지만 싱글톤 빈이 프로토타입 빈을 참조하려면 프록시를 사용해야 함        
    
    **프록시란?**         
    싱글톤 빈이 프로토타입 빈을 참조할 경우 참조하는 빈이 고정되기 때문에 프로토타입으로서 기능을 하지 않는 문제가 있음          
    따라서 이 경우 프록시로 프로토타입의 클래스를 감싸서 보호해주어 프로토타입 빈이 각각의 다른 객체로 참조될 수 있도록 해 줌                
    **@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)**          
    
 - 의존성 관리         
 - 라이프사이클 인터페이스        
빈이 만들어질 때 추가적인 작업을 하고 싶을 때 가능 (ex. @PostConstruct)      

**************************************************
### 빈 설정 방법 
#### 1. 빈 설정파일 만들기         
자바 클래스 파일 중 빈 설정 파일을 하나 지정한다.         
지정은 클래스 위에 annotation **(@Configuration)** 을 통해 지정한다.            
**@ComponentScan(basePackageClasses = 클래스경로)** 스캔 범위 지정        

**혹은, 그냥 @SpringBootApplication 하나만 annotation 넣어주면 됨**           

#### 2. 빈으로 설정할 클래스 위에 annotation붙이기         
@Component     
 - @Service            
 - @Repository     
 - @Controller        
 - @Configuration       

**Component와 컴포넌트 스캔에 대하여**       
컴포넌트 스캔의 주요 기능        
 - 스캔 위치 설정       
 - 필터: 어떤 어노테이션을 스캔할지 또는 하지 않을지           


#### 3. 의존 관계 지정     
빈 클래스에서 의존관계에 있는 변수 위에 annotation **(@Autowired)** 붙이기     
필요한 의존 객체의 타입에 해당하는 빈을 찾아 해당 어노테이션을 주입한다.             
autowired의 기본값은 true (따라서 못 찾으면 어플리케이션 구동 실패)       
반드시 autowired되는 클래스(객체)에는 빈 생성을 해주어야 함               
그러나 세터나 필드에 autowired를 쓰는 경우 의존관계에 있는 빈이 없어도(optional해도) 사용할 수 있도록 @Autowired(required = false)로 지정할 수 있음            

**Autowired 사용할 수 있는 위치**         
 - 생성자(스프링4.3부터는 생략 가능)             
 - 세터       
 - 필드           

**같은 타입의 빈이 여러개 일 때**        
**@Primary 어노테이션을 붙여서 가장 최우선으로 지정할 빈을 지정해주기**(권장)                      
혹은, 해당 타입의 빈 모두 주입 받기             
혹은, @Qualifier(쓰고싶은 빈 이름) 어노테이션 사용하기           


**************************************************
### ApplicationContext의 기능들    
가장 자주 쓰이는 beanfactory 인터페이스       

#### 1. Environment  
프로파일과 프로퍼티를 다루는 인터페이스          

```java
@Component
public class AppRunner implements ApplicationRunner{

@Autowired
ApplicationContext ctx;

@Override
public void run(ApplicationArguments args) throws Exception{
  Environment environment = ctx.getEnvironment(); // 주로 getEnvironment 함수 씀 
  }
}
```
**프로파일**          
빈들의 그룹으로 Environment의 역할은 활성화할 프로파일 확인 및 설정.           
프로파일 설정은 설정할 빈 클래스(@Configuration/@Component) 위에 추가 어노테이션으로 @Profile(프로파일명)          
혹은 메소드에 정의할 경우 @Bean에 추가 어노테이션으로 @Profile(프로파일명) 추가하면 됨          
프로파일은 (!-not/$-and/|-or) 사용 가능           

**프로파일 설정은 -Dspring profiles active="프로파일명"**          

**프로퍼티**         
다양한 방법으로 정의할 수 있는 설정값          
ex) environment.getProperty(app.name);         
프로퍼티의 우선 순위)               
 1. ServletConfig 매개변수          
 2. ServletContext 매개변수          
 3. JNDI        
 4. JVM 시스템 프로퍼티      
 5. JVM 시스템 환경변수        
**@PropertySource** 정의로 프로퍼티 추가 가능          

#### 2.MessageSource 
국제화 기능을 제공하는 인터페이스          
**getMessage**  메소드 주로 사용          
스프링부트를 사용한다면 별다른 설정 필요 없이 messages.properties 사용 가능       
 - messages.properties          
 - messages_ko_kr.properties 

#### 3.ApplicationEventPublisher 
이벤트 프로그래밍에 필요한 인터페이스 제공.           

**이벤트 만들기**           
1. MyEvent 클래스 만들기      
2. eventhandle 클래스를 만든 뒤 이벤트 핸들 시 취할 액션이 담긴 메소드에 @EventListener 어노테이션 붙이기 (해당 메소드는 event 객체를 파라미터로 받아야 함)           
3. applicationrunner 구현한 클래스에서 
```java
@Autowired
ApplicationEventPublisher publishEvent;

@Override
public void run(ApplicationArguments args) throws Exception{
  publishEvent.publishEvent(new MyEvent(생성자~~));
  }
}
```
이벤트를 순차적으로 실행하고 싶다면 @Order 사용           
비동기적으로 실행하고 싶다면 @Async와 함께 사용            

#### 4.ResourceLoader       
리소스를 읽어오는 기능           
 1. 파일 시스템에서 읽어오기  
 2. 클래스패스에서 읽어오기          
 3. URL로 읽어오기           
 4. 상대/절대 경로로 읽어오기          

getResource 메소드 써서 읽어오면 됨!              

