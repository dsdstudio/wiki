##CORS 

Cross Origin Resource Sharing 
다른 이기종 의 도메인간 통신을 위한 규약이다.
Browser security policy 때문에 XHR Request 를 보낼수 없는부분에 대한 지원 규약이다. 
아직 W3C Working draft 상태이며, IE8이상, 그밖의 모던 브라우저는 대부분의 버전이 지원하고있다. 자세한건 아래 Browser support comparison chart 를 참고하자. 
 

### Server Side 
Request Header 값 

	Access-Control-Allow-Origin: * 
	
헤더값만 추가하면 되므로 servlet filter 하나 만들어서 앞단에 붙여놓으면 사용하기 편할듯 하다. 



### Client Side 

Browser support 

IE7+, 그밖의 모던브라우저는 대부분 지원하고있다. 

[CORS Browser Support Comparison chart](http://caniuse.com/#search=cors)


### References 

- [cors](http://blog.iolo.kr/494)
- [enable-cors.org](http://enable-cors.org/index.html)
