---
title: Naver Boostcamp Day+2 [4] &#58; String and advanced function concept
categories: [boostcamp]
tags: [부스트캠프, 프로그래밍]
comments: true
math: true
---

\- 이 강의정리본은 가천대학교 최성철 교수님의 강의를 정리한 것임을 밝힙니다.\-

<p align="center" style="font-size : 30px; font-weight: bold;"><u>String & advanced function concept</u></p>

### String

- 문자열 : `Sequence`자료형으로 문자형 data를 메모리에 저장.
  - sequence 자료형 : `list`, `tuple`, `str` 등이 있다.
  - 영문자 1글자는 `1byte`의 메모리 공간을 사용한다. (1byte = 8bit = $$2^8$$)
    - cf) `int` 는 `4byte`, `float`는 `8byte`의 메모리 공간을 사용한다.
    - 할당된 공간에 따라 표현할 수 있는 숫자 범위가 다르다.
    - 데이터타입은 메모리의 효율적 활용을 위해 **매우** 중요하다!

#### 1Byte?

- 컴퓨터는 `2진수`로 데이터를 저장한다. 
- 이진수 한 자릿수(`0`혹은 `1`)는 `1bit` 로 저장된다.
- 따라서 `1bit`는 0 또는 1 이므로 2개가 저장 가능하다.
- 그렇다면 `1byte` = `8bit` = `2**8` = `256` 개의 다른 값을 저장 가능한 메모리공간인 셈.

####  컴퓨터가 문자를 인식하는 방식

- 컴퓨터는 문자를 직접적으로 인식할 수 없다. → 모든 데이터를 2진수로 인식한다.
- 따라서, 특정 문자를 특정 2진수 값으로 mapping해줘야 한다.
- 이를 위한 표준 규칙들이 있다.(ex. `ASCII`, `UNICODE`, `UTF-8`, `UTF-16`, `EUC-KR` 등)
  - 예를 들어 `UTF-8` 기준으로 대문자 `U`는 이진수 `1000011`로 변환된다.

궁금한게 어느 정도 해소되었다면, 문자열의 특징을 알아보도록 합시다.

#### 인덱싱(Indexing)

- `List`의 각 값들이 개별 주소를 가지듯이, `string`의 각 문자들도 개별 주소(offset)를 갖는다.
- 인덱싱 : 주소를 사용하여 할당된 값을 가져오는 것. `List`와 동일하다.

```python
string = 'abcde' # a, b, c, d, e 모두 개별 주소를 갖는다.
string[0] # a (앞에서 0번째.)
string[-5] # a (뒤에서 다섯번째. 뒤에서는 -1부터 시작.)
```

#### 슬라이싱(Slicing)

- `List`와 마찬가지로, `string`도 주소값을 기준으로 특정 문자열을 잘라낼 수 있다.

```python
string = 'abcde'
string[1:4] # bcd → 1번째부터 4번째 전까지.
string[-50:50] # abcde → 범위를 넘어가는 경우 자동으로 최대범위로 지정해준다.
string[::-2] # eca → 역순으로 2칸씩 건너뛰며 슬라이싱.
```

#### 문자열 연산 및 포함여부 검사

- `+` : 문자열 간 concatenation을 할 수 있다.
- `*` : 문자열을 반복할 수 있다.
- `in` : 특정 문자가 문자열에 포함되었는지를 알 수 있다.

```python
string_1 = 'hello'
string_2 = 'world'
# 덧셈 연산
print(string_1 + ' ' + string_2) # hello world
# 반복 연산
print(string_1 * 2) # hellohello
# in 
print('ello' in string_1) # True
```

#### 문자열 함수 : 이 정도는 알아야지 !

<p align='center' width='800px'><img src="https://user-images.githubusercontent.com/37925813/105387471-edb6f500-5c58-11eb-8a2a-aa44a2a6a412.png"></p>

