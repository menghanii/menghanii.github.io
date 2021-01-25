---
title: Naver Boostcamp Day+3 [1] &#58; Python Data Structure
categories: [boostcamp]
tags: [부스트캠프, 프로그래밍]
comments: true
math: true
---

\- 이 강의정리본은 가천대학교 최성철 교수님의 강의를 정리한 것임을 밝힙니다.\-

<p align="center" style="font-size : 30px; font-weight: bold;"><u>Python Data Structure</u></p>

### Data Structure

- 전화번호부 정보는 어떻게 저장하는게 좋을까? `list` 형식?
- 택배 화물차에 들어가는 상품들은 어떻게 들어가고 어떻게 나올까? `stack`?

#### 파이썬 기본 데이터 구조

- `Stack`과 `Queue`
- `Tuple`과 `Set`
- `Dictionary`
- `Collections` 모듈

#### Stack(스택)

<p align='center' width="800px"><img src="https://user-images.githubusercontent.com/37925813/105699552-e474a380-5f4a-11eb-8d32-5f8782e29da9.png"><br>출처 : https://labuladong.gitbook.io/</p>

- 후입선출(나중에 들어간 데이터를 먼저 반환하는 자료구조)로,  `LIFO`(Last-In Firstt-Out)라고도 한다.
- `Stack` 구조에서 자료의 입력은 `Push`, 자료의 출력은 `Pop`이라고 한다.
- `List`를 이용해서 `Stack`을 구현할 수 있는데, `append`로 `Push`를 `pop`으로 `Pop`을 할 수 있다.

```python
a = [1, 2, 3, 4, 5]
a.append(10) # push
print(a) # [1, 2, 3, 4, 5, 10] -> 후입
a.pop()  # pop
print(a) # [1, 2, 3, 4, 5] -> 선출!
```

#### Queue(큐)

<p align='center' width="800px"><img src="https://user-images.githubusercontent.com/37925813/105699595-f2c2bf80-5f4a-11eb-82fb-339a212ab3d9.png"><br>출처 : https://labuladong.gitbook.io/</p>

- 선입선출(먼저 들어간 데이터를 먼저 반환하는 자료구조)로, `FIFO`(First-In First-Out) 라고도 한다.
- `Queue`에서는 자료의 입력을 `Put`, 자료의 출력을 `Get`이라고 한다.
- `Stack`과는 반대되는 개념이다. 
- `Queue` 역시 `List`를 이용하여 구현할 수 있는데, `append`로 `Put`을 `pop(0)`으로 `Get`을 수행할 수 있다.

```python
a = [1, 2, 3, 4, 5]
a.append(10) # put
print(a) # [1, 2, 3, 4, 5, 10] # 1 이 선입되어 있음. 10은 후입.
a.pop(0)  # get
print(a) # [2, 3, 4, 5, 10] -> 1이 먼저 나옴. 즉 선입선출!
```

#### Tuple(튜플)

- 값의 변경이 불가능한 리스트이다. 즉, `List`와 다른 점은 `Tuple`은 값의 변경이 불가능하다는 점이다. 아래 코드를 통해 무슨 차이인지 알아보자.

```python
lst = [1, 2, 3, 4, 5]
lst[1] = 100
print(lst) # [1, 100, 3, 4, 5] -> 1번째 인덱스에 값의 변경이 일어남.


tup = (1, 2, 3, 4, 5)
tup[1] = 100 # TypeError 오류 발생 ! 'tuple' object does not support item assignment. 즉, 튜플은 값의 변경을 지원하지 않는다!
print(tup) 
```

