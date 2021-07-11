### 사용자로부터 요청 받아오는 방법 
**********************************************
**1. HttpServletRequest 클래스**           
![캡처](https://user-images.githubusercontent.com/84822464/125188269-41041a80-e26e-11eb-97c9-f42d19aa1a33.PNG)
 - Model 클래스와 객체:Controller에서 RequestMapping된 메소드 내에서 데이터를 처리하여 view jsp파일에 보낼 객체            
 - HttpServeltRequest 클래스와 객체: 사용자 입력 요청 명령어를 Controller에서 RequestMapping된 메소드에 보내고 그곳에서 가져오기 위해 사용하는 객체      
 
 **2. RequestParam 어노테이션**            
 ![캡처](https://user-images.githubusercontent.com/84822464/125188427-b53ebe00-e26e-11eb-8fac-82fd05acc820.PNG)
 Controller의 RequestMapping메소드의 파라미터에서       
 @RequestParam(Request에서 받은 파라미터 변수명)/RequestMapping메소드에서 새로 지정할 변수타입/ 변수명 순으로 작성     
 
 **3. 데이터(커멘드)객체**
![캡처](https://user-images.githubusercontent.com/84822464/125188626-5f1e4a80-e26f-11eb-93d0-2f1691821e99.PNG)
Member 객체를 커멘드 객체라고 부름      

**4. PathVariable 어노테이션**            
경로에 변수를 넣어 요청메소드에서 파라미터로 이용할 수 있다.
![캡처](https://user-images.githubusercontent.com/84822464/125188764-f2f01680-e26f-11eb-87e7-3acd29e87f26.PNG)

 ****************************************************
 
 ### 폼데이터 값 검증     
 **************************************
 **1. Validator를 이용한 검증**           
 ![캡처](https://user-images.githubusercontent.com/84822464/125189263-351a5780-e272-11eb-9bdb-193e34ecb152.PNG)
커멘드 객체로 controller에서 폼데이터를 받을 때 그 값의 변수타입이 일치하는지 확인하는 객체                
![캡처](https://user-images.githubusercontent.com/84822464/125189608-d48c1a00-e273-11eb-8f6e-e41cc24014fb.PNG)
1. validator 클래스 파일 생성하고 validate 메소드 오버라이드하기      
2. validate 메소드에서 원하는 조건 작성하기       
3. controller requestMapping 메소드에서 파라미터로 받은 요청에 따라 validator가 error나는지 작성하기      
이때 requestMapping 메소드에 파라미터로 BindingResult 받아줘야 함          


 **2. @Valid와 @InitBinder 사용**               
![캡처](https://user-images.githubusercontent.com/84822464/125189771-bd016100-e274-11eb-9443-227263d81ce0.PNG)
