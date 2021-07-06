---
title: Naver Boostcamp Day+4 [2] &#58; Module and Project
categories: [boostcamp]
tags: [부스트캠프, 프로그래밍]
comments: true
math: true
---

\- 이 강의정리본은 가천대학교 최성철 교수님의 강의를 정리한 것임을 밝힙니다.\-

<p align="center" style="font-size : 30px; font-weight: bold;"><u>Module and Project</u></p>

`Python`은 대부분의 라이브러리가 이미 다른 사용자에 의해서 구현되어있다. 근데 이렇게 남이 만든 프로그램을 쓰기 위해서는, `모듈`과 `패키지` 개념에 대해서 잘 알고 있는 것이 중요하다. 그러니 한 번 알아보자.

먼저, `프로젝트(혹은 패키지)`가 하나의 완성된 레고 작품이라면, `모듈`들은 레고 작품을 이루고 있는 개개의 레고 조각들이라고 생각하면 된다. `Python`에서 모듈들은 하나의 `py`파일로 이루어져 있으며, 패키지는 이러한 모듈들을 모아놓은 `폴더(디렉토리)` 이다.

```python
import random
```

위의 코드를 보자. `random`은 `Python`의 Built-in 모듈의 이름이다. 다양한 방식으로 난수(random number)를 생성할 수 있도록 도와주는 모듈이며, 그 안에 `randint`, `randrange` 등 다양한 함수들이 존재한다. 이제 한 번 모듈을 직접 구현해보자.

### Module 만들기 및 사용하기

- 하나의 `Module`은 하나의 `py`파일을 의미한다.
- `import`문을 사용하여 호출한다. (그러나 `import`는 모듈뿐 아니라 패키지도 불러올 수 있고, 모듈 내부의 특정 함수를 호출할 수도 있다.)

```python
# fah_converter.py (섭씨온도를 화씨온도로 변환시켜주는 모듈)
def convert_c_to_f(celcius):
	return celcius * 9.0 / 5 + 32

# module_exp.py (fah_converter.py와 같은 위치에 존재하는 파일)
import fah_converter
celcius = 36.5
fah = fah_converter.convert_c_to_f(celcius) # 모듈.함수 의 방식으로 사용한다.
# 만약 모듈이 아닌 함수를 import한 경우에는 바로 함수 이름을 써서 사용할수도 있다.
print(fah)
```

- 모듈 사용하기

  - `alias` 설정 : `import ~ as ~`의 형식으로, 모듈의 이름에 별칭을 붙일 수 있다. 

  ```python
  import random as r
  ```

  - 모듈에서 특정함수 혹은 클래스만 호출하기 : `from ~ import ~` 

  ```python
  from random import randint
  ```

  - 모든 함수/클래스를 호출하기 : `from ~ import *`

  ```python
  from random import *
  ```

- Built-in Modules : `Python`에서 기본 제공되는 라이브러리로, 별다른 설치 없이 `import`문으로 바로 활용이 가능하다.

```python
import random
import time
import urllib.request
```

### 패키지

- 하나의 대형 프로젝트를 만드는 코드의 묶음이다.
- 다양한 모듈들의 합이며, 모듈들을 디렉토리 구조로 연결한다.
- `__init__`, `__main__` 등의 키워드 파일명이 사용된다.

#### 패키지 만들어보기

- `game`이라는 패키지 안에, `image`, `sound`, `stage`라는 폴더를 만들고, 세부 폴더 안에 필요한 모듈들을 구현한다고 해보자.

- 각 폴더가 패키지임을 알리기 위해서는 ,`__init__.py` 를 구성해야 한다. `Python 3.3`부터는 구성하지 않아도 알아서 폴더가 패키지임을 인식하지만 아직도 많은 코드들에서는 `__init__.py` 파일을 구성하고 있다.

  - 우리의 패키지에서는 `game`, `image`, `sound`, `stage` 폴더에 각각 `__init__.py` 파일을 구성해야 한다.

  <p><img src="https://user-images.githubusercontent.com/37925813/106131119-9c9d8880-61a5-11eb-8860-da907de89496.png"></p>

  - `__init__.py` 파일 내에는 우리가 사용할 패키지/모듈들을 불러와야 하는데, 이 때 `__all__`과 `import` 구문을 사용한다.

- `__main__.py` : 이 파일은 패키지를 `Python`으로 실행시켰을 때, 바로 실행되는 파일이다. 

```python
# image - show.py (모듈)
def show_image(): # 함수
    print('This shows some images')
    
# sound - echo.py
def sound_play():
    print('This module plays sound.')
    
# stage - main.py
def show_stage():
    print('This module shows the stage.')
```

- 위의 코드와 같이 각 폴더 별로 모듈을 하나씩 구성해놓고, 그 안에 함수들을 구현하였다.
- `game` 폴더에 `__main__.py` 파일을 생성한 후, 각 폴더의 모듈에서 함수를 불러와 사용하는 프로그램을 작성해보자.

```python
from image.show import show_image
from sound.echo import sound_play
from stage.main import show_stage

if __name__ == '__main__':
    print('First : Image module')
    show_image()
    print()
    print('Second : Sound module')
    sound_play()
    print()
    print('Third: Stage module')
    show_stage()
```

- `python game`으로 `game`폴더를 실행시켜보자.

<p><img src="https://user-images.githubusercontent.com/37925813/106132259-14b87e00-61a7-11eb-9979-44dfb228e897.png"></p>

- `__main__.py` 폴더 내에 `if __name__ == "__main__"` 구문 이하의 코드들이 실행되는 것을 볼 수 있다.

### 가상환경 설정하기(Virtual Environment)

- 가상환경은 왜 설정하는가? 가상환경은 보통 프로젝트별로 설정하는 것이 일반적이다. 만약 `프로젝트1`에서는 `Tensorflow 1.x`버전을 사용하고, `프로젝트2`에서는 `Tensorflow 2.x` 버전을 사용한다고 했을 때, 사용자의 컴퓨터에는 `Tensorflow 2.x`만 깔려있다면 어떻게 할 것인가? 매번 프로젝트별로 다운그레이드/업그레이드를 반복할 것인가?

- 그렇지 않다.. 그렇기 때문에 가상환경이 필요한 것이다.

- 가상환경은 하나의 깨끗한 `스케치북`으로, 각 프로젝트 별로 새로운 스케치북을 주고, 프로젝트에서 필요한 패키지만 인스톨하여 사용할 수 있게 만들어 놓은 것이다.

- 가상환경의 종류 : 대표적으로 `virtualenv`와 `conda`가 있다.

  - `Windows`를 쓰고, 설치가 편한 것이 좋다면 `conda`를,
  - 정말 다양한 패키지를 사용해서 쓰고자 한다면 `virtualenv`를 사용한다고 한다.
  - 여기서는 `conda`를 활용하여 가상환경을 다뤄보도록 하겠다.

  ```
  conda create -n my_project python=3.8
  <venv 만들기> 	  <프로젝트명>  <파이썬 버전>
  ```

  - 위의 코드를 실행하게 되면, `my_project`라는 이름을 가진 새로운 가상환경이 생성된다.

  <p><img src="https://user-images.githubusercontent.com/37925813/106133170-3e25d980-61a8-11eb-9120-9b9767730cbb.png"></p>

- 가상환경을 호출하는 법 

```
conda activate my_project
```

- 가상환경을 해제하는 법

```
conda deactivate
```

- 가상환경에서 패키지 설치하는 법 (activate 되었다는 전제 하에)

```
conda install <package name>
```