<p align='center' width='800px'><img src="https://user-images.githubusercontent.com/37925813/105387558-045d4c00-5c59-11eb-82ce-b1fd5b11b3c0.png"></p>

#### 다양한 문자열 표현

- 문자열 선언은 작은 따옴표(`'`)나 큰 따옴표(`"`)를 사용한다.

```python
# It's OK.
print("It's OK.") # 큰 따옴표로 둘러싸면 된다.
print('It\'s OK.') # 혹은 escape 문자 \ 를 사용하여 이 작은따옴표는 문장 안에서 쓰이는 작은 따옴표임을 나타낼 수 있다.

# I said "hello naver!"
print('I said "hello naver!"')
print("I said \"hello naver!\"")
```

- 두 줄 이상을 저장하고자 할 때는,
  - `\n`: 개행기호로서, 줄바꿈을 해주는 기호이다.
  - `'''`: `docstring`으로, 큰따옴표 혹은 작은 따옴표를 세번으로 연속으로 사용하면 줄을 바꿔가면서도 계속 문자열을 저장할 수 있다.

```python
# hello
# world 를 만들고 싶다면?
print('hello\nworld')
print('''hello
world''')
```

- `\`의 역할 : 표시하기 어려운 문자들을 표현할 때 쓴다.
  - `\n` : 개행기호(line break). 한 줄 내리고 싶을 때.
  - `\t`: tab 기호. tab을 한 번 친 것과 같은 효과.
  - 외에도 다양하다.
- raw string(`r`) : `\` 가 쳐진 글자까지도 무시하는 효과를 갖는다.

```python
print('hello\nworld') # hello (한줄내려가서) world
print(r'hello\nworld') # hello\nworld
```

---

### Function [2]

#### Call by object reference ?

- 함수에서 parameter를 전달하는 방식에는 크게 세 가지가 있다.
  - 값에 의한 호출(Call by Value) : 함수에 인자를 넘길 때, `값`만 넘김. 함수 내에 인자값 변경 시, 호출자에게 영향을 주지 않음.
  - 참조에 의한 호출(Call by Reference) : 함수에 인자를 넘길 때, `메모리 주소`를 넘김. 함수 내에 인자값 변경 시, 호출자의 값도 변경됨.
  - 객체 참조에 의한 호출(Call by Object Reference)
    - `Python`은 `객체의 주소`가 함수로 전달되는 방식이다.
    - ① 전달된 객체를 참조하여 변경 시 호출자에게 영향을 주지만(call by reference의 특징), ② 새로운 객체를 만들 경우 호출자에게 영향을 주지 않는다.(call by name의 특징)
    - 무슨 말이냐........

```python
def make_new_list(lst): # lst라는 인자를 전달받음.
    				  # 아래 메인 프로그램에서 my_list라는 객체의 주소를 받음!
	lst.append(5) # 전달된 객체(my_list)를 참조하여 5를 append함.
    			 # 호출자인 my_list에 변경이 일어난다.[1, 2, 3, 4, 5]
	lst = [3, 4, 5] # 함수 내에서 lst라는 새로운 객체에 [3, 4, 5]를 할당함.
    			   # 아까의 인자 lst와 이름은 같지만, 새로운 객체이다.
        		   # 따라서 이 때는 호출자인 my_list에는 변화가 없다.
    print(lst) # 새로운 객체인 lst를 출력하므로 [3, 4, 5]가 출력된다.
	
