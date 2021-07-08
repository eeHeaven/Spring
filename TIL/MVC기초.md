**스프링 MVC의 전체적인 동작**             
![캡처](https://user-images.githubusercontent.com/84822464/124863943-fdea4300-dff2-11eb-8937-114d8691f780.PNG)

**스프링 MVC 구조**           
1. 컨트롤러 : Dispatcher에서 전달된 요청을 처리           
2. servlet-context.xml : 스프링 컨테이너 설정 파일           
3. web.xml : DispatcherServlet 서블릿 맵핑, 스프링 설정 파일 위치 정의          
4. DispatcherServlet : 클라이언트의 요청을 최초로 받아 컨트롤러에게 전달           
5. jsp파일: View파일 

