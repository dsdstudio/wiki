## 용어 정리 


## [헝가리안 노테이션](http://en.wikipedia.org/wiki/Hungarian_notation) 

### Prefix 정리  


Type별 prefix 

- bResult : boolean 
- chInitial : char 
- cApples : count of items 
- nSize : integer 혹은 count 
- iSize : integer 혹은 index 
- pFoo : Foo 객체에대한 포인터 
- szLastName : (zero-terminated string) 
- stTime : time 
- fnFunction :functionName 
- oReport : Object instance (이건 회사에있을때 자주보던 패턴)
- hwndFoo : window 핸들 (MFC에서)
- lpszBar : zero-terminated string에 대한 long type 포인터 
- g_nWheels : 전역 멤버 이름공간으로 사용하는경우 (integer)
- m_nWheels : Class 혹은 structure 멤버변수 (integer) 
- s_wheels : 정적(static) 멤버변수

##### 이름별 의미 

psz ( Pointer to zero-terminated string)
	
*  보통 문자열의 끝이 `0`으로 끝나는 타입변수의 포인터의 이름으로 사용한다.

예제 	 
	 TCHAR *pszBuffer;

## References 

- [MSDN Hungarian notation](http://msdn.microsoft.com/en-us/library/aa260976(VS.60).aspx)
 
