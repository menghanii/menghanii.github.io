---
title: Week4_4 seq2seq, Encoder-Decoder Architecture and Attention
categories: [boostcamp]
tags: [부스트캠프, NLP]
math: true
comments: true
---

\- 이 강의정리본은 KAIST 주재걸 교수님의 강의를 정리한 것임을 밝힙니다.\-

### seq2seq

- `RNN` 모델 중 `many-to-many[1]`의 구조를 갖는 모델.
- 즉, 입력 sequence를 모두 읽은 후 출력 sequence를 예측하는 모델이다.
  - `input`과 `output` 모두 word 단위의 문장이다.
- `Machine Translation`, `Dialog System` 등에 활용되는 구조이다.
- `Encoder-Decoder`구조를 가지고 있다.

### Encoder-Decoder

- 서로 parameter를 공유하지 않는 `2`개의 RNN(LSTM) 모듈로 이루어져 있는 구조이다.
- `Encoder`에서는 입력 문장을 받고, `Decoder`에서는 출력 문장을 생성한다.

#### Encoder

- 입력문장의 각 단어를 time step별로 입력받아 hidden state를 출력한다.
- 마지막 time step에서의 hidden state vector는 이전까지의 모든 sequence 정보를 담은 vector이다. 이 vector는 `Decoder`의 `h_0`으로 들어가게 된다.

#### Decoder

- `Encoder`에서 받아온 hidden state를 통해 문장을 생성해내는 구조이다.
- `<SOS>` : 문장을 시작함을 의미하는 토큰이다. 'Start of Sequence'의 약자이다.
- `<EOS>` : 문장의 끝을 알리는 토큰이다. 'End of Sequence'의 약자이다.
- `Encoder`의 마지막 step에서 생성된 hidden state vector가 `Decoder`의 `h0`로 들어가면, `Decoder `의 첫 input인 `<SOS>`와 결합하여 첫 번째 time step의 hidden state vector를 생성해냄.
  - hidden state vector가 output layer을 거치면서 vocab의 확률 분포를 생성하고, 확률이 가장 높은 단어가 첫 time step의 단어로 출력됨.
  - 첫 번째 time step에서 생성된 $$h_1$$은 두 번째 time step으로 전달되고, 첫 time step에서 출력된 단어가 두 번째 time step의 input($$x_2$$)으로 들어가서 $$h_2$$와 2번째 output 단어를 생성함.
  - 이러한 과정이 `<EOS>`가 output 단어로 등장할 떄까지 계속됨.
  - `<EOS>` 가 등장하는 순간 decoding이 종료된다.

### 순수 RNN(LSTM) 계열의 Encoder-Decoder의 문제점 : Long-Term Dependency

- Long-Term Dependency(장기의존성): time step이 더해갈수록 앞 부분 time step의 정보가 변질/손실되는 현상
- Encoder-Decoder 구조에서도, `Encoder`의 마지막 time step의 hidden state vector에 앞서 나왔던 모든 정보들을 넣어야 하는 문제가 있다.
  - $$h_t$$의 dimension은 정해져있기 때문에, 문장이 길어질수록 고정된 dimension 내에 sequence 전체의 정보를 압축하여 저장하게 되면 문제가 발생한다.
  - 이를 해결하기 위해 Machine Translation task에서, 문장을 `역(reverse)`으로 넣어, 문장의 초반 정보를 잘 담을 수 있게 하는 방법 등을 고안하였으나, 본질적인 문제는 해결되지 못함.

### Attention

- 위에서 언급한 문제를 해결하기 위해 등장한 개념.
- `Encoder`의 마지막 time step에서의 hidden state vector만을 사용하지 않고, `Decoder`의 매 time step마다 `Encoder`의 모든 time step별 단어들의 확률 분포를 계산하는 방식.
- 쉽게 이해하자면, `decoding`을 한 단어 한 단어 할 때마다 그 시점에 가장 관련성이 높은 `Encoder` 내의 단어를 찾아 **'집중'**하는 것이다.
- "Are you free tomorrow?" 라는 문장을 번역해보자.
  - `Are`, `you`, `free`, `tomorrow` 라는 네 개의 단어로 이루어진 입력 sequence 가 encoder에 들어가서 각각 $$h_1^{(e)}$$, $$h_2^{(e)}$$, $$h_3^{(e)}$$, $$h_4^{(e)}$$를 생성한다. 이 때 각 encoding hidden state vector(이하 ehsv)는 해당 time step에 있는 단어의 정보를 가장 많이 담고 있을 것이다.
  - `Encoder`를 거쳐 `Decoder`로 넘어가자. 
  - `Decoder`의 첫 번째 time step에서는 `Encoder`에서 넘어온 $$h_0$$와 `<SOS>` 토큰이 결합하여 decoding hidden state vector $$h_1^{(d)}$$를 생성한다. 여기까지는 기본 enc-dec 모델과 같다.
  - 여기서 $$h_1^{(d)}$$는 두 가지 역할을 담당하는데,
    - ① 다음 step의 단어 예측을 위해 이용된다.
    - ② 어떤 $$h_i^{(e)}$$를 필요로 하는지 선정하는 과정을 거친다. (Attention Score 계산)
  - $$h_1^{(d)}$$는 $$h_1^{(e)}$$, $$h_2^{(e)}$$, $$h_3^{(e)}$$, $$h_4^{(e)}$$과 각각 내적 연산을 수행하게 된다. (참고로 내적은 유사도를 구하기 위해 사용되기도 한다.)
  - 만약 예를 들어, 각각의 내적결과가 `7, 1, -1, -2`로 계산되었다고 해보자. 이를 `Attention Score`라고 한다.
  - 이 값들을 softmax 층을 통과시키면, $$h_1^{(d)}$$가 각 ehsv에 대응하는 확률값이 나타나게 된다. 이 값들을 `[0.85, 0.08, 0.04, 0.03]`이라고 하자. 이 확률 분포를 벡터화 한 값을 `Attention Vector`라 한다.
  - 이렇게 나타난 `Attention Distribution`은 4개의 ehsv와 곱해져 `가중평균`을 계산하게 되고, 이는 한 개의 vector로 $$h_1^{(d)}$$가 어떤 단어에 '집중'해야하는지를 알려주는 역할을 하는 `Context Vector`가 된다.
  - 최종적으로 $$h_1^{(d)}$$과 Context Vector가 `concat`되어 output layer의 입력으로 들어가며, 이로부터 비로소 첫 번째 time step에 나올 단어가 예측되는 것이다.
  - 두 번째 time step에서도 마찬가지로 $$h_1^{(d)}$$와 첫 번째 time step에 나왔던 단어가 들어가 $$h_2^{(d)}$$를 생성한다. 이후 각 ehsv와의 attention score 계산, context vector 생성 및 concat을 통해 output layer를 거쳐 두 번째 단어를 생성하게 된다.
  - 이러한 반복은 마찬가지로 `<EOS>`가 나타날 때까지 계속된다.
