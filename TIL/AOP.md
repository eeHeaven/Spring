**AOP란?**         

프로그래밍을 하다 보면, 공통적인 기능이 많이 발생한다. 이러한 공통 기능을 모든 모듈에게 적용하기 위한 방법으로 상속을 통한 방법이 있다.         
상속도 좋은 방법이기는 하지만 자바는 다중 상속이 불가능하므로 다양한 모듈 상속기법을 통한 공통기능부여는 한계가 있다.                   
그리고 기능 구현부분에 핵심 기능 코드와 공통 기능 코드가 섞여 있어 효율성이 떨어진다.              
이에 따라 **핵심 기능과 공통 기능을 분리 시켜놓고, 공통 긴으을 필요로 하는 핵심 기능들에서 사용하는 AOP**가 등장한다.      
즉, AOP는 공통기능을 핵심기능과 분리해놓고, 공통 기능 중에서 핵심 기능에 적용하고자 하는 부분에 적용한다.                  

**********************************
**용어정리**               
1. Aspect : 공통 기능
2. Advice : Aspect의 기능 자체 
3. JointPoint : Advice를 적용해야 하는 부분(ex.필드 메소드)
4. Pointcut : JointPoint의 부분으로 실제로 Advice가 적용된 부분
5. Weaving : Advice를 핵심 기능에 적용하는 행위 
******************************************

**XML 기반의 AOP 구현**         

1. pom.xml에 AOP를 구현하겠다는 의미로 의존설정 필요 
```java
<!-- AOP -->
<dependency>
  <groupId>org.aspectj</groupID>
  <artifactId>aspectjweaver</artifactId>
  <version1.7.4</version>
 </dependency>
```
2. 공통기능 클래스 제작(Advice역할 클래스)
```java
public class LogAop{
  public Object loggerAop(ProceedingJoinPoint jp)throws Trowable{  //핵심 기능들을 jp로 가져옴
    공통 기능 내용들 적으면 됨~~~~
    try{
      Object obj = jp.proceed(); //핵심 코드를 실행시킴 
      return obj;
      }
      공통 기능 내용들 적으면 됨~~~~
      }
```

3. XML설정 파일에 Aspect 설정
```java
<bean id="logAop" class="com.javalec.ex.LogAop"/>
<aop:config>
  <aop:aspect id="logger" ref="logAop">
    <aop:pointcut id="publicM" expression="within(com.javalec.ex")"/>  //expression은 pointcut의 범위 com.javalec.ex안의 모든 클래스의 모든 메서드에 공통기능을 넣겠다. 
    <aop:around pointcut-ref="publicM" method="loggerAop"/> 
   </aop:aspect>
 </aop:config>
```


************************
**Advice 종류**           
1. aop:before - 메소드 실행 전에 advice 실행
2. aop:after-returning - 정상적으로 메소드가 실행 후에 advice 실행
3. aop:after-throwing - 메소드 실행 중 exception이 발생 했을때 advice 실행
4. aop:after - 메소드 실행 중 exception이 발생하여도 advice 실행
5. aop:around - 메서드 실행 전/후 및 exception 발생 시 advice 실행 
***************************

4. 자바클래스 파일에 Aspect 설정
```java
@Aspect // annotation으로 설정 가능 
public class LogAop{
  
  @Pointcut("within/com.javalec.ex.*)")
  private void pointcutMethod(){
  }
  
  @Around("pointcutMethod()")
  public Object loggerAop(ProceedingJoinPoint joinpoint) throws Throwable{
  ~~~ } //공통 제작 클래스 
```

5. AspectJ Pointcut 표현식                
**Execution**         
```java
@Pointcut("execution(public void get*(..))") //public void인 모든 get메소드           
@Pointcut("execution(* com.javalec.ex.*.*())") //com.javalec.ex 패키지에 파라미터가 없는 모든 메소드   
@Pointcut("execution(* com.javalec.ex..*.*())") //com.javalec.ex 와 그의 하위 패키지에 파라미터가 없는 모든 메소드         
@Pointcut("execution(* com.javalec.ex.Worker.*())") //com.javalec.ex.Worker 모든 메소드     
```
**within**  
```java
@Pointcut("within/com.javalec.ex.*)") //패키지 안에 있는 모든 메소드     
@Pointcut("within/com.javalec.ex..*)") //패키지 및 하위 패키지 안에 있는 모든 메소드          
@Pointcut("within/com.javalec.ex.Worker*)") // com.javalec.ex.Worker 모든 메소드    
```
**bean**        
```java
@Pointcut("bean(student)") // student 빈에만 적용      
@Pointcut("bean(*ker)") // ~ker로 끝나는 빈에만 적용          
```

