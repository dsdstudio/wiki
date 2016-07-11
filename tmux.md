
Ctrl+b based 

Tmux 시작하기 

	$ tmux -S {socketname}


Tmux 연결 끊기 

	Ctrl + b d   (detach)


화면 추가생성 

	Ctrl + b c 


화면 이름 변경 

	Ctrl + b ,


화면 삭제 

	Ctrl + b &


화면 이동 

	Ctrl + b n | p


화면 분할 

	Ctrl + b "

화면 분할 취소 

	Ctrl + b !

분할 화면간 이동 

	Ctrl +b o
	
화면 분할 배치 패턴 변경 

	Ctrl +b space
	
화면 스크롤링 

	Ctrl + b  [ 
	나갈때는 q 로 나갈수있음
	
화면 크기 변경하기 

	Ctrl + b, alt + 화살표키


분할된 화면중 현재화면을 전체화면으로 보기 

	Ctrl + b, z 
	복구도 마찬가지방법으로 가능 


### tmux powerline 설정

```
$ pip install --user psutil powerline-status
```

#### references 

- [powerline installation guide](https://powerline.readthedocs.io/en/latest/installation.html#pip-installation)

