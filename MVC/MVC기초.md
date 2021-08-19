**스프링 MVC의 전체적인 동작**             
![캡처](https://user-images.githubusercontent.com/84822464/124863943-fdea4300-dff2-11eb-8937-114d8691f780.PNG)

1. Dispatcher가 사용자의 요청을 받음 
2. 요청을 넘길 controller를 찾기 위해 servelt context에서 controller annotation이 있는 java파일을 스캔함 
3. Controller에게 요청을 넘김 
4. RequestMapping 되어있는 로직을 실행함 
![캡처](https://user-images.githubusercontent.com/84822464/125066741-50f4f080-e0ee-11eb-8a43-41c7ee5f4801.PNG)
5. 해당 로직에서 load 해야하는 view의 이름을 string 값으로 리턴해 줌 
6. 해당 string을 받아서 servelt context에서 view할 jsp파일 주소값을 생성
7. 주소에 따라 해당하는 view(jsp파일) 실행 

**주의사항**            
Dispatcher가 사용자의 요청을 받은 즉시 모두 controller에게 보내버림             
그렇기때문에 controller에 사용하지 않고 따로 쓰여야 하는 주소들은 미리 dispatcher가 가져가지 않도록 지정해주어야 함           
예를 들어 이미지 경로의 경우 이와 같이 따로 빼서 지정해주어야 한다             
지정방법) resources mapping             

****************************************
**스프링 MVC 구조**           
1. 컨트롤러 : Dispatcher에서 전달된 요청을 처리           
2. servlet-context.xml : 스프링 컨테이너 설정 파일           
3. web.xml : DispatcherServlet 서블릿 맵핑, 스프링 설정 파일 위치 정의          
4. DispatcherServlet : 클라이언트의 요청을 최초로 받아 컨트롤러에게 전달           
5. jsp파일: View파일 

************************************************************
**컨트롤러 클래스 제작**                  
컨트롤러는 요청에 대한 작업을 한 후 뷰쪽으로 데이터를 전달한다.            
![캡처](https://user-images.githubusercontent.com/84822464/125065229-8ac4f780-e0ec-11eb-9870-cc53b0875455.PNG)

 - Model 객체를 파라미터로 받는 방법 
![캡처](https://user-images.githubusercontent.com/84822464/125065817-41c17300-e0ed-11eb-8dde-c9bf2b886f4e.PNG)
1. 처음 요청이 Controller한테 옴 
2. Controller가 Model의 데이터를 처리함 
3. 처리한 Model 데이터를 View에 전송해줌          
이 때 Model객체는 Controller에서 requestmapping 된 메소드의 파라미터로 받는다.           
get이나 add 속성을 활용해서 Model 의 데이터를 controller가 처리한다.      
view에서 model객체의 속성을 이용하여 데이터를 출력한다.          

 - ModelAndView 클래스를 이용하는 방법 
![캡처](https://user-images.githubusercontent.com/84822464/125066439-f78cc180-e0ed-11eb-8b48-93dae84fec6f.PNG)
