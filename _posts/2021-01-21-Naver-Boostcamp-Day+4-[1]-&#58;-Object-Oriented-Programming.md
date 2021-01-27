---
title: Naver Boostcamp Day+3 [2] &#58; Object-Oriented Programming
categories: [boostcamp]
tags: [부스트캠프, 프로그래밍]
comments: true
math: true
---

\- 이 강의정리본은 가천대학교 최성철 교수님의 강의를 정리한 것임을 밝힙니다.\-

<p align="center" style="font-size : 30px; font-weight: bold;"><u>Object-Oriented Programming</u></p>

### 클래스와 객체

- 수강신청 프로그램을 작성한다고 했을 때, 두 가지의 작성방법이 있다.
  - 1) 수강신청 프로그램을 순서대로 처음부터 끝까지 작성하는 방법
  - 2) 수강신청의 `주체`, `행동`, `데이터`들을 각각 만들어서 프로그램을 작성후 이들을 연결시키는 방법
- 최근에는 두 번째 방식이 주를 이루고 있고, 이러한 기법을 객체 지향 프로그램(object-oriented program)이라 한다.

#### 객체 지향 프로그래밍

- `객체`란? 실생활에서 일종의 물건과 같다. `객체`는 `속성`(attribute)와 `행동`(method)를 갖는다.
- 이러한 객체의 개념을 프로그램으로 표현한 것이 객체지향 프로그래밍이다. `객체`의 속성은 `변수`로, 행동은 `함수`로 표현된다.
- `Python` 역시 객체지향프로그래밍 언어이다.

- 예를 들어, 축구 프로그램을 작성한다고 했을 때, `객체`에는 팀, 선수, 심판, 공 등이 있을 것이다. `행동`으로는 선수는 공을 차고, 심판은 휘슬을 부는 등의 행동들이 있을 것이고, `속성`1으로는 선수의 경우 이름, 소속팀, 포지션 등이 있을 것이다.
- `OOP`는 프로그래밍의 설계도인 `클래스(class)`와 이를 구현한 구현체인 `인스턴스(instance)`로 나눠진다.
  - 붕어빵 틀이 클래스라면, 실제로 만들어진 다양한 맛의 붕어빵은 인스턴스들인 것이다.
- 구현을 하면서 알아보자.

```python
# 축구 선수 정보를 Class로 구현하기
class SoccerPlayer(object): # object는 요즘 python 에서는 빠져도 되는 인자가 되었다.
    def __init__(self, name, position, back_number): # 생성자 함수. 초기화.
        self.name = name # 이름
        self.position = position # 포지션
        self.back_number = back_number # 등 번호
        # 축구선수 객체가 가질 속성 정보들을 __init__함수로 초기화해주었다.
        
    def change_back_number(self, new_number): # method(행동/함수) 
        print(f'등 번호를 {self.back_number}에서 {new_number}로 변경합니다.')
        self.back_number = new_number
        # 등번호 속성을 바꾸어주는 함수를 정의하였다.
```

#### naming rule

- `OOP`에서 `변수`, `클래스`, `함수` 의 이름은 짓는 방식이 존재한다. 꼭 따라야하는 건 아니지만, 일종의 규약처럼 여겨진다.
- ① `snake_case` : 띄워쓰는 부분에 "_" 를 추가하는 작명법이다. 보통 `함수`, `변수`명에 사용된다.
- ② `CamelCase` : 띄워쓰기 부분을 대문자로 대체하는 작명법이다. 보통 `클래스`명에 사용한다. 위의 코드에서도 `class SoccerPlayer`로 작성한 것을 볼 수 있다.

#### Attribute (속성 정보) 추가하기

- Attribute 추가는 `__init__`, `self`를 통해 할 수 있다.
- `__init__`은 객체 초기화 예약 함수이다. `init`이 '시작'을 의미하니까, 시작할 때 객체의 기본 정보를 정의해준다고 생각하면 함수의 역할을 좀 더 직관적으로 이해할 수 있을 것이다.
  - 초기화 함수이기 때문에, `인스턴스`를 처음 생성할 때 `__init__` 함수의 인자들을 모두 적어주어야 해당 정보를 가진 객체가 생성된다.