- 즉, attention mechanism의 핵심은, 
  - decoder의 매 time step별로 
  - encoder에 있는 모든 hidden state vector를 살펴 
  - decoder의 특정 time step과 가장 연관성이 높은 ehsv를 찾아 
  - 이를 decoder의 hidden state와 연결시켜 output을 생성하는 것이다.

### Attention Score을 구하는 3가지 방식

- ① dot product(내적)
  $$
  Score(h_t, \bar{h_s}) = h_t^T\bar{h_s}
  $$

  - $$h_t$$는 decoder의 hidden state, $$\bar{h_s}$$는 각 ehsv를 의미함.

  - $$h_i^{(d)}$$와 각 ehsv를 단순히 내적하는 방식으로 score를 구한다.

  - 위에서 언급한 방법과 동일하다.

    

- ② general dot product(일반화된 내적 방식)
  $$
  Score(h_t, \bar{h_s}) = h_t^TW_a\bar{h_s}
  $$

  - $$h_1^{(d)}$$ 와 각 ehsv의 중간에 학습가능한 행렬을 둠으로써 이 행렬의 값이 학습을 통해 최적화될 수있게끔 하는 방식.

    

- ③ Concat 기반의 attention 방식
  $$
  Score(h_t, \bar{h_s}) = v_a^Ttanh(W_a[h_t;\bar{h_s}])
  $$

  - $$h_t$$와 $$\bar{h_s}$$를 concat하여 Fully Connected layer를 생성한뒤

  - `W`를 지나게 하여 scalar값의 score를 받는 방식.

  - 혹은 `W`를 지나게 하여 벡터 형식으로 만든 후에 `W_2`를 거쳐 scalar 값의 score를 계산할 수도 있음.

  - 여기서 $$W_a$$ = `W`, $$v_a^T$$ = `W_2`를 의미한다. `v`로 표현되는 이유는 벡터의 형태를 띠고 있기 때문이다.

    

- attention score를 구하는 방식을 변화시키면, Back-Propagation이 일어날 때 attention layer를 거치면서 학습 및 최적화가 이루어질 수 있다.

### Attention의 장점 및 특성

- Neural Machine Translation에서 엄청난 성능 향상
  - 바로 기존 RNN 모델의 고질적 문제점인 Long-Term Dependency를 효과적으로 해결했기 때문이다.
- Bottleneck Problem Solved
  - 위와 비슷하다. encoder의 마지막 time step에서 고정된 hidden state의 dimension에 모든 sequence의 정보를 욱여넣어야 하는 병목현상을 attention을 통해 해결하였다.
- Vanishing Gradient 문제 해결
  - 만약 attention이 없었다면, decoder를 거쳐 encoder의 앞 단까지 오기 위해 엄청난 time step을 거쳐야 한다.
  - 그러나 attention을 이용함으로써, `shortcut path`가 생겼다고 볼 수 있다. 이를 통해 바로 Encoder의 특정 step에 gradient를 빠르게, 변질없이 전달할 수 있다.
- Provides some interpretability
  - attention의 패턴을 조사함으로써 Encoder 상의 어떤 단어에 attention이 집중되었는지를 알 수 있다.
  - 두 언어의 어순이 다른 부분은 역순으로 attention align이 이루어지기도 하고
  - 하나의 단어가 다른 언어의 두 단어로 이루어진 경우에는 attention이 두 개로 나뉘어져 집중되기도 한다.

### Teacher Forcing

- 각 time step별로 ground truth를 제공하는 학습방식.
- 실제 Inference는 각 time step 별로 이전의 ouput 단어가 새로운 input으로 들어가는 방식이기 때문에 Teacher Forcing을 활용한 학습 방법과 다르다.
- Teacher Forcing은 학습이 빠르고 용이하다는 장점을 가지고 있지만, inference 상황과는 다르다는 괴리가 존재한다.
- 따라서 Teacher Forcing & No Teacher Forcing을 적절히 결합하는 방법도 사용될 수 있다.
- 처음에는 Teacher Forcing 방법으로 학습을 어느정도 진행하다가, 모델 예측력이 일정 수준 이상 정확해지면 학습 후반부에는 No Teacher Forcing 방법을 진행하는 방식이다.