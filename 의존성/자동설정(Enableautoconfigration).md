## 자동 설정 이해 

#### @EnableAutoConfiguration(@SpringBootApplication 안에 내장되어 있음)
bean은 두 단계로 나눠서 읽힘           
 - 1단계: @ComponentScan             
 - 2단계: @EnableAutoConfiguration           

#### @ComponentScan
 - @Component         
 - @Configuration @Repository @Service @Controller @RestController           

#### @EnableAutoConfiguration
 - spring.factories(META-INF하위) -> org.springframework.boot.autoconfigure.EnableAutoConfiguration(key값)의 value들이 bean으로 conditional 조건에 따라 설정됨                
 - @Configuration             
 - @ConditionalOnXxxYyyZzz               

## 자동설정 만들기

#### 1. 자동설정으로 추가할 프로젝트의 의존성 설정 
```java
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-autoconfigure</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-autoconfigure-processor</artifactId>
<optional>true</optional>
</dependency>
</dependencies>
<dependencyManagement>
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-dependencies</artifactId>
<version>2.0.3.RELEASE</version>
<type>pom</type>
<scope>import</scope>
</dependency>
</dependencies>
</dependencyManagement>
```

#### 2. 1에서의 프로젝트에 @Configuration 파일 작성
#### 3. 1에서의 프로젝트에 rc/main/resource/META-INF에 spring.factories 파일 만들기
```java
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
<2에서 만든 Configuration 파일 경로>
```
#### 4. mvn install
로컬 maven 저장소에 해당 프로젝트의 jar 파일이 설정됨           
다른 프로젝트가 접근할 수 있도록             

#### 5. 자동설정을 받을 프로젝트의 의존성에 주입하기 
자동설정으로 추가할 프로젝트의 pom 파일에 install 후 관련 의존성이 생김           
이를 복사해서 그대로 자동설정 받을 프로젝트에 붙여넣기 하면 됨              

이러면 자동설정에서 configuration파일에서 등록되어져 있던 bean들이 받은 프로젝트에도 @Autowired를 통해 그대로 설정된다.         

### 이때 발생하는 문제와 해결방안
#### 문제점: component 스캔 bean이 enableautoconfiguration 에 의해 덮어쓰기 당해버림
자동설정을 받은 파일에서 자체적으로 component 빈을 생성했는데 그게 자동설정 받은 bean과 겹치는 경우          
자동설정 받은 bean으로 덮어쓰기가 되어 프로젝트 내의 자체적인 component는 사용할 수 없게 됨          
**따라서 component 스캔이 우선순위가 되도록 설정해 주어야 함**        

#### 해결방안: 자동설정 프로젝트에서 configuration 파일에 "Conditional" 조건 붙여주기          
ConditionalOnMissingBean 에노테이션을 붙여주게 되면 자동설정 받는 파일에서 해당하는 bean이 없을 때만 해당 bean이 주입 됨         


### !Bean으로 주입받지 않고 자동설정 받는 프로젝트에서 properties로 즉각 내용 수정 가능!
#### 1. 자동설정으로 추가할 프로젝트에 ConfigurationProperties("이름") 파일 설정 
#### 2. 자동설정으로 추가할 프로젝트에 필요한 의존성 추가하기 
```java
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-configuration-processor</artifactId>
 <optional>true</optional>
 </dependency>
```
#### 3. Configuration파일에 EnableConfigurationProperties(1번파일 클래스명.class) 어노테이션 추가 
#### 4. Configuration 파일의 bean 생성자에 1번파일의 properties를 인자로 받아 bean 생성하기       
#### 5. 자동설정을 받을 프로젝트의 resources 폴더 아래 application.properties 생성 후 "이름".변수 = OO 형식으로 bean 객체 변수 set



