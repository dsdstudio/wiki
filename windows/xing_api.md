## Xing API 

Etrade 증권에서 사용하는 Xing API 사용방법을 정리한다. 

기본적으로 Visual c++ 6.0 에서 빌드한 dll 과 헤더파일을 제공하고있다.
 
다른버전의 visual studio 에서 사용하려면 설정과 싸워야하므로 될수있으면 visual c++ 6.0 에서 진행하는것이 정신건강에 좋을거예요 



dll 로딩 / 초기화 -> 서버 접속 -> 로그인 -> 조회 TR / 혹은 실시간 TR 

DLL 로딩 및 초기화 함수 
	
	g_iXingAPI.Init(NULL); 
	

서버 접속 

	g_iXingAPI.Connect();
	
Login 

	g_iXingAPI.Login() 
	

조회 TR 

	g_iXingAPI.Request()
	
실시간 TR 

	g_iXingAPI.AdviseRealdata()
	
	
	
## 패킷 데이터 다루기 
