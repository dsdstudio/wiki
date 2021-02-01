
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

### ssh session timeout 시 freezing 되는 문제 해결 

- `Enter`, `~`, `.` 
  - https://superuser.com/questions/98562/way-to-avoid-ssh-connection-timeout-freezing-of-gnome-terminal

### mouse on 되어있는경우 drag-copy가 안되는데 이걸 명시적으로 조정하는 방법 

- `Ctrl + B`로 tmux command 진입 
- `:` 콜론으로 커맨드 입력 
- `set -g mouse off` 명령어로 mouse 끄고 켜고가 가능..

- https://stackoverflow.com/questions/17445100/getting-back-old-copy-paste-behaviour-in-tmux-with-mouse

#### references 

- [powerline installation guide](https://powerline.readthedocs.io/en/latest/installation.html#pip-installation)

