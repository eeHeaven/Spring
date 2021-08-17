## AOP

공통 관심 사항과 핵심 관심 사항의 분리          

### 시간 측정 AOP 등록
1. aop 패키지 만들기        
2. Component 빈 등록하기            
3. Aspect 어노테이션 붙이기     
4. 메서드에 around 어노테이션으로 해당 메서드 적용 범위 정하기        
5. 메서드의 파라미터는 ProceedingJoinPoint 객체이며 이는 핵심 관심 사항에 해당         
6. joinpoint 실행은 proceed() 메서드로    


```java
package hello.hellospring.aop;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class TimeTraceAop {
 @Around("execution(* hello.hellospring..*(..))")
 public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
 long start = System.currentTimeMillis();
 System.out.println("START: " + joinPoint.toString());
 try {
 return joinPoint.proceed();
 } finally {
 long finish = System.currentTimeMillis();
 long timeMs = finish - start;
 System.out.println("END: " + joinPoint.toString()+ " " + timeMs +
"ms");
 }
 }
}
```

![캡처](https://user-images.githubusercontent.com/84822464/129735905-4b0a9c9a-cc4c-4fc0-9e4a-e3bd7b2c96c6.PNG)
