---
title: Week4_3 RNN:Recurrent Neural Network
categories: [boostcamp]
tags: [부스트캠프, NLP]
math: true
comments: true
---

\- 이 강의정리본은 KAIST 주재걸 교수님의 강의를 정리한 것임을 밝힙니다.\-

### RNN

- $$h_{t-1}$$과 $$X_{t}$$를 결합하여 현 상태의 hidden state($$h_t$$)를 계산하는 것.
- 가장 중요한 특징은, 다른 time step에서 들어오는 입력벡터를 처리할 때 동일한 parameter를 갖는다는 것이다. 즉, Recurrent 모듈을 매 timestep에서 동일하게 사용한다.
  - RNN의 timestep마다 사용되는 가중치들($$W_{hh}$$, $$W_{xh}$$)이 동일하다는 의미이다.
- 이를 수식으로 나타내면

$$
h_t = f_W(h_{t-1}, x_t)
$$

이며, $$f_W$$는 RNN 모듈에 필요한 linear transformation matrix를 정의하는 파라미터이다. 

- $$h_t$$는 매 time step마다 나오는데, 이를 바탕으로 최종 출력값인 $$y_t$$(output vector)를 계산할 수 있다.
  - 1) 매 time step마다 계산 및 출력(ex. 품사 예측)
  - 2) 마지막 time step에만 계산 및 출력(ex. 감성 분석)

### $$W_hh$$ 와 $$W_xh$$

- $$h_t$$를 계산함에 있어, 해당 time step의 입력벡터 $$x_t$$와 $$h_{t-1}$$ 이 concat되어 입력으로 들어가게 됨.
- 예를 들어 입력벡터가 `3 x 1`, 이전 hidden state 벡터가 `2 x 1`의 shape을 가진다면 입력으로는 `5 x 1`짜리 벡터가 들어가게 됨.
- $$h_t$$는 $$h_{t-1}$$과 마찬가지로 `2 x 1`의 shape을 가져야하므로, 입력에 곱해지는 가중치의 shape은 `2 x 5`가 되어야 함. 
- 이 가중치 행렬이 입력과 내적되는 과정을 그림으로 보게되면, 첫 `2 x 3`까지는 입력벡터 $$x_t$$와 곱해지고, 나중 `2 x 2`는 $$h_{t-1}$$ 과 곱해짐을 알 수 있음.
  - 따라서, 가중치행렬의 첫 `2 x 3`은 `x`와 곱해져 $$h_t$$의 일부를 구성하므로, $$W_{xh}$$로 나타내고, 이후 `2 x 2`만큼은 $$h_{t-1}$$과 곱해져 $$h_t$$의 일부를 구성하므로 $$W_{hh}$$로 나타낸다.
- 또한, 이렇게 곱해진 값은 `tanh`의 activation function을 지나 최종적으로 $$h_t$$가 된다.
- 이에 따라 위의 수식을 풀어보면

$$
h_t = tanh(W_{xh}X_t + W_{hh}h_{t-1})
$$

로 나타낼 수 있으며,

- $$h_t$$를 output vector인 $$y_t$$까지 출력하고자 하는 경우,  output layer를 거치며 $$W_{hy}$$와 내적을 거쳐

$$
y_t = W_{hy}h_t
$$

이렇게 표현할 수 있다.

- 만약 binary classification task라면 1차원 스칼라 값을 받아 sigmoid를 거치고, multi-class classification이라면 클래스 개수만큼의 dimension을 가지는 vector를 softmax를 통과시켜 확률 분포를 얻을 수 있다.

### Type of RNNs

- 1) One-to-One
  - 사실상 sequence data가 아닌 구조이다. sequence 가 아닌 입력을 받아 sequence가 아닌 출력을 내는 구조이다.
  - 예를 들어, `키`, `몸무게`, `나이` 값을 받아 이 사람이 `저혈압`, `정상`, `고혈압`군에 속하는지 예측하는 task가 one-to-one task에 속한다.
- 2) One-to-Many
  - sequence가 아닌 입력을 받아, sequence data를 출력하는 구조이다.
  - `Image Captioning`, 즉, 하나의 이미지 데이터를 받아 해당 이미지에 대한 설명을 내는 task에 활용된다.
  - 이 때, 입력은 sequence가 아닌데, RNN을 통과하려면 입력을 sequence로 만들어줘야한다. 따라서, 이 경우에는 나머지 time step에 처음 입력과 동일한 사이즈의 벡터/행렬/텐서가 들어가되, 모든 값을 `0`으로 만들어주어 들어가게 된다. (torch.zeros 사용)
- 3) Many-to-One
  - sequence인 입력을 받아, 하나의 출력만을 내는 구조이며, 가장 일반적으로 이해하기 쉽고 많이 쓰이는 구조이다.
  - `감성분석` task가 대표적인 예이다. "I love this movie."라는 입력을 받아 최종적으로 이 문장이 긍정인지 부정인지를 예측하는 작업이다.
    - 입력으로 들어가는 문장의 길이가 달라지는 경우에는 RNN Cell이 확장된다.
- 4) Many-to-Many [1] : 기다렸다가 출력을 내는 구조
  - sequence 입력을 받아 sequence 출력을 내는 구조이다. 
  - `Machine Translation` task가 대표적인 예이다.
  - "I go home."이라는 입력문장을 받아 끝까지 읽은 후  번역에 해당하는 단어 sequence ('나는 집에 간다.')를 생성한다.
