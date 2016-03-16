## Windows Socket Programming 


Winsock2.h 를 사용해서 콘솔 서버를 만들어본다. 

사용편의를 위해 precompiledheader인 stdafx.h 에 winsock2.h를 include 

	#include <WinSock2.h>
	
하지만 이대로 빌드하게되면 라이브러리를 못찾는 문제가 있다. 
Visual Studio 에서 Project - properties - linker - commandline Additional options 에 `ws_32.dll` 을 추가하자. 이렇게 설정하고나면 정상적으로 빌드가 된다. 