- 그럼 굳이 왜 리스트가 있는데 튜플을 만들어서 쓰는걸까?
  - 변경되지 않는 데이터를 저장하는 데 유용하기 때문이다.
  - 예를 들어, 주민등록번호 / 학번 등 변하지 않는 정보는 건드리면 안된다.
  - `List`를 사용하여 이들을 관리하게 되면, 어쩌다 사용자의 실수로 특정 값을 변경해버리는 불상사가 일어날 수 있다. 주민등록번호 뒤의 첫자리가 `1`이었는데, `2`로 바뀌는 불상사같은...
  - `List`는 `[ ]`를 이용했지만, `Tuple`은 `( , )`를 이용한다.
    - 사실 `( )`가 중요한게 아니고 `,` (쉼표)가 더 중요하다.
    - 값이 하나짜리인 `Tuple`객체라도 꼭 `,`를 붙여줘야 한다.

```python
t = (1) 
print(type(t)) # <class 'int'>

t = (1,)
print(type(t)) # <class 'tuple'>
```

#### Set(집합)

- `Set`의 가장 큰 특징 : `중복`이 존재하지 않는 자료구조이다 !!
- 2번째 특징 : 값을 순서없이 저장한다. `List`나 `Tuple`이 값의 순서를 보장하는 자료구조임에 비해, `Set` 안에 있는 원소들은 순서가 없다. 따라서 `Indexing`이나 `Slicing`이 불가능하고, 당연히 특정 위치에 있는 값의 변경도 불가능하다.

- `Set`은 언제 사용할까? 나같은 경우는 주로 중복을 제거해야하는 계산이 필요할 때, 즉 `unique`한 `item`의 개수를 세거나 하는 일에 `Set`을 사용하곤 한다. 
- `Set`에서 주로 사용하는 함수들은?
  - `add(x)`:  원소 `x`를 집합에 추가함. 만약 `x`가 이미 집합에 있으면, 일어나는 변화는 없다.
  - `update([x, y, z])`: 원소 여러개를 추가함.
  - `remove(x)` : 원소 `x`를 삭제함. 만약 `x`가 집합에 없으면 `keyError`를 발생시킨다.
  - `discard(x)`: `remove`와 마찬가지로 원소 `x`를 삭제함. 만약 `x`가 집합에 없으면 그냥 넘어간다. 따라서, 내가 사용하는 집합에 내가 지우려는 원소가 있는지 없는지 아리송할 때 사용하면 좋다. 있으면 지워주고 없으면 넘어가니까!
  - `union`(`|`), `intersection`(`&`), `difference`(`-`)
    - 두 집합 간에 사용하는 연산자이다. 
    - 순서대로 합집합, 교집합, 차집합이다.
    - 괄호 안에 있는 기호를 사용해서도 연산이 가능하다.

```python
s = set([1, 2, 3, 1, 2, 3]) # list -> set , tuple -> set도 가능하다.
print(s) # {1, 2, 3} - 중복이 제거되었다. 보기에는 순서를 기억하는 것 같이 생겼지만 속지말자.

s.add(1)
print(s) # {1, 2, 3}

s.remove(4) # 4는 현재 s에 없으므로, keyError를 발생시킴.
s.discard(4) # 4는 현재 s에 없으므로, 그냥 넘어감.
```

```python
s1 = set([1, 2, 3, 4, 5])
s1 = set([3, 4, 5, 6, 7])

s1.union(s2) # {1, 2, 3, 4, 5, 6, 7}
s1 | s2 # {1, 2, 3, 4, 5, 6, 7}

s1.intersection(s2) # {3, 4, 5}
s1 & s2 # {3, 4, 5}

s1.difference(s2) # {1, 2}
s1 - s2 # {1, 2}
```

#### Dictionary(사전)

- `Dict`의 가장 큰 특징 : `Key`와 `Value` 의 쌍으로 데이터를 저장한다는 특징이 있다!
  - 다른 언어에서는 `Hash Table`이라는 용어로 나타내기도 한다.
  - {Key1 : Value1, Key2 : Value2, ...} 의 형태를 가지고 있다.
    - `Value` 값에 또다시 `dict`가 들어가는 경우도 많다. (`json`파일이 `dict`와 같은 형태를 가지고 있다.)
  - 언제 사용할까? 
  - 학생들의 점수를 각각 `Key`와 `Value`의 쌍으로 나타낼 수 있다.