- 5) Many-to-Many [2] : 위와 달리 delay 없이 매 time step마다 출력값을 생성하는 구조
  - `POS Tagging` : 품사 태깅은 각 단어별로 output을 내야 하므로 이 구조에 해당한다.
  - `Video Classification on frame level` : 해당 frame이 어떤 scene인지 분류하는 작업(ex. 전쟁씬, 주인공 없는 씬, ...)

### Character-level language model

- '언어모델' : 주어진 문자열/단어를 바탕으로 다음 번 문자열/단어를 맞추는 것.

- 이를 간단한 character-level의 예제를 통해 구현해보자.

- "hello"

  - 1) 사전을 구축한다.

  ```python
  vocab = ['h', 'e', 'l', 'o']
  ```

  - 2) one-hot vector를 생성한다.

  ```python
  h = [1, 0, 0, 0]
  e = [0, 1, 0, 0]
  l = [0, 0, 1, 0]
  o = [0, 0, 0, 1]
  ```

  - 3) 모델을 구축한다.
    - `h`가 주어지면, `e`를 예측해야
    - `h`와 `e`가 주어지면, `l`을 예측해야
    - `h`, `e`, `l` 이 주어지면, `l`을 예측해야
    - `h`,`e`,`l`,`l`이 주어지면, `o`를 예측해야 함.
  - output layer는 `vocab size`만큼의 dimension을 가져야함.(softmax를 통과해서 vocab 내 문자별 확률을 예측해야하기 때문)
  - softmax 를 거쳐 나온 vector가 ground truth와 가장 가까워지게 하는 softmax loss를 적용하여 네트워크를 학습하여, 최종적으로 `h`가 들어갔을 때 `ello`가 순차적으로 나오게 해야한다.

### Inference

- Inference 하는 상황을 생각해보자.
- 테스트 데이터로는 `h`만 주어진다. 
- 학습된 RNN 모델은 첫번째 time step에서 `e`를 예측한다.
- 예측된 `e`는 두 번째 time step의 input으로 들어간다.
- 두 번째 time step에서 `l`이 예측된다.
- 예측된 `l`은 세 번째 time step의 input으로 들어간다.
- ..... 무한대 길이의 sequence 생성이 가능해진다.

### 고려해야할 사항

- (학습할 때) 첫 번째 time step에서는 아직 hidden state가 생성되지 않은 상태이다. 그럼 어떻게 해야할까?
  - 이 때는 hidden state의 dimension에 맞춰 `0`으로 채운 $$h_0$$가 들어간다.
- `Logit` : Softmax에 들어가기 이전, hidden state가 output layer를 통과하여 나온 값.
- 3번째와 4번째 step은 동일하게 `l`이 들어갔지만, 나오는 값은 `l`과 `o`로 각각 다르다. 어떻게 가능한걸까?
  - 바로 hidden state를 통해 가능하다. hidden state는 이전가지의 정보를 모두 포함하고 있으므로, time step 3에서의 hidden state와 time step 4에서의 hidden state가 다르기 때문에 output으로 출력되는 문자도 달라지는 것.
- 문장을 온전히 character level로 학습시킬 때는, `white space`도 하나의 문자로 인식되어 들어가야 한다. 그래야지만 단어와 단어 사이에 공백이 생기고, 문단이 끝나는 경우 줄바꿈을 할 수 있기 때문이다.
- `Truncation` : 만약 우리가 입력하는 sequence의 길이가 수천~수만의 길이를 갖는 경우, 이 네트워크를 GPU에 한 번에 올릴 수 없는 문제가 발생한다. 또한 올릴 수 있다 가정하더라도, 각 time step에서의 output을 모두 계산하고, loss function을 적용하고, back-propa를 하는 과정에 시간이 엄청나게 걸릴 것이다.
  - 따라서 현실적으로 이를 한꺼번에 처리하는 것은 무리가 있으므로, `truncation` 즉 일정한 길이만큼 sequence를 뭉텅이로 잘라 해당 길이의 sequence들을 학습하는 방식을 사용한다.

### Searching for Interpretable Cells

- `hidden state`가 3차원 vector라고 한다면, 1~3차원 중 어디에 어떤 정보가 저장되는걸까?
  - 한 차원을 고정하여 그 변화를 지켜보면 이것이 어떤 정보를 저장하는지 알 수 있다.
- 특정 cell이 특정 time step에서 (-)값을 갖는 경우 파란색으로, (+)값을 갖는 경우 빨간색으로 나타내는 방식을 통해 추이를 살펴보면,
  - 예를 들어, `Quote detection cell`을 볼 수 있다. 이 cell은 `따옴표`가 열리고나서부터 값이 (-)로 유지되다가, `따옴표`가 닫히는 순간부터 (+) 값을 나타낸다. 즉, 해당 cell이 속한 dimension은 따옴표가 열리고 닫히는 상태를 기억한다는 것이다.
  - 이외에도 다양한 상태를 기억하는 경우를 찾아볼 수 있다.

### Vanishing/Exploding Gradient

- Back-Propagation 과정 중에 생기는 기울기 소실 혹은 기울기 폭발 현상이다.
- RNN 구조에서는 $$W_{hh}$$ 의 값에 따라 기울기 소실/폭발 문제가 야기된다.
  - Back-Propagation 과정에서 앞 단으로 갈수록 $$W_{hh}$$ 값이 거듭 곱해짐에 따라, 이 값이 1보다 작은 경우에는 `기울기 소실` 문제가, 1보다 커지는 경우에는 `기울기 폭발`문제가 발생할 수 있다.