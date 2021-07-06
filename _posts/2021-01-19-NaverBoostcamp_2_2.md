---
title: Naver Boostcamp Day+2 [2] &#58; Function and Console I/O
categories: [boostcamp]
tags: [부스트캠프, 프로그래밍]
comments: true
---

\- 이 강의정리본은 가천대학교 최성철 교수님의 강의를 정리한 것임을 밝힙니다.\-


<p align="center" style="font-size : 30px; font-weight: bold;"><U>Function and Console I/O</U></p>

### function

- 함수 : 어떠한 일을 수행하는 코드의 덩어리

  - 코드를 한 번만 작성해도, 반복적으로 사용할 수 있다.
  - 캡슐화 : 인터페이스만 알면 타인의 코드를 쉽게 사용할 수 있다.
  - 강제적인건 아니지만, 함수와 함수 사이에는 기본적으로 `2줄`이 띄어져 있어야 한다.

  ```python
  # 인터페이스 - 함수 calculate_rectangle_area에서 인자 x, 인자 y가 무엇을 의미하는지, 그리고 어떤 결과값을 반환하는지 → 즉 함수의 설명
  def calculate_rectangle_area(x, y):
      return x * y
  ```

#### 함수 선언 문법 및 사용 예시

```python
def function_name(parameter_1, param_2, ..., ):
    statements_1
    statements_2
    ...
    return return_value
```

```python
def calculate_rectangle_area(x, y): # x, y → parameters	
    return x * y

print('x=2, y=4인 사각형의 넓이:', calculate_rectangle_area(2, 4)) # 2, 4 → arguments
```

* `Parameter` 는 함수를 선언할 때 인터페이스에 들어가는 **변수**이고, `Arguments`는 실제 함수를 사용할 때 `Parameter` 부분에 들어가는 **값**들이다.

#### 함수 수행 순서

- 함수 부분을 제외한 메인 프로그램부터 시작한다.
- 함수를 호출할 경우, 함수부분을 수행한 다음에 메인프로그램으로 되돌아온다.

```python
def calculate_rectangle_area(x, y): # --- (2) 함수 수행
    return x * y

print('x=2, y=4인 사각형의 넓이:', calculate_rectangle_area(2, 4)) # --- (1) 메인프로그램 수행 → 함수 호출--(2) → 출력 
```

#### 함수에서 Parameter, Return의 유무

- `Parameter` : 없을 경우 파라미터를 이용하지 않고 함수를 수행하며, 있을 경우 파라미터를 이용하여 함수를 수행한다. 이해하기 쉽게 보자면,

```python
def say_hello(): # parameter 없음
    print("hello")
    
say_hello() # hello → parameter가 없으므로 고정된 형태의 수행문만 수행된다.

def say_hello_to_your_name(name): # parameter : name
    print("hello,", name)
    
say_hello_to_your_name('myunghan') # hello, myunghan
say_hello_to_your_name('Naver') # hello, Naver → 다양한 parameter에 대응하는 함수가 수행된다.
```

- `return` : 없을 경우 함수 내의 수행문만을 수행하며, 있을 경우 결과값을 반환한다. 차이는, 반환값이 있다면 이를 변수에 할당할 수 있다는 것이다.

```python
def say_hello(): # return 없음
    print("hello")
    
a = say_hello() # hello
print(a) # None : say_hello 함수의 반환값이 없으므로, a에 할당되는 값도 없기 때문이다.

def say_hello():
    return 'hello'

a = say_hello() # 아무 출력도 없다. 출력문 없이 반환값을 a에 할당했기 때문!
print(a) # hello
```

---

### Console In & Out

<p align="center" style="font_size:20px; font-weight:bold;">"어떻게 프로그램과 데이터를 주고받을 것인가?"</p>


앞서 day1에서 서술한 바와 같이, `터미널`에서는 키보드로 text를 입력하여 프로그램을 실행한다. 이 때, 프로그램에 사용자가 input 데이터를 집어넣어야 하는 경우가 생기는데, 이를 어떻게 할 수 있을까?

#### 콘솔창 입/출력 : input - print

- `input()` 함수 : 콘솔창에서 **문자열**을 입력받는 함수

```python
# hello.py
name = input('이름을 입력하세요:')
print('안녕하세요', name, '님.') # comma를 사용하는 경우 print 문은 각 인자를 한 칸 띄어쓰면서 연결하여 출력해준다.

# 콘솔창에서 hello.py를 실행 (python hello.py)
# 이름을 입력하세요 :
# 이름을 입력하세요 : 양명한
# 안녕하세요 양명한 님.
```

- `input`함수는 사용자의 어떤 입력이라도 `문자열(str)`로 받기 때문에, 숫자 등을 활용한 계산을 하고 싶다면 데이터 형 변환을 해주어야 한다.

