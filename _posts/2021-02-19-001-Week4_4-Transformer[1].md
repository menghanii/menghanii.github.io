---
title: Week4_5 Transformer[1]
categories: [boostcamp]
tags: [부스트캠프, NLP]
math: true
comments: true
---

\- 이 강의정리본은 KAIST 주재걸 교수님의 강의를 정리한 것임을 밝힙니다.\-

### Transformer

- `LSTM`, `GRU` 기반의 `seq2seq` 모델의 성능을 개선함.
- `Attention is all you need`라는 논문을 통해 처음 소개됨.
  - add-up으로만 사용되던 attention 모듈을 사용하여 아예 `RNN`을 대체해버림.
  - 오로지 attention 만으로 sequence data를 입력받아 sequence를 예측하는 모델구조.
- 기존 `RNN`의 고질적인 문제 : Long-Term Dependency
  - encoder의 마지막 step의 hidden state에 이전까지의 모든 sequence가 담김.
  - 초기정보의 유실/손실/변질이 발생함.

#### Bi-Directional RNN

- Long-Term Dependency를 해결하기 위한 하나의 방법으로 제안됨.

- 아이디어 : **순방향-역방향의 모든 sequence 정보를 고려하자!**

- 일반적인 RNN은 왼쪽에서 오른쪽으로 sequence가 흘러가며, 이를 `Forward RNN`이라고 한다.

- 이를 역방향으로 바꾸면 `Backward RNN`이 되며, 이 RNN 모델은 `Forward RNN`과는 또다른 모델이므로, parameter를 share하지 않는 독립적인 모델이다.

- 예를 들어, "I go home."이라는 문장을 `Bi-Directional RNN`으로 분석하면,

  - `Forward`는 "I go home", `Backward`는 "home go I" 순으로 encoding이 이루어짐.
  - `Forward`에서 "go"라는 단어의 hidden state는 "I go" 까지의 정보를 encoding.
  - `Backward`에서 "go"라는 단어의 hidden state는 "home go"까지의 정보를 encoding.

  - 이 두 RNN을 병렬적으로 만든 후, 동일 단어에서의 hidden state를 가져와서 병렬적으로 `concat`함. 즉, 원래 hidden state의 2배의 차원을 가지는 새로운 hidden state들이 각 sequence의 단어별로 생성됨.

- 따라서, 하나의 단어에 해당하는 hidden state는 이전까지의 정보와, 이후로부터의 정보를 모두 담고 있음.

#### 다시 Transformer

- 입-출력의 setting은 RNN과 동일하다. 즉, "I go home."이라는 문장이 들어왔을 때, 각 단어의 embedding vector를 입력받아 각 단어별 hidden state를 output으로 내놓는 구조.
- 그러나 input - output 사이에서 일어나는 일이 완전 달라진다.
- `Self - Attention`
  - 이전까지의 attention은 decoder의 각 timestep 별 `h_t`와 `input`이 들어가서 hidden state를 생성하고, 이것이 attention module을 거쳐 각 encoding hidden state vector(ehsv)와의 내적 유사도를 구하는 방식으로 attention scores를 구했음.
  - 그러나 self-attention은 그 단어의 의미에서도 알 수 있듯이, encoding input으로 들어온 단어 스스로(self)를 decoder의 hidden state vector처럼 사용해서, 자기자신 및 나머지 input 단어들과의 내적을 수행하는 방식을 사용한다.
    - 즉, ehsv와 dhsv 간의 구분이 없어지는 것이다. 스스로가 스스로와의 유사도를 구한다.
  - 근데, self-attention을 한다고 생각해보면, 당연히 스스로와의 내적 유사도가 가장 높게 나타날 것임을 생각해볼 수 있다. 이 문제를, 작동과정을 좀 더 확장해서 개선해보자.
- `Query (vector)` 
  - 말 그대로 `'질의'`를 하는 벡터를 의미한다. 
  - "I go home."에서 "I"에 대한 self-attention을 수행하고자 하는 경우, "I"에 해당하는 embedding vector가 `query vector`가 된다.
  - 이 `query vector`는 `key vectors`와의 내적을 통해 유사도가 구해진다. 그럼 `key vectors`는 뭐지?
- `key (vectors)`
  - encoder input으로 들어가는 각각의 단어 벡터**들**이다.
  - 역할 : 각각의 `key vector`들은 `query vector`와의 내적으로 유사도가 구해지고, 어떤 `key vector`가 높은 유사도를 갖는지 구하게 된다.
  - `Softmax`를 통과하여 합이 `1`이 되도록 하는 가중치값들이 구해진다.
  - 이 가중치값은 `value vectors`에 각각 적용된다.
- `value (vectors)`
  - 얘도 `key vectors`와 마찬가지로 각각의 단어벡터**들**이다. 실체는 같고, 다만 역할이 다르다.
  - 역할 : 가중치가 적용되어 `query vector`의 time step에 해당하는 hidden state를 출력하는데 사용된다.

### 예제 : "I go home."

- "I"를 query vector로 하여 self-attention을 수행해보자.
- ① $$W^Q$$ : "I"의 embedding vector와 곱해져서 "I"에 해당하는 `query vector`를 생성함.
- ② $$W^K$$, $$W^V$$ : "I", "go", "home"을 모두 받아서 각각 `key vectors`와 `value vectors`를 생성함. (`key vectors`와 `value vectors`의 개수는 동일하다.)
- ③ `query vector`를 각 `key vector`들과 내적한 후, `softmax`값을 취하여 가중치 값으로 만들어줌. (이렇게 하면, "I"에 해당하는 query vector와 "I"에 해당하는 key vector의 유사도가 꼭 가장 높은 유사도를 갖지 않을 수도 있다고 한다.)
  - "I"와 "I", "go", "home"의 softmax 가중치가 [0.2, 0.1, 0.7]로 나왔다고 하자.
- ④ 이를 각각의 `value vector`에 곱해준다. 즉, 가중평균의 결과로서 최종적인 vector가 나오게 되는데, 이것이 $$h_1$$ 이다.
- "go"와 "home"의 경우에도 마찬가지 방식을 따른다.