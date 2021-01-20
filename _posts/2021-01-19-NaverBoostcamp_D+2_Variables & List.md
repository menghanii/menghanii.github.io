---
title: Naver Boostcamp Day+2 [1] &#58; Variables & List
categories: [boostcamp]
tags: [부스트캠프, 프로그래밍]
comments: true
---

\- 이 강의정리본은 가천대학교 최성철 교수님의 강의를 정리한 것임을 밝힙니다.\-

<p align="center" style="font-size : 30px; font-weight: bold;"><U>Variables & List</U></p>

### variable & memory

- 변수(variable) : 가장 기초적인 프로그래밍 문법 개념으로, 데이터(값)를 저장하기 위한 메모리 공간의 프로그래밍 상 이름이다.

  ```python
  student = 'MyungHan Yang' # 변수 : student, 데이터(값): 'MyungHan Yang'
  ```

- 위 코드를 땅! 치면 정확하게 어떤 일이 발생한걸까?
  
  - student라는 **변수**에, 'MyungHan Yang' 이라는 **값**을 넣으라는 의미이다.
- 그렇다면 이 student라는 변수는 어디에 저장되는 것일까?
  - `App(python)`에서 변수에 값을 할당하면, `memory`의 주소에 할당된다.
  - **폰 노이만 아키텍처** : 컴퓨터에 값을 입력 → 정보를 먼저 메모리에 저장 → CPU가 순차적으로 그 정보를 해석하고 계산하여 결과값을 전달함.
  
  

<p align="center"><img src="https://user-images.githubusercontent.com/37925813/105166297-a9452f80-5b5a-11eb-99f4-ddd3676727e5.png"></p>

```python
# 아래와 같이 변수 세 개를 할당했을 때 어떤 일이 발생할까?
student = 'MyungHan Yang'
a = 3
b = 7
```

<p align="center" width="500px"><img src="https://user-images.githubusercontent.com/37925813/105166823-3a1c0b00-5b5b-11eb-8bbc-0f674ff52398.png"> </p>



- 변수 이름 작명법 : ① 의미있는 단어로, ② 대소문자를 구분하여, ③ 알파벳, 숫자, 언더스코어(_)를 이용하여 선언한다.

  - 그러나 변수명의 첫 시작이 숫자여서는 안된다!

  - 또한 `for`, `if`, `else` 등과 같은 `예약어`는 쓰지 않는다.

  ```python
  # 잘못되었거나, 권장하지 않는 변수 표기
  1abc = 3 # 변수가 될 수 없음
  for = 'forfor' # 예약어 - 변수가 될 수 없음.
  a = 3 # 단순한 연산을 할 때는 상관없지만, 의미가 분명해야하는 경우 권장되지 X
  
  # 권장하는 변수 표기
  student_name = 'MyungHan Yang' # 변수명에 의미가 명확하게 드러나 있다.
  x_length, y_length = 3, 7 # 마찬가지.
  ```

---

### Basic Operations

간단한 사칙연산, 문자열 처리 등과 같은 기초적인 연산부터 알아보겠습니다.

- 기본자료형

  <p align="center" width="600px"><img src="https://user-images.githubusercontent.com/37925813/105167511-27ee9c80-5b5c-11eb-86f2-2a708878bed0.png"></p>

- 위 표의 각 자료 유형마다 차지하는 메모리 공간이 다르다. → **이후에 매우 중요해짐!!!!**

  - 예를 들어, `integer`는 32bit(=4byte)를 차지하고, `float`는 64bit(=8byte)를 차지함.

- 파이썬은 `Dynamic Typing` 언어이다. 즉, 코드의 실행 시점에 데이터의 `type`을 결정한다.

  - 예를 들어, `Java`의 경우 실행 전에 이미 `integer`라고 표현해주지만, `Python`은 실행하고나서야 이 변수가 `integer`인지 아닌지를 알게 된다. 
  - 이러한 성질 때문에, `Python`은 사용자가 편리하게 변수를 작성할 수 있게끔 돕고, type의 변환이 용이하다는 **장점**이 있는 반면, 코드 실행시간이 좀 더 오래걸린다는 **단점**이 있다.

- 연산자(Operator)와 피연산자(Operand)

  - 연산자 : `+`, `-`, `*`, `/`, `//`, `%` ,`**` 등과 같은 기호들

    - ★ 나누기 연산을 실행하면 결과값이 `integer`라도 `float`으로 바뀐다!

  - 피연산자: 연산자에 의해 계산되는 숫자들.

  - `3 ** 2` 에서 `**` 는 연산자, `3` 과 `2`는 피연산자이다.

  - 문자 간에는 `+` 와 `*` 연산이 가능하다.

    ```python
    # + : Concatenate 연산
    string_1 = "Hello"
    string_2 = "World"
    full_string = string_1 + string_2 # 연산자 +, 피연산자(string_1, string_2)
    print(full_string) # HelloWorld
    ```

    ```python
    # * : 반복 연산
    string = "Hello"
    print(string * 3) # HelloHelloHello
    ```

  - `+=`, `-=` : 증가 및 감소연산을 의미함. (`*=`, `/=`  등도 존재한다.)

    ```python
    a = 3
    a += 10 # a = a + 10
    print(a) # 13
    
    b = 5
    b *= 30 # b = b * 30
    print(b) # 150
    ```

