### “그러고 보니 JAR 파일 하나로 실행할 수 있네?”

#### ● mvn package를 하면 실행 가능한 JAR 파일 “하나가" 생성 됨.
● spring-maven-plugin이 해주는 일 (패키징)                    
● 과거 “uber” jar 를 사용                       
○ 모든 클래스 (의존성 및 애플리케이션)를 하나로 압축하는 방법                                   
○ 뭐가 어디에서 온건지 알 수가 없음                            
■ 무슨 라이브러리를 쓰는건지..                                      
○ 내용은 다르지만 이름이 같은 파일은 또 어떻게?              
### ● 스프링 부트의 전략
○ 내장 JAR : 기본적으로 자바에는 내장 JAR를 로딩하는 표준적인 방법이 없음.                                                 
○ 애플리케이션 클래스와 라이브러리 위치 구분                 
○ org.springframework.boot.loader.jar.JarFile을 사용해서 내장 JAR를 읽는다.                      
○ org.springframework.boot.loader.Launcher를 사용해서 실행한다.             
