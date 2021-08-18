### ApplicationEvent 등록
**○ ApplicationContext를 만들기 전에 사용하는 리스너는 @Bean으로 등록할 수 없다.**        
=> ApplicationStartingEvent 가 대표적.       
이렇게 bean으로 등록할 수 없는 event 리스너는 SpringApplication에서 직접 설정해 주어야 함.          
```java
@SpringBootApplication
public class SpringinitApplication{
   public static void main(String[] args){
    StringApplication app = new StringApplication(SpringinitApplication.class);
    app.addListeners(new SampleListener()); // ApplicationStartingEvent 리스너 직접 주입하기(bean 설정 안됨)
    app.run(args);
   }
}
```
이외의 것들은 따로 addListener 없이도 bean으로 등록만 하면 자동 사용 가능          

### 애플리케이션 아규먼트 사용하기
○ ApplicationArguments를 빈으로 등록해 주니까 가져다 쓰면 됨.           
VM options는 -D를 붙여 사용, 일반 arguments는 --붙여서 사용하면 된다           
이때 VM options 에 있는 값은 ApplicationArguments로 등록 안되고 program arguments 에 있는 값만 ApplicationArguments를 통해 불러올 수 있음           

● 애플리케이션 실행한 뒤 뭔가 실행하고 싶을 때            
○ ApplicationRunner (추천) 또는 CommandLineRunner                     
○ 순서 지정 가능 @Order             
