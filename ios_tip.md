## UIKit frame, bounds 차이

frame은 절대좌표, size  
bounds 는 상대 좌표, size 

## 구버전 SDK 대응코드 작성하기

RESPONDSTOSELECTOR 로 기존에 존재하는 메소드인지 확인이 가능하다.

	if ([tableView respondsToSelector:@selector(setSeparatorInset:)]) {
    	[tableView setSeparatorInset:UIEdgeInsetsZero];
	}

## UIViewController 에서 tabbar 나 navigationbar 를 사용하는 view일때 기본설정

UI 를 직접그리는경우 navbar 의 height 를 계산해서 그려야하는데 아래 옵션을 주면 0,0 좌표계로 바로 작업할수있다.

	- (void) viewDidLoad {
    	// 포지셔닝 이옵션을 줘놓으면 navbar, tabbar 상관없이 개발이 가능 IOS7에서만
    	self.edgesForExtendedLayout = UIRectEdgeNone;
	}

## UILabel 그려보기전에 text Size 알아내기

	NSString *price = @"10,550";
	CGSize txtSize = [price sizeWithAttributes:@{NSFontAttributeName:[UIFont systemFontOfSize:18.0f]}];
    // do something


## NSCoding Boolean decode

## Push Notification 시 인증서만들기


Certificates - APNS for IOS, `apns_xxx.cer`

키체인접근 - 인증서- Apple IOS Push Services 개인키와 얽혀있는 인증서를 내보내기한다.  `key.p12`


### pem 파일 만들기

위의 2 파일로 cert.pem, key.pem 파일을 생성한다.

	$ openssl x509 -in apns_xxx.cer -inform DER -outform PEM -out cert.pem
	$ openssl pkcs12 -in key.p12 -out key.pem -nodes


### 생성된 pem 인증서 확인
	$ openssl s_client -connect gateway.sandbox.push.apple.com:2195 -cert YourSSLCertAndPrivateKey.pem -debug -showcerts -CAfile server-ca-cert.pem

### NodeJs 에서 PUSH 메시지 보내보기

	준비중 
