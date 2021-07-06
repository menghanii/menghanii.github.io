---
title: Naver Boostcamp Day+3 [2] &#58; Pythonic Code
categories: [boostcamp]
tags: [부스트캠프, 프로그래밍]
comments: true
math: true

---

\- 이 강의정리본은 가천대학교 최성철 교수님의 강의를 정리한 것임을 밝힙니다.\-

<p align="center" style="font-size : 30px; font-weight: bold;"><u>Pythonic Code</u></p>

### 파이썬 스타일 코드

- `Python` 특유의 문법을 활용하여 효율적으로 코드를 표현함.
- 그러나 더 이상 파이썬 특유의 코드라고 할 수는 없다. 왜냐면 많은 언어들이 서로의 장점을 채용하고 있는 추세이기 때문이다.
- `Pythonic Code`는 고급 코드를 작성할수록 더 많이 필요해진다. 그리고 많은 코드들이 `Pythonic Code`로 작성되었기 때문에 꼭 이해하고 넘어가는 것이 필요하다!

### Split & Join

- `split` : string type의 값을 "기준값"으로 나누어(split), `List` 형태로 반환한다.

```python
string = 'I hate homework'
word_list = string.split() # split의 인자에 아무것도 없으면 빈칸을 기준으로 나눈다.
print(word_list) # ['I', 'hate', 'homework']

string_2 = 'green,yellow,red,black'
color_list = string_2.split(',') # 쉼표를 기준으로 string을 나누었다.
print(color_list) # ['green', 'yellow', 'red', 'black']
```

- `join` : string으로 구성된 `List`를 합쳐 하나의 string으로 반환한다. `split`의 반대라고 생각하면 된다.

```python
word_list = ['I', 'hate', 'homework']
string = ' '.join(word_list) # 한칸을 띄워서 word_list 내의 문자열들을 합쳐준다.
print(string) # I hate homework
```

### List Comprehension

- 기존의 List를 활용하여 간단히 다른 List를 만드는 기법이다.
- `Python`에서 가장 많이 사용되는 기법 중 하나이다.
- 일반적으로 `for + append `문보다 속도가 빠르다는 장점이 있다.

```python
# General style
result = []
for i in range(10):
    result.append(i)
    
# List comprehension
result = [i for i in range(10)]
```

-  `if else 문`, `2중 for문` 이 포함된 list comprehension은 어떻게 작성할까?
  - `if else 문` : `if문`만 들어갈 때는 `for문` 뒤에 `if문`을 작성하지만, `if ~ else~ 문`인 경우에는 `if ~ else~ 문`을 `for문` 앞에 작성해주어야 한다. 꼭 주의하자!
  - `2중 for문` : 기본적인 `for문`의 모양을 생각하면서 짜면 쉽다. 첫 번째 라인의 for문이 먼저, 두 번째 라인의 for문이 그 다음으로 들어가면 된다. 그리고 append해야하는 값은 가장 앞에 써준다.

```python
# 10까지의 숫자 중 2의 배수만 뽑아서 리스트를 만들어보자.
result = [i for i in range(1,11) if i % 2 == 0]
print(result) # [2, 4, 6, 8, 10]

# 2의 배수가 아닌 경우, 5를 append하는 리스트를 만들어보자.
result = [i if i % 2 == 0 else 5 for i in range(1, 11)] 
# 위와는 달리 if ~ else ~문일때는 if else가 for문 앞에 나와야함에 주의하자!
print(result) # [5, 2, 5, 4, 5, 6, 5, 8, 5, 10]

# 2중 포문을 해보자.
str_1 = ['a', 'b', 'c']
str_2 = ['x', 'y', 'z']
string_combination = [i+j for i in str_1 for j in str_2] 
# for i in str_1:
#    for j in str_2:
#        string_combination.append(i+j) 와 같다.
print(string_combination) # 첫 번재 for문이 처음, 두 번재 for문이 두번째로 나온다.
# ['ax', 'ay', 'az', 'bx', 'by', 'bz', 'cx', 'cy', 'cz']
```

### Enumerate & Zip

- `enumerate` : `List`의 원소들을 추출할 때 각 원소에 번호를 붙여주는 역할을 하는 함수이다. 보통 `for문`에서 굉장히 많이 이용한다.
- `zip` : 두 개 이상의 `List`의 값들을 **병렬적**으로 추출한다. 
- 예제를 통해 감을 잡아보자!

