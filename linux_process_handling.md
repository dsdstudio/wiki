## Li(u)nix Process handling 

	$> kill -l 
	 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL
	 5) SIGTRAP	 6) SIGABRT	 7) SIGEMT	 8) SIGFPE
	 9) SIGKILL	10) SIGBUS	11) SIGSEGV	12) SIGSYS
	 13) SIGPIPE	14) SIGALRM	15) SIGTERM	16) SIGURG
	 17) SIGSTOP	18) SIGTSTP	19) SIGCONT	20) SIGCHLD
	 21) SIGTTIN	22) SIGTTOU	23) SIGIO	24) SIGXCPU
	 25) SIGXFSZ	26) SIGVTALRM	27) SIGPROF	28) SIGWINCH
	 29) SIGINFO	30) SIGUSR1	31) SIGUSR2
	

kill 명령어 

	kill [option] [processid]
	## signal index로 kill 하는경우 
	kill -3 48587
	## signal Name으로 kill하는경우 
	kill -SIGTERM 48587 
	

Java process의 경우 인자없는 kill 명령을 받는경우 `SIGTERM` 을 받는다. 또한 exit code 를 별도로 명시하지않은 경우에 java process exit code 는 `143`을 리턴하도록 되어있다.(Linux / unix계열) 


kill -9 를 쓰지말아야할 이유 

프로세스가 종료될때 리소스들을 정리할수없다. 

- 소켓연결 
- 임시파일 정리 
- child process혹은 Thread 들의 종료 통지


Bash kill signal 날리고 종료될때까지 기다리기 

	kill $PID
	wait $PID
	
LogRotate