```python
student_math_score_dict = dict()
student_math_score_dict['양맹'] = 97
student_math_score_dict['누구'] = 100
student_math_score_dict['클로바'] = 120 # (?)
print(student_math_score_dict) # {'양맹': 97, '누구': 100, '클로바': 120}
```

- `Dict`에서 `Key`와 `Value`를 자유자재로 다룰 줄 알아야 한다.
  - `dict.items()` : `(Key , Value)` 쌍으로 `dict` 내의 모든 아이템을 보여준다.
  - `dict.keys()` : `Key`들의 모음을 `List`형식으로 반환한다.
  - `dict.values()` : `Values`들의 모음을 `List` 형식으로 반환한다.

```python
student_math_score_dict.items()
# dict_items([('양맹', 97), ('누구', 100), ('클로바', 120)])
student_math_score_dict.keys()
# dict_keys(['양맹', '누구', '클로바'])
student_math_score_dict.values()
# dict_values([97, 100, 120])
```

- 조금 더 심화된 `dict`의 사용예를 보자.
  - 갑자기 학교에 학생이 1000명이 생겨, `student_math_score_dict`가 1000명분의 데이터를 갖게 되었다고 가정하자.
  - 그 중에 `양맹`이라는 학생의 스코어만을 뽑아내고 싶다면?

```python
for key, value in student_math_score_dict.items():
	if k == '양맹': # key가 양맹이라면
		print(value) # 97
```

- 혹은, `string` 데이터에서 `key`와 `value`를 바꾼 `reversed dictionary`를 만들어야 하는 경우가 아주 자주 있을 것이다.

```python
string = '나는 오늘 밥을 먹었다. 내일은 라면을 먹을 것이다.'
string_list = string.split()
# ['나는', '오늘', '밥을', '먹었다.', '내일은', '라면을', '먹을', '것이다.']
# 이제 구분된 단어들에 고유 번호를 부여해서 dictionary에 넣어보자.
string_dict = dict()
for idx, word in enumerate(string_list):
    string_dict[word] = idx
print(string_dict)
# {'나는': 0, '오늘': 1, '밥을': 2, '먹었다.': 3, '내일은': 4, '라면을': 5, '먹을': 6, '것이다.': 7}

# 그럼 이번에는 list comprehension을 통해 reversed dictionary를 만들어보자. 
rev_string_dict = {v:k for k, v in string_dict.items()}
print(rev_string_dict)
# {0: '나는', 1: '오늘', 2: '밥을', 3: '먹었다.', 4: '내일은', 5: '라면을', 6: '먹을', 7: '것이다.'}
```

#### Collections 모듈

- `List`, `Tuple`, `Dict`에 대한 `Python Built-in` 확장 모듈이다.
- 주로 사용하는 모듈들은 다음과 같다.

##### deque

- 덱이라고 부른다. `Stack`과 `Queue`를 지원하는 모듈이다. 
- `List`로도 스택과 큐를 구현할 수 있지만, `deque`는 `List`보다 효율적이고 빠르다는 장점을 지닌다.
- `appendleft()`, `append()` 함수를 모두 갖고 있어서, `List`에서의 `insert(0, )`, `append()`와 동일한 기능을 수행한다. `List`의 기능은 전부 지원한다.
- 또한 `rotate`, `reverse` 등 `Linked List`의 특성도 지원한다.
- 참고) `List`는 끝으로 가면 처음으로 돌아올 수 없지만, `Linked List` 중 이중연결리스트는 처음과 끝이 연결되어 있다.

```python
from collections import deque

deque_list = deque() # 덱 객체 생성
for i in range(5):
    deque_list.append(i)
print(deque_list) # deque([0, 1, 2, 3, 4])

deque_list.appendleft(10)
print(deque_list) # deque([10, 0, 1, 2, 3, 4])
```



##### OrderedDict

