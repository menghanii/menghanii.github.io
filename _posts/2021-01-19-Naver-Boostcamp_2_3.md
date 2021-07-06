---
title: Naver Boostcamp Day+2 [3] &#58; Conditional and Loops
categories: [boostcamp]
tags: [부스트캠프, 프로그래밍]
comments: true
---

\- 이 강의정리본은 가천대학교 최성철 교수님의 강의를 정리한 것임을 밝힙니다.\-

<p align="center" style="font-size : 30px; font-weight: bold;"><U>Conditionals and Loops</U></p>

### Condition

<p align="center" style="font_size:20px; font-weight:bold;">"if ~ elif ~ else ~"</p>

- 조건문 : 조건에 따라 특정한 동작을 하게 하는 명령어.
  - **"~ 이면 ~ 하라."** (**"if ~ then ~"**)의 형식을 가지고 있다.
  - `Python`은 조건문으로 `if` , `else`, `elif` 등의 예약어를 사용함.

#### if-else문 문법

```python
if <condition_1>: # 첫 번째 조건 만족 시,
    <statement_1> # 수행문 1 수행 
    <statement_2> # 수행문 2 수행 ...
    
elif <condition_2>: # else if : 첫 번째 조건을 만족하지 못했지만, 두 번째 조건은 만족 시
    <statement_3> # 수행문 3 수행
    <statement_4> # 수행문 4 수행

else: # 첫 번째, 두 번째 조건 모두 만족 못할 시
    <statement_5> # 수행문 5 수행
```

- `if`, `elif`, `else` 문 내에서는 `indentation(들여쓰기)` 가 꼭 이루어져야 한다.

#### 조건 판단

<p align='center' width='600px'><img src="https://user-images.githubusercontent.com/37925813/105207782-47e98480-5b8b-11eb-9813-83d375f99320.png"></p>

- `a == b` 와 `a is b` 의 차이 : `a == b`는 a와 b의 **값**을 비교하는 반면, `a is b`는 a와 b의 **메모리주소**가 동일한지를 비교한다.

```python
a = 3
b = 3
print(a == b) # True
print(a is b) # True

a = 300
b = 300
print(a == b) # True
print(a is b) # False
```

- 위 코드에서 a와 b가 3일 때와, 300일 때의 조건 판단 결과가 왜 다를까?
  - `Python` 에서는 `-5 ~ 256` 까지는 미리 해당 값의 메모리 주소를 할당해놓았다.
    - 옛날 `Python`에서 자주 쓰는 숫자는 빠르게 실행하기 위해서 그랬다고 한다.
    - 따라서 a = 3, b = 3은 항상 같은 메모리 주소를 가리키게 된다. 그렇기 때문에 값도 동일, 메모리 주소도 동일하다.
    - 연장선상에서, a = 300, b = 300인 경우는 값은 동일하나 각각 할당된 메모리 주소는 다르기 때문에 `True` 와 `False`가 나오게 된다.

#### 조건 참/거짓의 구분

- 컴퓨터는 존재하면 `True` , 존재하지 않으면 `False`라고 판단함.
  - `0` 이 아닌 어떤 숫자라도 존재한다고 볼 수 있으므로 `True`
  - `0`은 존재한다고 보지 않으므로 `False`가 된다.

```python
if 1: # 존재함
    print('True')
else:
    print('False')
# True

if '': # 존재하지 않음
    print('True')
else:
    print('False')
# False
```

#### 논리 키워드 : and / or / not

- `and`, `or` : 모든 조건이 참일 때만 참, 하나의 조건이라도 참이면 참.
- `not` : **'~ 이 아닌'**으로 생각하면 될듯하다.

```python
string = 'Hello World!'
if 'Hello' not in string: # string 변수에 'Hello'가 없으면
	print('no Hello here')
else:
    print('Hello is here!')
# Hello is here!
```

- `all()`, `any()` built-in 함수 : `iteration`이 가능한 객체(리스트, 튜플 등)를 인자로 받아, 참과 거짓을 판단하는 함수

```python
all([False, False, True]) # 모든 조건이 참이어야 True를 반환
# False
any([False, False, True]) # 하나의 조건이라도 참이면 True를 반환
# True
```

#### 삼항 연산자(Ternary Operators)

- 참일 경우와 거짓일 경우의 결과를 무려 **한 줄**에 표현하는 방법!

```python
name = 'maeng'
is_my_name = True if 'm' in name else False # 삼항 연산자 구문
print(is_my_name)
# True
```

---

### Loop

<p align="center" style="font_size:20px; font-weight:bold;">"for ~ in range():" OR "While True:"</p>

#### for loop

- 반복문 : 정해진 동작을 반복적으로 수행하게 하는 명령문.
- `Python`은 반복문으로 `for`, `while` 등의 키워드를 사용함.

#### range