- 데이터 형 변환

  - 정수형 ↔ 실수형
    - `float()` 와 `int()`를 사용하여 변환 가능.
    - 만약 10.3 과 10.7을 정수형으로 변환 후 덧셈하면 어떤 값이 나올까?

  ```python
  a = 10.3
  b = 10.7
  
  a = int(a) # int를 하게되면 반올림하지 않고 내림한다!! 기억할 것.
  b = int(b) # int를 하게되면 반올림하지 않고 내림한다!! 기억할 것.
  
  print(a+b) # 20
  ```

  - 숫자 ↔ 문자열
    - 문자열로 선언된 값도, 변환이 가능한 숫자로 이루어져 있다면 변환 가능함.

  ```python
  a = '76.3'
  b = float(a)
  
  print(type(a)) # <class 'str'>
  print(type(b)) # <class 'float'>
  ```

  - 위에서 한번 봐서 알겠지만, `type()` 함수를 통해 데이터형을 확인할 수 있다.

  - 부동소수점 문제 : 이진수로 변환하는 과정에서 발생하는 무한소수 문제. 그러나 Python 3.x 부터는 정상적으로 나온다고 합니다.

  ```python
  c = 38.8
  print(c) # 38.8
  c # 38.79999999999997
  ```

---

### List

<p align="center" style="font-size:20px; font-weight:bold"> "데이터 100개가 있다면 어떻게 관리할 것인가?"</p>

- 이에 대한 대답은, 100개의 변수를 일일이 만들어서 관리한다 가 아니라, **"1개의 변수를 생성하고 여기에 데이터 100개를 모두 넣어서 관리한다"**.
- 이에 적합한 자료구조가 바로 `List`이다.

```python
my_list = [1, 2, 3, 4, 5, ..... , 100]
```

- 먼저, 리스트의 특징을 살펴봅시다.

  - Indexing : 리스트에 들어간 값들은 모두 `주소(offset)`를 갖는다. 이 주소를 이용하여 리스트의 특정 값을 찝어낼(?) 수 있다. 이를 `인덱싱`이라 한다.

  ```python
  # 인덱싱의 예
  my_list = [1, 2, 3, 4, 5]
  print(my_list[0]) # my_list 변수의 0번째 주소에 해당하는 값을 출력하라. → 1
  # 주소의 시작은 0 부터!! ★★★
  ```

  

  - Slicing : 리스트의 주소들을 이용하여 여기서부터 저기까지 자르는 것을 의미한다. 

  ```python
  # 슬라이싱의 예
  # list[start : stop : step] → start 주소값부터 시작하여 step만큼 이동하며 stop 전 주소값에서 슬라이싱이 끝난다.
  my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  print(my_list[1:5]) # my_list의 첫 번째 주소부터 5번째 주소 '전'까지 출력하라. → [2, 3, 4, 5]
  
  print(my_list[::2]) # step을 2씩 밟아라. → [1, 3, 5, 7, 9]
  ```

  

  - 리스트 연산 : `+`, `in` 등

  ```python
  # list concatenation
  color = ['red', 'blue', 'green']
  color2 = ['orange', 'black', 'white']
  print(color + color2) # ['red', 'blue', 'green', 'orange', 'black', 'white']
  
  # in
  'pink' in color # 'pink'는 color에 없으므로 False
  
  # 리스트 값 변경
  color[0] = 'pink'
  print(color) # ['pink', 'blue', 'green']
  print('pink' in color) # True
  ```

  

  - 추가 / 삭제 : `append`, `extend`, `insert`, `remove`, `del` 등

  ```python
  color = ['red', 'blue', 'green']
  color2 = ['orange', 'black', 'white']
  color.append('peru')
  print(color) # ['red', 'blue', 'green', 'peru']
  
  color.extend(color2)
  print(color) # ['red', 'blue', 'green', 'peru', 'orange', 'black', 'white']
  
  color.insert(0, "purple") # 0번째 위치에, purple을 삽입해라.
  print(color) # ['purple', 'red', 'blue', 'green', 'peru', 'orange', 'black', 'white']
  
  color.remove("white") # 리스트에서 white를 삭제함. 만약, 리스트에 white가 여러 개 있다면 제일 앞에 있는 놈 하나만 삭제함.
  
  del color[0] # 0번째 주소 리스트 객체 삭제.
  print(color) # ['red', 'blue', 'green', 'peru', 'orange', 'black']
  ```

  

  - 메모리 저장 방식 : `Python`은 특이하게도 다양한 data type이 하나의 리스트에 들어갈 수 있다. 또한, 리스트 안에 리스트를 다시 입력하는 것도 가능하다.

  ```python
  my_list = [1, 2, 3, 'abc', [1.3, 2.1]]
  ```

  

  - 패킹과 언패킹 : 한 변수에 여러 개의 데이터를 넣는 것을 **패킹(packing)**이라 하고, 한 변수의 데이터를 각각의 변수로 반환하는 것을 **언패킹(unpacking)**이라 한다.

  ```python
  t = [1, 2, 3] # t라는 변수에 1, 2, 3을 list의 형식으로 packing
  a, b, c = t # t에 있는 값 1, 2, 3을 변수 a, b, c에 각각 unpacking
  print(t, a, b, c) # [1, 2, 3] 1 2 3
  ```

  

  - 이차원 리스트 : 리스트 내에 리스트를 만들면 이차원의 리스트가 생성되는데, 이는 `행렬(Matrix)`같이 사용될 수 있다.

  ```python
  # 명한, 기봉, 나현, 경환, 연정의 국어, 수학, 영어 점수
  kor_score = [80, 92, 95, 91, 85]
  math_score = [75, 92, 87, 69, 90]
  eng_score = [80, 76, 100, 96, 92]
  class_score = [kor_score, math_score, eng_score]
  
  # 만약 명한의 수학 점수를 알고싶다면?
  print(class_score[1][0]) # [1] : 수학 점수, [0]: 명한이 점수
  ```