my_list = [1, 2, 3, 4]
make_new_list(my_list) # [3, 4, 5]
print(my_list) # [1, 2, 3, 4, 5]
```

<p align='center' width='400px'><img src="https://user-images.githubusercontent.com/37925813/105389557-4d160480-5c5b-11eb-8d4e-a79a4559e536.png"></p>

#### Scoping Rule

- 변수의 범위 
  - `지역변수`(local variable) : 함수 내에서만 사용되는 변수
    - 함수 바깥에서는 정의된 적이 없는 변수이기 때문에, 함수를 나가면 의미가 없어진다.
    - 같은 이름의 변수라도 하나는 `전역변수`, 하나는 `지역변수`라면 두 변수는 다르다! (동명이인을 생각하면 된다.)
    - 다만, `global` 선언을 해줄 경우 함수 내에서 `전역변수`를 사용/변경할 수 있다. 함수 내, 함수 바깥의 같은 이름의 변수가 같은 `전역변수`를 가리키게 된다.
  - `전역변수`(global variable) : 프로그램 전체에서 사용하는 변수

```python
def test(t):
    print(x)
    t = 20
    print("in function:", t)
    
x = 256
test(x) # 256
	    # in function: 20
print(t) # NameError: name 't' is not defined - 지역변수이기 때문에!
```

```python
# 동명이인 : 지역변수와 전역변수
def f():
    s = 'I love Seoul'
    print(s)
s = 'I love NewYork'
f() # I love Seoul
print(s) # I love NewYork

# 함수 내에서 전역변수 사용하기 by global
def f():
    global s
    s = 'I love Seoul'
    print(s)
s = 'I love NewYork'
f() # I love Seoul
print(s) # I love Seoul - 전역변수 s를 함수에서 사용하고(global), 그 s를 바꿔줬기 때문!
```

#### Recursive Function(재귀함수)

- 재귀함수 : 자기자신을 호출하는 함수
- 점화식과 같은 재귀적 수학모형을 표현할 때 사용한다.
- 재귀 종료 조건까지 함수 호출을 반복하게 된다.

```python
def factorial(n): # 팩토리얼 값을 구하는 함수
    if n == 1 : # n이 1이라면
        return 1
    else: # n이 1이 아니라면
        return n * factorial(n-1)
    
factorial(5)

########## How It Works ###########
# (1) 5가 들어가서, 5 * factorial(4) 반환
# (2) factorial(4) 에서 4 * factorial(3) 반환 --- 5 * 4 * factorial(3)
# (3) factorial(3) 에서 3 * factorial(2) 반환 --- 5 * 4 * 3 * factorial(2)
# (4) factorial(2) 에서 2 * factorial(1) 반환 --- 5 * 4 * 3 * 2 * factorial(1)
# (5) factorial(1) 에서 1 반환 --- 5 * 4 * 3 * 2 * 1 = 120
```

#### function type hints

- `Python`의 특징인 `dynamic typing`으로부터 비롯되었다.
- 처음 함수를 사용하는 사용자는 해당 함수의 인터페이스를 알기 어렵다. 예를 들어, 인자가 5개 되는 함수에서 각 인자가 어떤 `type`으로 들어가야 하는지 알 수 없다.
- 이러한 문제를 해결하기 위해, `Python 3.5` 버전 이후로는 `PEP484`에 기반하여 `type hints` 기능을 제공하고 있다.
- 함수의 각 인자에 콜론 기호(`:`) 및 해당 인자의 `type`을 적어주고, `->` 및 결과값의 `type`을 적어주어, 처음보는 사용자도 어떤 `type`의 인자가 들어가고 어떤 `type`의 결과값이 나올지 쉽게 알 수 있다.

```python
def name_and_age(name: str, age: int) -> str: # name은 str, age는 int, return 값은 str 이구나~
    return f"Hello, my name is {name} and i'm {age} years old."
```

#### function docstring

- 이 역시 처음 함수를 보는 사용자가 해당 함수의 역할을 명확히 알 수 있도록 하는 기능이다.
- `docstring` : 아까 string 챕터에서, 작은/큰 따옴표 세 개를 써서 두 줄 이상의 문자열을 표현하고자 하는 경우를 의미한다고 했다.
- 이 `docstring`을 이용하여 함수에 대한 상세설명을 작성할 수 있다.
- `VSCode`에서는 `extension`에서 `docstring generator`를 깔고, `(ctrl + shift + P)`를 눌러 `Palette`를 켠 다음에 `docstring`을 검색해서 실행하면 자동으로 상세설명 틀(frame)을 입력해준다.

```python
def name_and_age(name: str, age: int) -> str: 
    '''
    Returns string that shows name and age.
    	
    	Parameters :
    		name (str) : A user's name
    		age (int) : A user's age
    	
    	Returns:
    		(string) : string which gives user's name and age.
    '''
    
    return f"Hello, my name is {name} and i'm {age} years old."