- (참고) `__` 즉, underscore 2개가 양옆으로 들어간 함수들은 특수한 예약함수나 변수, 그리고 함수명 변경(맨글링)을 할 때 사용된다.
  - `__main__`, `__add__`, `__str__` 등 다양한 예약함수가 존재한다.

```python
class SoccerPlayer(object):
    def __init__(self, name, position, back_number):
        self.name = name # 이름
        self.position = position # 포지션
        self.back_number = back_number # 등 번호
        
    def __str__(self):
        return f"Hello, my name is {self.name} and my position is {self.position}"
    
maeng = SoccerPlayer("Maeng", "FW", 32)
print(maeng) # Hello, my name is Maeng and my position is FW
```

- maeng = SoccerPlayer("Maeng", "FW", 32) 에 대해 알아보자.
  - `maeng`은 `Class`를 통해 실제로 구현된 객체이다. 즉, 붕어빵 틀에서 나온 실제 붕어빵인 것이다.
  - `SoccerPlayer()`은 `Class명으로, 붕어빵 틀이다. 
  - `("Maeng", "FW", 32)` 는 초기값으로 들어가는 인자들로, 객체의 속성정보이다. 

#### 실제 클래스 작성해보기 - Note와 NoteBook 클래스 만들기

- `Note` : 사용자가 무엇인가를 적을 수 있는 노트이다. 
  - `Note`에는 `content`가 있고, 내용을 제거할 수 있다. 
  - 두 개의 `Note`의 `content`를 합쳐 하나로 만들 수 있다.
  - 또한 해당 `Note` 객체를 출력하는 경우 `content`가 나와야 한다.
- `NoteBook` : `Note`들이 저장되는 하나의 책이다.
  - `NoteBook`은 `title`, `page_number`, `notes`를 가진다.
  - `Note`가 삽입될 때 `page_number`가 1씩 늘어난다.
  - `NoteBook`에서 특정 `page_number`의 노트를 지울 수도 있다.
  - 또한, 현재 `NoteBook`의 총 페이지 수를 받아올 수도 있다.

```python
class Note():
    def __init__(self, content=None):
        self.content = content
        
    def write_content(self, content):
        self.content = content
        
    def remove_all(self):
        self.content = ""
        
    def __add__(self, other):
        return self.content + other.content
    
    def __str__(self):
        return self.content
```

```python
class NoteBook(object):
    def __init__(self, title):
        self.title = title
        self.page_number = 1
        self.notes = dict()
        
    def add_note(self, note, page=0):
        if page == 0:
            self.notes[self.page_number] = note
            self.page_number += 1
        else:
            self.notes[page] = note
            self.page_number += 1
            
    def remove_note(self, page_number):
        if page_nubmer in self.notes.keys():
            return self.notes.pop(page_number)
        else:
            print("해당 페이지는 존재하지 않습니다.")
    
    def get_number_of_pages(self):
        return len(self.notes.keys())
```

```python
mynote = Note() # mynote 객체를 생성함.
mynote.write_content('나는 명한이다.') # content를 입력함.
print(mynote) # 출력했을 때 content가 나와야 함. '나는 명한이다.'

mynotebook = NoteBook('명한이의 책') # mynotebook 객체를 생성하면서, title을 정함.
mynotebook.add_note(mynote) # 아까 만들어뒀던 mynote 객체를 notes에 삽입함.
mynotebook.get_number_of_pages() # 페이지수를 반환. 2

print(mynotebook.page_number) # 2

print(mynotebook.title) # '명한이의 책'

