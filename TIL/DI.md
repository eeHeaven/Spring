**스프링을 이용한 객체 생성과 조립**           

객체 하나하나를 bean이라고 생각하기      
- bean태그로 객체 생성하기          
```xml 
<bean id = "myCalculator" class="com.javalec.ex.MyCalculator"/> //id로 변수 설정 
  <property name = "calculator"> //property로 필드 설정, myCalculator에 각 필드 setter 다 설정되어 있어야 함 
      <ref bean = "calculator"/> //ref로 다른 bean을 참조해 옴 
</property>
<property name = "firstNum" value = "10"></property>
<property name = "secondNum" value = "2"></property>
</bean>
```
**********
firstNum = setFirstNum()      
secondNum = setSecondNum()
***********