- `range(start, end, step)` : `range`는 `start`부터 `end` 앞까지 리스트를 만들어주는 함수이다.
- `step`은 한 번에 몇 스텝(간격)을 갈 것인지에 대한 인자로, `2`로 하는 경우 2스텝을 밟고, `-1`을 하는 경우 역순으로 1스텝을 밟게 된다.

```python
for i in range(0, 10, 2):
	print(i) # 0, 2, 4, 6, 8 --- end 전까지이므로 10은 포함되지 X
    
for j in range(10, 5, -1):
    print(j) # 10, 9, 8, 7, 6 --- end 전까지!
```

#### 반복문 관련 상식

- 반복문 변수명 : 임시변수는 `i`, `j`, `k` 로 주로 정하나, 꼭 그런건 아니다. 의미가 있는 반복 변수의 경우 의미를 살리는 편이 좋다.

```python
for i in range(5):
    print(i) # 0, 1, 2, 3, 4

words = ['Hello', 'World']
for word in words:
    print(word) # 'Hello' \n 'World'
```

- 0부터 시작하는 반복문 : 반복문은 대부분 0부터 시작한다.
- 무한 loop : 반복 명령이 끝나지 않는 프로그램 오류

#### While

- 조건이 만족하는 동안 **계속** 반복 명령문을 수행하는 키워드

```python
while True: # 조건이 무조건 만족되므로
    print(1) # 1이 평생 출력된다.
    
while 'abc':
    print(1) # 마찬가지다.
    
a = 0
while a < 10:
    print(a)
    a += 1
# 0 1 2 3 4 5 6 7 8 9 
# 10이 되는 순간 while 문의 조건을 만족하지 않기 때문에 조건문이 끝나게 됨.
```

#### 반복문을 제어하는 방법 : `break`, `continue`

- `break` : 특정 조건이 만족되면 반복문을 탈출함.
- `continue` :  특정 조건이 만족되면 이후의 명령을 수행하지 않고 다시 iteration으로 돌아감. 일종의 skip문.

```python
for i in range(10):
	print(i)
    if i == 5:
        break
# 0 1 2 3 4 5
# 5까지 출력 후, if문의 조건을 만족하면서 break문을 만나 반복문을 탈출한다.

for i in range(10):
    if i <= 5:
        continue
    print(i)
# 6 7 8 9
# 0~5까지는 if문의 조건을 만족하며 continue를 만나 이후의 명령을 수행하지 않고 되돌아간다. (skip)
# 6~9까지는 if문의 조건을 만족하지 않으므로 출력된다.
```

#### 가변적인 중첩 반복문 (variable nested loops)

- 실제 프로그램에서 반복문은 사용자의 입력에 따라 가변적으로 반복된다. 예를 들어 숫자맞추기 게임이라면, 사용자가 숫자를 맞출때까지 반복된다.

```python
# 숫자 맞추기 게임
import random
guess_number = random.randint(1, 100) # 1에서 100 사이 중 랜덤 정수 발생
users_input = int(input('숫자를 맞춰보세요(1~100): '))
while users_input != guess_number:
    if users_input < guess_number:
        print('숫자가 작습니다.')
    elif users_input > guess_number:
        print('숫자가 큽니다.')
    users_input = int(input('숫자를 맞춰보세요(1~100): '))
else:
    print('정답입니다.')
```

- 또한 반복문은 중복되어 반복이 나타나는 경우도 많다.(예를 들어 2중 for문)

```python
# 2중 for문
# 1부터 9까지의 구구단을 모두 계산하고 싶은 경우
for i in range(1, 10):
    for j in range(1, 10): # for문(loop)이 중첩(nested)되었습니다.
        print(f'{i} x {j} = {i * j}')
```

---

### Debugging

<p align="center" width="300px"><img src="https://user-images.githubusercontent.com/37925813/105213039-871ad400-5b91-11eb-8242-08d0825d8c6a.gif"></p>

(...) 나임.

#### Debugging

- 코드의 오류를 발견하여 수정하는 과정.
- 오류의 **원인**을 알고 **해결책**을 찾아야 함.
- `문법적 에러` 와 `논리적 에러`를 잘 찾아야 함..

#### 문법적 에러

- 들여쓰기 오류
- 오탈자
- 대소문자 구분 안함

```python
# 들여쓰기 오류
x = 2
 y = 5 # indentation error
print(x+y)

# 오탈자
prlnt(x+y) 

# 대소문자 구분 안함
data = 'Python'
print(Data) # NameError 발생
```

#### 논리적 에러

- 뜻대로 실행이 안되는 코드인 경우.
- 가장 확실한 방법은 **중간 중간 프린트 문을 찍어서 확인해보는 것.**

```python
def calculate_rectangle_area(x, y):
	return x * y

print(calculate_rectangle_area(2, 4)) # 8이 잘 나오는지 확인!
```