print(mynotebook.notes[1]) 
# notes의 1에 해당하는 values -> mynote 객체
# print(mynote)이므로 '나는 명한이다.' 를 출력함.
```

### OOP Characteristics

#### 객체 지향 프로그래밍에 꼭 필요한 것들

① Inheritance(상속)

- 부모클래스로부터 속성과 method를 물려받는 자식클래스를 생성하는 것이다.
- 자식클래스를 생성할때, 인자값에 부모클래스를 입력하여 상속받는다.
- 또한, 부모클래스의 함수를 불러올 때는 `super().함수명(파라미터)` 로 불러와서 사용한다.
- 이외에 자식클래스에서 추가할 속성정보나 메소드가 있다면 추가할 수 있다.

```python
class Person(object): # 부모 클래스 Person 선언
    def __init__(self, name, age, gender): # 속성정보는 이름/나이/성별 뿐.
        self.name = name
        self.age = age
        self.gender = gender
    
    def about_me(self): # Method 선언
        print("저의 이름은 ", self.name, "이구요, 제 나이는 ", str(self.age), "살입니다.")

class Employee(Person): # 부모 클래스 Person으로 부터 상속
    def __init__(self, name, age, gender, salary, hire_date): # salary, hire_date를 속성정보로 추가.
        super().__init__(name, age, gender) # 부모객체 사용
        self.salary = salary
        self.hire_date = hire_date # 속성값 추가
        
    def do_work(self): # 새로운 메서드 추가
        print("열심히 일을 합니다.")
        
    def about_me(self): # 부모 클래스 함수 재정의
        super().about_me() # 부모 클래스 함수 사용
        print("제 급여는 ", self.salary, "원 이구요, 제 입사일은 ", self.hire_date," 입니다.")
```

② Polymorphism(다형성)

- 같은 이름의 메소드지만, 내부 로직을 다르게 작성하는 것을 의미한다. 메소드 오버라이딩이라고도 하는 것 같다.

```python
class Person:
    def greeting(self):
        print('안녕하세요.')
 
class Student(Person):
    def __init__(self, name):
        self.name = name
        
    def greeting(self):
        print(f'안녕하십니까? 저는 {self.name}입니다.')
 
maeng = Student('maeng')
maeng.greeting() # 안녕하십니까? 저는 maeng입니다.
```

- 위의 코드에서 볼 수 있다시피, `Person` 클래스를 정의하고, 아래에 `Person` 클래스를 상속받는 `Student` 클래스를 선언했다.
  - `Person` 클래스도 `greeting`이라는 함수가 있고, `Student` 클래스도 `greeting`이라는 함수가 있다.
  - 이 때 `maeng`이라는 객체를 `Student` 클래스의 객체로 선언하고, `greeting`함수를 실행하면 `Person` 클래스의 함수가 아닌, `Student` 클래스의 함수가 출력되는 것을 볼 수 있다.
  - 이를 다형성, 메소드 오버라이딩이라고 한다.
- 왜 필요한가?
  - 어떤 기능이 비슷한 역할을 수행하지만 약간씩 다른 경우에, 같은 메소드 이름으로 사용되는 것이 훨씬 편리할 경우들이 많다.
  - 예를 들어, `Student` 클래스, `Engineer` 클래스 등 많은 클래스들에 각각 `greeting` 함수가 필요하다면 이를 `greeting_1`, `greeting_2`, ... 등으로 만드는 것이 매우 불편하다. `greeting`하나로 퉁치면서, 각각의 클래스별로 약간의 변화를 줄 수 있는 것이다.
  - 또한, 부모클래스의 `greeting`을 가져와서 일부 사용할 수도 있다. 만약 `안녕하세요.` 부분이 모든 자식클래스의 `greeting` 함수에서 동일하게 쓰여야 하는 경우, `super().greeting()`을 활용하여 부모클래스의 `greeting`함수를 재사용할 수 있다.

③ Visibility(가시성)

- 객체의 정보를 볼 수 있는 레벨을 조절하는 것이다.
- 특히, 어떤 서비스를 제공할 때 사용자가 보거나 함부로 건드려서는 안되는 정보들에 접근하지 못하게끔 하기 위해 사용한다.
- 이를 `캡슐화` 혹은 `정보 은닉`이라고도 한다. (Information Hiding)
- 근데 아직 그렇게까지 깊이 알아야될 단계는 아닌 것 같다. 추후에 필요한 일이 생기면 다시 공부하기로 마음먹었다.

### Decorate

- 일단, 나는 `클로저`의 개념과 `데코레이터`의 개념을 정확하게 숙지하지 못했다. 실제 사용되는 예를 많이 보지 못했기 때문이다. 
- 그런데 https://wikidocs.net/83687를 보니, 어떤 개념인지 대략적으로 이해는 된다. 이것을 참고하는게 보다 도움이 될 것이다.

- 데코레이터를 이해하기 위해 알아야되는 개념으로는 크게 ① 1급 객체(함수), ② inner function이 있다.

#### 1급 객체(First-class objects)

- `Python`의 함수는 모두 일급 함수이다. 이것이 무슨 의미인가?
- 1급 객체는 해당 객체가 **변수로 할당될 수 있는 객체**임을 의미한다.
- 즉, 어떠한 함수를 변수로 받을 수 있다.
- 그리고 이렇게 함수를 할당받은 변수를 다른 함수의 파라미터, 혹은 리턴값으로 사용할 수 있다.
- 예제를 통해 알아보자.

```python
def square(x):
	return x ** 2

