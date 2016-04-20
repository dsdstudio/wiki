## Emacs 관련 정보 정리 :)

### emacs 기본 설정 파일 경로 

`~/.emacs`, `~/emacs.d/init.el`

### emacs daemon

http://www.emacswiki.org/emacs/EmacsAsDaemon


emacs 서버 데몬 띄우기 

```
emacs --daemon
```

클라이언트로 붙어서 사용하기

```
emacsclient -nw <filename>
```

GUI client 로 붙어서 사용하기
```
emacsclient -c <filename>
```
emacs daemon 죽이기

```
emacsclient -e '(kill-emacs)'
```


### 단축키 정리 

#### 버퍼 열기

```
C-x
```

#### 버퍼 닫기

```
C-x k
```

#### 창간 이동 

```
C-x o
```

#### 윈도우 닫기 
```
C-x 0 (선택된 윈도우 닫기)
```

```
C-x 1 (모든 창 닫기)
```

#### 검색

```
C-s
```

#### 특정 라인으로 가기 

```
M-g g
GotoLine
```

#### undo & redo

```
C-/ (언두) 
C-_ (리두)

#### 선택한 라인 indent 처리 

```
C-u TAB
```

#### 기본 디렉토리 설정하기 

```
M-x cd
```

#### 전체 선택 

```
C-x C-p
```

### 내가 설치한 플러그인들

- https://github.com/n3mo/cyberpunk-theme.el (colortheme)
- [magit](http://magit.vc/)
- [helm fuzzy finder](https://emacs-helm.github.io/helm/)
- cask pack manager
