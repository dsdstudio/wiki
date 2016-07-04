# Emacs 관련 정보 정리 :)

이문서는 제가 emacs를 사용하면서 까먹지않기 위해 정리해놓는 문서입니다. 
참고로 제 PC환경은 MacOSX el cappitan 이며, homebrew를 사용합니다. 

emacs를 사용할때 원하는 기능적인 목표 

- emacs에서 textmate나 alpred 같은 fuzzy-finder 기능이 제공됐으면 좋겠다. 플러그인이던 뭐던, 
- 요즘 자주 사용하고 있는 언어는 javascript인데 emacs에서 제대로 indent, spacebar들을 컨트롤 가능했으면 좋겠다. 
- git 환경도 emacs에 통합시키고 싶다.
- find files in project 


emacs를 사용할때 내가 할수있었으면 하는 작업들 

- 세로 편집 
- 화면 쪼개기, 화면 크기조정 


## 00. 설치

homebrew를 이용해서 설치

```
 $ brew install --with-cocoa --srgb emacs
```

- `--with-cocoa` 옵션은 gui용 emac를 같이 설치하기위해서 필요한 옵션입니다. 
- `--srgb` 옵션은 GUI환경에서 sRGB 컬러를 지원하기 위해서 필요합니다.

설치가 끝나고나면 gui 실행용 바이너리를 링크합니다.

```
 $ brew linkapps emacs
```


## 01. 환경설정


### emacs 기본 설정 파일 경로 

`~/.emacs`, `~/emacs.d/init.el`

### emacs daemon

http://www.emacswiki.org/emacs/EmacsAsDaemon

### cider 설정을 위한 project.clj 내용 추가


project.clj 

```
:plugins [[cider/cider-nrepl "0.12.0"]]
```

상단 내용이 추가되어있어야 cider 에서 테스트 실행시 우아하게 연결이 된다



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
```

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
- [디렉토리 기준으로 검색가능한 기능을 제공하는 fiplr](https://github.com/grizzl/fiplr)
