### JUnitParames
테스트에 대하여 매개변수에 따라 각 테스트의 결과값을 다르게 보고 싶을 때 테스트 코드의 중복 작성을 보완해주는 기능

### JUnitParames 사용법
#### 1.의존성 추가하기
Mvn 기준 
```java
&lt;!-- https://mvnrepository.com/artifact/pl.pragmatists/JUnitParams
--&gt; &lt;dependency&gt;
&lt;groupId&gt;pl.pragmatists&lt;/groupId&gt;
&lt;artifactId&gt;JUnitParams&lt;/artifactId&gt;
&lt;version&gt;1.1.1&lt;/version&gt;
&lt;scope&gt;test&lt;/scope&gt; &lt;/dependency&gt;
```
#### 2. 단위테스트에서 사용하기
@Parameters 에 직접 각 파라미터 값을 주는 방법                
```java

@RunWith(JUnitParamesRunner.class)
public class PersonTest{

@Test
@Parameters({"17, false","22, true"})
public void PersonIsAdult(int age, boolean isAdult)throws Exception{
  assertThat(new Person(age).isAdult(), is(isAdult));
}
```
paramesFor OOO 형태의 Object배열로 파라미터 값을 각각 주는 방법
```java
@RunWith(JUnitParamesRunner.class)
public class PersonTest{

@Test
@Parameters
public void PersonIsAdult(int age, boolean isAdult)throws Exception{
  assertThat(new Person(age).isAdult(), is(isAdult));
}

private Object[] paramesForPersonIsAdult(){
return new Object[] {
                      new Object[] {17,false},
                      new Object[] {22, true}
};
}
```