```

#### PEP8 - Python 코딩 컨벤션의 기준

- 코딩 컨벤션 : 코딩을 함에 있어 일관적이고, 규칙적인 코드를 작성할 수 있도록 하는 미리 정해진 규약 !  즉, **사람의 이해를 돕기 위한 코딩 규칙**을 의미한다.
  - `코딩은 협업` : 일관된 규칙(컨벤션)이 있어야 팀원들과 협업을 원활하게 할 수 있다.
  - `코드는 보고서` : 내가 조직을 나가더라도, 내가 짜놓은 코드를 다른 사람이 봤을 때 이해가 가능해야 한다. 이를 위해서는 만국공통의 규칙을 따라서 코드를 작성해야 할 것이다.
  - `일관성` : 가장 중요하다!
  - `읽기 좋은 코드가 좋은 코드` : 말해 뭐하나
  - ex) 들여쓰기를 `Tab`으로 할 것이냐, `4 space`로 할 것이냐?
    - `Tab`과 `4 space` 중 하나를 골라서, 무조건 그것만 써야함!! 
    - 혼합하면 혼난다.
  - ex2) 소문자 l, 대문자 O, 대문자 I 금지
    - l과 I, O와 0은 헷갈리기 쉬운 문자이기 때문에 금지한다.
- 함수 개발 가이드라인
  - ① 함수는 가능하면 짧게 작성할 것. 
    - 가능하다면 한 번에 하나의 기능만을 하는 함수를 여러개 짜서 쓸 것을 의미한다.
  - ② 함수 이름에 함수 역할, 의도가 명확히 드러날 것.
    - `def f(x)` 와, `def calculate_circle_area(r)` 중 직관적인 함수는?
    - 당연히 후자이다.
  - ③ 하나의 함수에는 유사한 역할을 하는 코드만 포함하라.
    - ①과 비슷한데, 하나의 함수에서 다양한 역할을 하도록 만들지 말라는 뜻이다.
  - ④ 인자로 받은 값 자체를 바꾸지 말 것!!!
    - 즉, 인자로 받은 값을 복사해서 쓰는 편이 좋다.
    - 이는 `Python`이 `Call by Object Reference`를 따르기 때문이다.
    - 객체 주소가 참조되는 호출자가 영향을 받아서 바뀌어버리면 곤란한 일이 생길 가능성이 매우 높기 때문이다!
- 함수는 언제 만드는가?
  - 공통적으로 자주 사용되는 코드는 함수 하나로 만들어서 재사용한다.
  - 복잡한 수식의 경우, 식별 가능한 이름의 함수로 변환한다.
    - ex) 근의 공식 : 이름 없이 띡 코드에 작성해놓으면, 처음보는 사용자는 이 코드가 근의 공식을 구현한 것인지 알기 어렵다. 그러나 `quadratic_equation`이라는 함수명을 써서 함수를 구현한 후에, 해당 함수를 코드 내에서 쓰면 바로 이것이 근의 공식임을 알 수 있다.
  - 복잡한 조건의 경우, 식별 가능한 이름의 함수로 변환한다. 
- 나의 코딩이 `PEP8`의 코딩컨벤션을 잘 지키는지 확인하고 싶다면?
  - `flake8` : `conda install flake8` or `pip install flake8` 로 설치하고, `import flake8`하여 사용한다.
  - `black` : 마찬가지 방식으로 설치하고 사용한다. 이 라이브러리는 `PEP8`에 맞게 나의 코드를 reformat해준다!









