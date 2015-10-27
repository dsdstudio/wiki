ssl사설 인증서 만들기 


	keytool -genkey -alias <keystore_alias> -keyalg RSA -keystore <keystore_file>
	 
	 keytool -genkey -alias wenestidx -keyalg RSA -keystore p.key
	키 저장소 비밀번호 입력:  
	키 저장소 비밀번호가 너무 짧음 - 6자 이상이어야 합니다.
	키 저장소 비밀번호 입력:  
	새 비밀번호 다시 입력: 
	이름과 성을 입력하십시오.
	  [Unknown]:  Wenest
	조직 단위 이름을 입력하십시오.
	  [Unknown]:  Wenest
	조직 이름을 입력하십시오.
	  [Unknown]:  Wenest
	구/군/시 이름을 입력하십시오?
	  [Unknown]:  
	시/도 이름을 입력하십시오.
	  [Unknown]:  Seoul
	이 조직의 두 자리 국가 코드를 입력하십시오.
	  [Unknown]:  KR
	CN=Wenest, OU=Wenest, O=Wenest, L=Unknown, ST=Seoul, C=KR이(가) 맞습니까?
	  [아니오]:  Y

	<wenestidx>에 대한 키 비밀번호를 입력하십시오.
		(키 저장소 비밀번호와 동일한 경우 Enter 키를 누름):  
		
		

CSR 

	keytool -certreq -keyalg RSA -alias tomcat -file certreq.csr -keystore <your_keystore_filename>
	
	
