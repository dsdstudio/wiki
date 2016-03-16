## MFC 

MFC Application 에서 console 창에 stdout 로그 남기기 

	#pragma comment(linker, "/entry:WinMainCRTStartup /subsystem:console") 
	
stdafx.h

공통적으로 사용하는 라이브러리들을 넣어놓는 헤더 파일이다. 


Dialog 여러개 사용하기 


CButton 비활성화 시키기 

	m_ctrlLoginBtn.EnableWindow(FALSE);
	

CString to LPTSTR 

	CString strData;
	strData.Format("mel");
	
	LPTSTR strConv;
	
	strConv = strData.GetBuffer(strData.GetLength());

CListBox Scrolling 하기 