```python
# enumerate - word_list의 각 문자에 번호를 붙여서 dict를 만들어주자.
word_list = ['Ninja', 'Turtle', 'is', 'fast']
word_dict = dict()
for idx, word in enumerate(word_list): # [(0, 'Ninja'), (1, 'Turtle'), (2, 'is'), (3, 'fast')] 이런식으로 되어 있음
    word_dict[word] = idx
print(word_dict.items()) # dict_items([('Ninja', 0), ('Turtle', 1), ('is', 2), ('fast', 3)])
```

```python
# zip - 3차원 공간에 있는 두 점의 유클리드 거리를 구해보자.
a = [7, 3, 2]
b = [2, 3, 7]
dist = 0
for i, j in zip(a, b): # zip(a, b) = [(7, 2), (3, 3), (2, 7)]
    dist += (i-j) ** 2
    
dist = dist ** 1/2
print(dist) # 25.0
```

### Lambda, Map, Reduct

- `lambda` : 익명함수이다. 함수를 굳이 `def`로 정의하지 않고, 한 번만 간단하게 사용하고 버릴 때 많이 사용한다. (앞서서 `defaultdict`에서 본 적이 있을 것이다.)
  - lambda <인자> : <return 값> 의 형식으로 이루어져 있다.
  - `Python 3`의 `PEP8`부터는 권장하지 않으나 여전히 많이 쓰이긴 한다.

```python
f = lambda x, y: x + y
print(f(1, 4)) # 5

f = lambda x : x ** 2
print(f(5)) # 25
```

- `map` :  특정 함수를 `List`의 각 원소들에 모두 적용하고자 할 때 사용하는 함수이다.
  - 두 개 이상의 리스트에 대해서도 적용이 가능하다.
  - `if`문도 사용이 가능하다.

```python
ex = [1, 2, 3, 4, 5]
list(map(f, ex)) # [1, 4, 9, 16, 25]

# 두 개 이상의 리스트에 대해서도 적용이 가능하다.
list(map(lambda x,  y : x + y, ex, ex)) # [2, 4, 6, 8, 10]

# lambda의 if else문을 사용하여 filtering도 가능하다.
list(map(lambda x: x ** 2 if x % 2 == 0 else x, ex)) # [1, 4, 3, 16, 5]
```

- `reduce` : 말 그대로 '줄인다'. 즉, 하나의 리스트에 같은 함수를 적용해서 값들을 하나로 통합시킨다(줄여버린다).
  - `functools` 라이브러리에서 불러와서 사용한다.

```python
from functools import reduce

print(reduce(lambda x, y : x+y, [1, 2, 3, 4, 5])) # 15
# List의 첫번째 원소 두번째 원소를 더함.
# 이를 첫번째 원소로 하여 다시 두번째 원소인 3과 더함.
# ...
# 결국 1+2+3+4+5를 수행하는 셈이다.
```

- Lambda, map, reduce는 간단한 코드로 다양한 기능을 제공하지만, 코드의 직관성이 떨어져서 사용을 권장하지는 않는다. 그럼에도 우리가 이것들을 배우는 이유는 Legacy Code(옛날 코드)나 다양한 머신러닝 코드에서 여전히 이 함수들을 이용하고 있기 때문이다!

### iterable object