- 데이터의 입력 순서대로 `dict`를 반환하는 딕셔너리이다.
- 다만, `Python 3.6`부터는 일반 `dict`도 입력한 순서를 보장하여 출력해준다.
- 그래서 딱히 이제는 쓸 일이 없는 .... 자료구조가 된 것 같다.

```python
from collections import OrderedDict

d = OrderedDict()
d['x'] = 100
d['y'] = 200
d['z'] = 300
d['l'] = 500

for k, v in sorted(d.items(), key=lambda t: -t[1]): # [('l', 500), ('z', 300), ('y', 200), ('x', 100)]
	print(k, v) 
```



##### defaultdict

- `Dict` 에 기본 값을 지정해서, 신규값을 생성시에 자동적으로 기본값이 지정되는 `Dict`
- 일반적으로 `Dict`는 원래 `Dict`에 없던 `key`를 출력하면, 해당 `key`가 없기 때문에 Error를 내지만, `defaultdict`에서는 자동으로 해당 `key`를 생성 - 기본값을 매칭해서 출력해준다.

```python
from collections import defaultdict
d = defaultdict(object)
d = defaultdict(lambda : 0) # lambda 함수로 기본 값을 지정해주어야 한다.
print(d['first']) # 0
```



##### Counter

- Sequence type의 data element들의 갯수를 `Dict` 형태로 반환하는 모듈.

```python
from collections import Counter
c = Counter('gallahad')
print(c) # Counter({'a': 3, 'l': 2, 'g': 1, 'h': 1, 'd': 1})

# Dict type, keyword Parameter 등도 처리 가능
c = Counter({'red': 4, 'blue' : 2})
print(c) # Counter({'red': 4, 'blue': 2})
print(list(c.elements())) # ['red', 'red', 'red', 'red', 'blue', 'blue']

c = Counter(cats=4, dogs=8)
print(list(c.elements()))
# ['cats', 'cats', 'cats', 'cats', 'dogs', 'dogs', 'dogs', 'dogs', 'dogs', 'dogs', 'dogs', 'dogs']
```

- 심지어 `Counter`는 `Set`의 연산들도 지원한다고 한다. 



##### namedtuple

- `Tuple` 형태로 Data 구조체를 저장하는 방법
- 저장되는 데이터의 variable을 사전에 지정해서 저장한다.

```python
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y']) # namedtuple 객체를 생성.(Point)
p = Point(11, y=22) # x=11, y=22를 갖는 'Point' tuple을 생성
print(p) # Point(x=11, y=22)
print(p[0] + p[1]) # 33

# unpacking
x, y = p

# 이름으로 호출
print(p.x) # 11
print(p.y) # 22
```

- `namedtuple`은 왜 사용할까?
  - 튜플은 보통 개별 항목에 `index`로 접근하기 때문에, 특정 인덱스가 특정 값을 의미하는 경우에도 직관적으로 이를 알 수 없다.
  - 하지만 `namedtuple`을 통하면, 보다 직관적으로 이름을 지정해서 사용할 수 있기 때문에 이러한 편리성이 있다. 마치, `Dict`의 `Key`를 불러오는 것과 비슷한 효과를 갖는다.

```python
from collections import namedtuple

# 3차원 공간의 두 점 사이의 유클리드 거리(L2 Norm)을 구해보자.
a = (10, 8, 7)
b = (15, -1, 3)

l2_norm = ((a[0]-b[0])**2 + (a[1]-b[1])**2 + (a[2] - b[2])**2) ** 1/2 # 61.0

# 이를 namedtuple로 변환해서 나타내면 보다 직관적인 계산식을 만들 수 있다.
Point = namedtuple('Point', ['x', 'y', 'z'])
a = Point(10, 8, 7)
b = Point(15, -1, 3)

l2_norm = ((a.x-b.x)**2 + (a.y-b.y)**2 + (a.z-b.z)**2) ** 1/2
print(l2_norm) # 61.0
# x좌표, y좌표, z좌표로 나타내니 보다 이해하기 편하다.
```