```python
# calculate.py
num_1 = int(input('첫 번째 숫자(정수)를 입력하세요: ')) # --- int로 형 변환
num_2 = int(input('두 번째 숫자(정수)를 입력하세요: ')) # --- int로 형 변환
print('두 수를 더한 값은', num_1 + num_2, '입니다.')

# 콘솔창에서 calculate.py를 실행 (python calculate.py)
# 첫 번째 숫자(정수)를 입력하세요: 3
# 두 번째 숫자(정수)를 입력하세요: 7
# 두 수를 더한 값은 10 입니다.
```

- `print` 함수의 경우, 다양한 인자를 갖고 있는데, `sep`, `end` 등이 있다.

  - `sep` : seperator의 줄임말로, print문의 인자들 사이에 구분값을 지정해주는 역할을 한다. 아까, comma를 사용하는 경우 띄어쓰기 한 칸 되면서 출력해준다고 주석에 달아놨는데, 이는 `sep` 인자의 기본값이 띄어쓰기이기 때문이다. 

  ```python
  print('I','Seoul', sep='♡')
  # I♡Seoul
  print('You','I','We','Us', sep=' & ')
  # You & I & We & Us
  ```

  - `end` : print문 마지막(end)을 어떻게 마무리지을 것인가에 대한 인자이다. 기본적으로는 `\n(개행)` 값을 갖기 때문에, 아무런 인자를 입력받지 않는 경우 한 줄 띄운다.

  ```python
  print('Hello')
  print('World')
  # Hello
  # World
  
  print('Hello', end=' ')
  print('World')
  # Hello World
  ```

---

### Print Formatting

<p align="center" style="font_size:20px; font-weight:bold;">"형식에 맞춰서 출력을 해야하는 경우"</p>

기본적으로 print formatting에는 `% string`, `format 함수`, `fstring` 이 있다. 앞의 두 개는 old-school 한 방식이고, 가장 최근에 도입된 것이 `fstring`이다.

#### Old-School Formatting

- `% string` : `%` 기호를 이용하여 포매팅을 함. 변수가 들어갈 자리에 `%` 기호와 변수의 `type`을 써준 뒤, 출력문이 끝나면 `%`를 한 번 더 붙이고 `tuple`형식으로 변수를 집어넣는다.
- `%s`, `%d`, `%f` : 각각 `string`, `digit(int)`, `float` 이다.

```python
print('%s love %s' % ('I', 'Python')) # I love Python
print('%s love number %d' % ('I', 7)) # I love number 7
print('%s is %f' % ('Pie', 3.14)) # Pie is 3.14
```

- `format 함수` : `{ }(bracket)`을 사용하여 출력문 내에 변수가 들어갈 자리를 만들고, 출력문이 끝날때 `.format(arg1, arg2, ...)` 형식으로 작성한다.
- `{ }` 안에는 변수 목록의 인덱스 번호가 들어갈 수도 있고, 변수명이 직접 들어갈 수도 있다.

```python
print('{} love {}'.format('I', 'Python'))
print('{} love {}'.format('I', 7)) # I love 7
print('{me} love {seven}'.format(me = 'I', seven = 7)) # I love 7
print('{1} love {0}'.format('I', 7)) # 7 love I → 변수의 인덱스를 넣어주면 인덱스에 맞춰서 출력함.
```

#### fstring

- `fstring` : `Python 3.6` 이후 , PEP498에 근거한 formatting 기법
- 출력할 문장 앞에 `f`를 붙이고, `{ }` 안에 변수명을 직접 기입하면 해당 변수명을 가져와서 출력함.

```python
py = 'python'
me = 'I'
num = 7
pi = 'Pie'
pi_num = 3.14

print(f'{me} love {py}') # I love Python
print(f'{me} love number {num}') # I love number 7
print(f'{pi} is {pi_num}') # Pie is 3.14
```

#### 소숫점 자리 / padding / 정렬

```python
print('{0:*^9s} is {1:#>15.2f}'.format('Pie', 3.141592))
# ***Pie*** is ###########3.14

name = 'YMH'
say = 'hello'

print(f'{say:~<10s} my name is {name:^20s} !')
# hello~~~~~ my name is         YMH          !
```

- `{ }` 안에서,
  - `0:`, `1:` : 변수의 인덱스 번호
  - `*`, `#`, ... : `padding` 시에, 공백부분을 채울 문자
  - `^`, `<`, `>` : 가운데 정렬 / 왼쪽 정렬 / 오른쪽 정렬을 의미함.
  - `9`, `15`, ... : `padding`을 할 문자 개수로, 9면 총 9칸을, 15면 총 15칸을 쓰라는 의미.
  - `s`, `d`, `f` : string, digit, float을 의미함. float의 경우 소수점 자리를 지정할 수 있는데  `.2f`, `.5f` 와 같이 나타내며, 숫자는 나타내고 싶은 소수점 자리수를 의미한다.