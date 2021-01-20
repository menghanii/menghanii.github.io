---
title: Naver Boostcamp Day+1 [1] &#58; Basic Computer Class for Newbies
comments: true
categories: [boostcamp]
tags: [부스트캠프, 프로그래밍]
math: true
---

\- 이 강의정리본은 가천대학교 최성철 교수님의 강의를 정리한 것임을 밝힙니다. \-

### 컴퓨터 OS

OS(운영체제; Operating System) : 프로그램이 동작할 수 있는 구동환경을 의미한다. 컴퓨터는 Hardware 와 Software로 이루어져 있는데, Hardware는 CPU, Memory 등 물리적인 실체가 있는 부분을 가리키고, Software는 Windows, Mac과 같은 Operating System, Chrome, Slack, Python 등과 같은 Applications 등을 포함하는 물리적인 실체가 없는 부분이다.

파일들은 일반적으로 운영체제에 의존적이다. 예를 들어, exe 확장자를 가진 파일들은 Mac OS에서 구동되지 않고, Windows 에서만 구동된다. (그러나 후술할 Python은 운영체제 독립적이다.)



### 파일시스템(File System)

#### File System 

- OS에서 파일을 저장하는 **트리 구조**의 저장체계를 의미한다. 트리 구조라 함은 Windows 탐색기에서 특정 폴더를 열었을 때 또 다른 폴더, 파일들이 가지치듯이 나오는 구조를 의미한다고 생각하면 된다.

#### 파일과 디렉토리 

- 디렉토리(Directory)는 우리가 '폴더'라고 알고 있는 것의 일반적인 명칭이다. 디렉토리는 파일과 다른 디렉토리를 포함할 수 있다.
- 파일(File)은 컴퓨터에서 정보를 저장하는 논리적인 단위를 의미한다. 파일은 기본적으로 **파일명 + 확장자**로 구성된다. 예를 들어, hello.py라는 파일은 'hello'라는 파일명과 'py'라는 확장자로 이루어져 있다. 파일은 실행, 쓰기 및 읽기가 가능하다.

#### 절대경로와 상대경로

- 절대경로는 파일의 고유한 위치를 나타낸다. 즉, 루트 디렉토리에서부터 파일 위치까지의 경로를 모두 출력한다. 예를 들어 `C` 드라이브의 `Users` 디렉토리 내의 `Workspace` 디렉토리 내의 `abc.py` 라는 파일의 절대경로는 `C:Users\Workspace\abc.py` 로 나타난다.
- 상대경로는 현재 내가 위치한 디렉토리로부터 파일이 있는 곳까지의 경로를 의미한다. 예를 들어, 내가 이미 `Workspace` 디렉토리에 있고, `abc.py`파일을 찾고자 한다면 상대경로로는 `./abc.py`로 나타낼 수 있다.



### 터미널(Terminal)

#### 터미널

- 마우스가 아닌 키보드로 명령을 직접 입력하여 컴퓨터를 제어하는 소프트웨어.
- `GUI(Graphic User Interface)`는 우리에게 친숙한 바탕화면을 생각하면 된다. 
- `CLI(Command Line Interface)`는 text를 사용하여 명령을 입력하는 체계이다. 터미널을 통해 컴퓨터를 조작하는 인터페이스 체계이다.
- `쉘(Shell)` 은 터미널이 활성화됨과 동시에 실행되는 **명령어 해석기**이다. 터미널을 켬과 동시에 쉘은 자동적으로 실행된다.
  - Windows에는 대표적으로 `cmd`가 있고, `powershell`도 있다. 또한 리눅스 명령어를 입력받을 수 있는 `cmder`도 있다.
  - Mac / Linux에는 `csh`, `bash`, `zsh` 등이 있다.

#### 간단한 명령어

- `mkdir` + `디렉토리명`: 새로운 디렉토리 생성
- `cd` + `디렉토리명` : 해당 디렉토리로 이동
- `ls(dir)` : 현재 위치한 디렉토리 내의 파일/디렉토리를 보여줌.
- `cp(copy)` + `복사할 파일경로(a)` + `복사시킬 경로(b)` : a에 있는 파일을  b로 복사함.
- \* 참고로 상대경로를 쓸 때, Windows cmd에서는 역슬래시를 사용한다. \*

