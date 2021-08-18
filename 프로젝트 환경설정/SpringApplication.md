## SpringApplication의 위치 
위치는 java 패키지 아래 패키지를 따로 생성한 뒤에 그 안에 넣어주는 것이 좋다.            
왜냐하면 SpringApplication은 Component Scan 을 자동으로 실행하여 해당 어플리케이션이 속한 패키지 안의 모든 bean을 등록하게 되기 때문에.         
만약 root 패키지에 application이 있으면 온갖 bean을 다 스캔하고 등록해서 골치아파지고 프로그램도 무거워진다..           

