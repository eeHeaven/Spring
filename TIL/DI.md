**스프링을 이용한 객체 생성과 조립**           

객체 하나하나를 bean이라고 생각하기      
- xml파일에서 bean태그로 객체 생성하기          
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

-main class에서 xml과 연동하기 
```java
 String configLoc = "classpath:applicationCTX.xml";
        AbstractApplicationContext ctx = new GenericXmlApplicationContext(configLoc);     //스프링 컨테이너 (IOC) 생성
        MyCalculator myCalculator = ctx.getBean("mycCalculator", MyCalculator.class);     //스프링 컨테이너에서 컴포넌트 가져옴 
        ctx.close(); 
```

**xml에 참조해오는 class에서 접근하는 변수에 대해서는 반드시 set함수들이 전부 정의되어 있어야 함**               

- 생성자와 set함수 
생성자를 활용하는 방법과 set함수를 활용하는 방법 두 가지로 xml파일에서 class의 변수들을 설정해줄 수 있다.    


**생성자**         
```java
student(int id, String name, ArrayList hobbys){
this.id = id;
this.name = name;
this.hobbys = hobbys;} 식으로 class에 생성자가 선언되었을 때 

xml파일에서 
<bean id="student" class="com.javalec.ex.Student">
<constructor - arg value = "1" />
<constructor - arg value = "홍길동" />
<constructor - arg>
<list>
<value>수영</value>
<value>요리</value>
</list>
</constructor - arg>
</bean>
```
이와 같은 형태로 생성자를 활용하는 방법이 있음.           
이때 주의할 점은 constructor - arg에서 생성자의 parameter 순서 그대로 value 값을 넣어 주어야 한다는 것임           
파라미터의 자료형 상관없이 모두 value로 칭하고 넣을 수 있음             

**set함수**      
```java
student class에서 setId 와 setName 함수가 존재한다고 가정할 때 
xml파일에서
<bean id="student" class="com.javalec.ex.Student">
<property name="id" value = "1" />
<property name="name" value = "홍길동" />
<property name="hobbys">
<list>
<value>수영</value>
<value>요리</value>
</list>
</property>
</bean>
```
이때 반드시 set함수가 정의되어 있어야 함               
property 사용시 name ="변수명" 지정 필요         
마찬가지로 자료형 상관없이 모두 value로 칭하고 넣을 수 있음         


**main class에서는 자유롭게 새로운 객체를 ctx.getBean으로 가져와 생성할 수 있고          
기존 자바 문법에 자유롭게 해당 객체를 활용할 수 있음**            


**********************************
### spring 프레임워크의 장점
java 파일의 수정 없이 스프링 설정 파일만을 수정하여 부품들을 생성/조립할 수 있다.
************************************

**name space 간편하게 설정하는 방법**       
1. xml 파일 가장 상위 beans에 xmlns:c = "http://www.springframework.org/schema/c" 와 xmlns:p = "http://www.springframework.org/schema/p"추가
2. bean에서 객체 생성 시 생성자에서 변수 설정시 c:변수명, set함수에서 변수 설정시 p:변수명으로 간편하게 정의해주면 됨 

```java
<bean id="family" class="com.javalec.ex.Family" c:papaName="홍아빠" p:mamiName="홍엄마" /bean>
```

**여러 개의 xml파일을 GenericXmlApplicationContext 메소드의 파라미터(다중)로 한 번에 받아서 하나의 ctx로 다양한 xml안의 객체 접근이 가능하다.**      
