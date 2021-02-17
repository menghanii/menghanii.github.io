---
title: Week4_3 LSTM & GRU
categories: [boostcamp]
tags: [부스트캠프, NLP]
math: true
comments: true
---

\- 이 강의정리본은 KAIST 주재걸 교수님의 강의를 정리한 것임을 밝힙니다.\-

### LSTM(Long Short-Term Memory)

- `RNN`이 갖는 단기기억을 보다 오랫동안 유지할 수 있게끔 했다는 의미에서, `Long``Short-term memory`라 부름.
- `RNN`의 고질적인 문제인 `Long Term Dependency`, 즉 time step이 더해갈수록 sequence 앞 부분의 정보가 희미해지는 현상을 상당 부분 해결한 모델.
- RNN 모델의 hidden state를 구하는 식을 살펴보자.

$$
h_t = f_W(x_t, h_{t-1})
$$

- 즉, hidden state는 이번 time step의 입력과 이전 hidden state를 통해 구해지고 있다.
- LSTM은 어떤 방식으로 작동하는걸까?

$$
\{h_t, C_t\} = LSTM(x_t, C_{t-1}, h_{t-1})
$$

- RNN에는 없던 $$C_t$$ 가 생겨난것을 볼 수 있다.
- 이를 `Cell State`라고 한다. 
  - RNN의 $$h_t$$보다 완성된, 보다 여러가지 정보를 포함하는 벡터로 이해하자.
  - 그럼, LSTM에서의 hidden state는 무슨 역할을 할까?
  - cell state를 한번 더 가공하여, 특정 time step에서 노출할 필요가 있는 정보만을 남기는 벡터로 이해할 수 있다.

### LSTM의 구조

<p align='center'><img src="https://user-images.githubusercontent.com/37925813/108066415-a890b200-70a2-11eb-807b-38550e6fc966.png"></p>

- LSTM 역시 입력 $$x_t$$와 이전의 hidden state $$h_{t-1}$$을 받는다.

- 그러나 LSTM의 특징은 마치 컨베이어벨트처럼 `Cell State`를 계속 흘러가게 하는 데 있다.

  - 이 `Cell State`가 흘러가는 와중에, 각 time step에서 많은 일들이 발생하게 된다.
  -  $$x_t$$와 $$h_{t-1}$$는 가중치행렬과 결합하면서 `i`, `f`, `o`, `g`의 4개의 `gate`를 생성하는데, 이들은 이전 time step에서 넘어온 $$C_{t-1}$$을 적절하게 변환 및 새로 업데이트할 $$C_t$$를 생성하는데 사용된다.

- `Forget Gate` : $$C_{t-1}$$ 에서 넘어온 정보 중 잊어버릴 정보에 관여한다.

  - 만약 $$C_{t-1}$$이 `3 x 1`의 간단한 벡터라고 가정하고, 그 값이 [3, 5, -2]라고 해보자.
  - $$x_t$$와 $$h_{t-1}$$의 결합 및 `sigmoid`를 거친 `forget gate vector`가 [0.7 0.4 0.8]의 값을 가지고 있다면, (sigmoid는 모든 값을 0~1 의 값으로 변환한다.)
  - 이 두 벡터 간에 `element-wise` 곱 연산이 발생하며, $$C_{t-1}$$이 업데이트된다.
  - [3, 5, -2]와 [0.7, 0.4, 0.8]이 곱해지면 [2.1, 2, -1.6]이 된다. 즉 벡터 차원당 각각 30%, 40%, 20%의 정보를 **'Forget'**한 것이다.

- `Input Gate` : 새로 만들어지는 $$\tilde{C_t}$$와 결합하여, 얼마만큼의 새로운 정보를 input할지를 결정하는 역할을 담당한다.

  - $$\tilde{C_t}$$ ? 

  $$
  \tilde{C_t} = tanh(W_c[x_t, h_{t-1}] + b_c)
  $$

  - 즉, 이번 time step에서 이전의 cell state 정보와 결합할 새로운 cell state '후보'이다.
  - 이 '후보'가 $$x_t$$와 $$h_{t-1}$$의 결합 및 `sigmoid`를 거친 `input gate vector`와 `element-wise`로 곱해지면서 그 중 얼만큼의 정보를 cell state로 전달할지 결정된다.
  - 즉, `input gate`가 하는 역할은 사실상 `forget gate`가 하는 역할과 매우 유사하다.
    - `forget gate`는 이전 cell state의 정보를 일정 부분 잊는 역할을 담당했다면
    - `input gate`는 이번 cell state의 정보를 일정 부분 가져가는 역할을 담당한다.

- `Gate Gate` : `Forget gate`를 거친 이전 cell state 와 `input gate`를 거친 이번 cell state '후보'를 합쳐 $$C_t$$를 생성하는 역할을 담당한다.

$$
C_t = forget * C_{t-1} + input * \tilde{C_t}
$$

- `Output Gate`: 이번 time step에서 내보내야할 hidden state를 만드는데 사용된다.
  - `output gate`는 0~1 사이의 값을, `tanh`를 거친 `C_t`는 -1 ~ 1의 값을 갖는다.

$$
h_t = output * tanh(C_t)
$$

- $$C_t$$ 와 $$h_t$$ 의 차이점
  - $$C_t$$는 처음부터 기억해야할 모든 정보를 담는 벡터인 반면,
  - $$h_t$$는 현재 time step에서 필요한 정보만을 filtering한 벡터이다.
  - 예를 들어, `"hell`까지 입력된 경우를 가정해보자.
    - `Cell State`는 언젠가 따옴표로 마쳐야한다 라는 전체적인 정보를 기억하고 있다면,
    - `hidden state`는 당장 지금 `o`가 나와야한다는 정보만을 담는 것으로 이해할 수 있다.

### GRU(Gated Recurrent Units)

<p align="center"><img src="https://user-images.githubusercontent.com/37925813/108066285-7a12d700-70a2-11eb-940f-3248909eb789.png"></p>

- `LSTM`을 경량화하여 연산을 줄이고, 속도를 향상시킨 모델이다.

- 특징

  - `Cell State` 및 `hidden state`로 나눠졌던 `LSTM`에 비해, `GRU`는 `RNN`과 같이 하나의 `hidden state`만을 이용한다.
  - 두 개의 독립된 `Gate`(`input`, `forget`)에서 하던 연산을 하나의 게이트로 줄이면서 연산을 줄였다.

- `GRU`에서 $$h_t$$가 연산되는 과정을 알아보자.

  - 먼저, `LSTM`은

  $$
  C_t = forget * C_{t-1} + input * \tilde{C_t}
  $$

  - `GRU`는

  $$
  h_t = (1-input)*h_{t-1} + input*\tilde{h_t}
  $$

  - 두 구조의 차이점을 하나하나 분석해보자.
    - 1) $$C_t$$ 가 아닌 $$h_t$$가 쓰였다.
    - 2) `forget`이 없이 `input`만이 쓰였고, `forget` 대신 `1-input`이 들어갔다.
  - 이에 따라 $$h_t$$는 $$h_{t-1}$$과 $$\tilde{h_t}$$의 `가중평균` 형태로 계산됨을 알 수 있다.

- 이렇게 `state`의 수와 `gate`의 수를 줄이면서, `GRU`는 `LSTM`에 비해 훨씬 빠른 연산을 수행한다. 또한, 특정 task에 있어서는 더 좋은 성능을 보이기도 한다고 전해진다.