f = square # 함수를 f라는 변수로 받았다.
f(5) # 25 가 반환된다.
```

```python
# 위에서 이어서

def cube(x):
	return x ** 3

def formula(method, argument_list):
    return [method(value) for value in argument_list]

f = cube
formula(f, [1, 2, 3, 4, 5]) # [1, 8, 27, 64, 125]
formula(cube, [1, 2, 3, 4, 5]) # 위와 동일한 결과 나옴.

# 이전에 배웠던 map 함수와 비슷한 사용법을 보이고 있다.
# map 역시 특정함수를 sequence의 모든 원소에 적용하는 함수로,
# Python의 함수가 일급객체라는 특징을 활용한 것이다.
```

#### Inner Function

- `함수` 내에 또다른 `함수`가 존재하는 것을 의미한다.

```python
def print_msg(msg):
	def printer(): # printer함수를 정의하고
        print(msg) 
    printer() # print_msg 함수에서 printer()를 수행한다.
    
print_msg("Hello, Python!") # Hello, Python!
```

- `closures` : inner function을 return 값으로 변환한다.
  - return 하는 `함수`가 생기게 되므로 이를 변수로 할당받아 사용할 수 있다!
  - 1급객체 개념을 여기서 다시 이용하고 있다.

```python
def print_msg(msg):
	def printer(another_msg): # printer함수를 정의하고
        return msg + ' ' + another_msg
    return printer

greeting = print_msg("Hello, Python!") # 바깥 함수인 print_msg를 greeting으로 받음. -> 이 때 인자로 받은 msg는 another에 고정된다.
greeting('This is maeng') # 안쪽 함수인 printer()를 실행
greeting('Honestly, i do not like python hehe')

# Hello, Python! This is maeng
# Hello, Python! Honestly, i do not like python hehe
```

- `decorator` : 데코레이터 함수는 `함수`를 입력인자로 받아, 기존 함수의 변경 없이 추가적인 기능을 덧붙일 수 있도록 해주는 함수이다. 복잡한 클로저 함수를 간단하게 해주는 기능이라고 한다. 솔직히 간단한지 잘 모르겠다. 개념자체가 복잡하다.

```python
def print_msg(func):
    def printer(another_msg):
        return func() + ' ' + another_msg
    return printer

@print_msg # decorator 함수
def greeting(): 
    return "It's nice to meet you."
    
greeting('Maeng') # greeting 안에 들어간 인자는 another_msg 인자이다...
```

- 와 진짜 어렵다.