- Sequence 자료형에서 데이터를 순서대로 추출하는 object
- 내부적으로 이를 구현할 때, `__iter__` 와 `__next__`가 사용된다.
- `iterable` 객체(예를 들어, `List`나 `Tuple` 를 `iter()` 및 `next()` 함수를 통해 iterator object로 사용할 수 있다.

```python
cities = ['Seoul', 'Busan', 'Jeju']
iter_obj = iter(cities)

print(next(iter_obj)) # Seoul
print(next(iter_obj)) # Jeju
print(next(iter_obj)) # Busan
next(iter_obj) # StopIteration : 
```

### generator

- 위에서 설명한 iterable object를 특수한 형태로 사용하게 해주는 함수이다.
- (중요) element가 사용되는 시점에 값을 메모리에 반환한다. 즉, 모든 값을 메모리에 한꺼번에 올려놓는 것이 아니라, 사용하는 시점에 메모리에 올려놓는다.
  - 그리고 `yield`를 사용하여 한번에 하나의 element만 반환한다.

<p align="center" width="1000px"><img src='https://user-images.githubusercontent.com/37925813/105884532-69df7d00-604b-11eb-9302-51e1912ad662.png'><br>generator의 실행 예</p>

- `generator comprehension` : `list comprehension`과 유사한 형태로 생성할 수 있다. `list comprehension`이 `[ ]`를 사용하는 것과 달리, `( )`를 사용한다는 점이 차이점이다.

- generator는 대체 왜 쓰는가?

  - 일반적인 `List`같은 경우는 `generator`에 비해 훨씬 큰 메모리 용량을 사용한다.
  - 따라서, 대용량 데이터를 처리함에 있어 그냥 `List`를 사용하여 연산을 처리하다가는 메모리 뻑이 날수도 있다.
  - generator를 쓰게 되면, 호출할 때마다 데이터를 불러들이기 때문에 용량이 커도 처리의 어려움이 없다.

  <p align="center" width="1000px"><img src='https://user-images.githubusercontent.com/37925813/105885246-3cdf9a00-604c-11eb-8bf9-d2865fcb8e1d.png'><br>generator와 일반 list의 메모리 사용량</p>

- 개념이 쉽지않고, 나도 아직 다 이해하지 못했다. 실제 데이터를 처리하면서 사용해보면 어떤 느낌인지 알 수 있을 것 같다.

```python
# generator에서 yield문을 쓰는 예
def generator_list(value):
    for i in range(value):
        yield i
       
# generator comprehension
gen_ex = (n*n for n in range(500))
print(type(gen_ex))  #  <class 'generator'>
```

### Asterisk (중요!!!!!)

- 함수의 parameter가 정해져 있지 않은 경우가 많다. 즉, 같은 함수에 파라미터가 2개 들어올 때도 있고, 200개 들어와야 할 때도 있다.
- 이러한 `가변인자`를 `asterisk(*)`를 활용하여 처리할 수 있으니, 잘 숙지해두자.

#### 가변인자 : *args

- 함수에 들어가는 인자의 개수가 정해져있지 않은 경우에  사용하는 방법이다.
- 일반적으로 `*args`를 활용하여 이를 표시한다.
- 가변인자로 들어오는 값들은 `tuple type`으로 사용할 수 있다. 즉, 함수 내부를 구성할 때 가변인자는 `tuple`로 생각해서 처리해야함을 의미한다. 
- 가변인자는 **오직 한개만, 맨 마지막 parameter 위치**에 사용이 가능하다.

```python
def ast_sum(a, b, *args):
    return a + b + sum(args) # args는 tuple 로 들어올 것이므로, 미리 sum해줘야함.
print(ast_sum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)) # 55
# a = 1, b = 2, args = (3, 4, 5, 6, 7, 8, 9, 10) 으로 들어간 것!!
```

#### 키워드 가변인자 : **kwargs

- 함수에 parameter 이름을 따로 지정하지 않고, **parameter 이름 = 값** 으로 인자가 들어갈 수 있게끔 해놓는 방식이다.
- 일반적으로 `**kwargs`를 활용하여 표시한다. `asterisk`가 `2개`인 것에 주의하자!
- 입력된 값은 `dict type`으로 사용할 수 있다. 즉, `key : value` 쌍으로 저장되는 것이다.

```python
def kwargs_test_1(**kwargs):
    print(kwargs)
    
kwargs_test_1(myname='maeng', hisname='kkong') 
# {'myname': 'maeng', 'hisname': 'kkong'}
```

```python
# args와 kwargs를 모두 사용하여 함수를 만들어보자.
def kwargs_test_3(one, two, *args, **kwargs):
    print(one * two)
    print('args의 합은', sum(args))
    print('kwargs의 values는', kwargs.values())
    
kwargs_test_3(3, 6, 7, 8, 9, 19, 29, 38, myname='maeng', hisname='kkong')
# 18
# args의 합은 110
# kwargs의 values는 dict_values(['maeng', 'kkong'])
```

#### Unpacking a container

- `tuple`, `dict` 등의 자료형에 들어가 있는 값을 unpacking하는 역할로도 asterisk가 사용될 수 있다.
- 근데, unpacking하는 방법으로는 잘 쓰이지 않을 것 같은 느낌적인 느낌이라, 일단 여기서는 패스하고자 한다.





