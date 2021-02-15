---
title: Week4_1 Intro to NLP(2)
categories: [boostcamp]
tags: [부스트캠프, NLP]
comments: true

---

\- 이 강의정리본은 KAIST 주재걸 교수님의 강의를 정리한 것임을 밝힙니다.\-

본 과정은 NLP, Text Mining, Information Retrieval 중 NLP를 주로 다루는 수업이다. 그럼 자연어처리 분야의 처리 과정별 trend를 살펴보자.

### Word Embedding

- 최근 자연어처리 분야에서 가장 활발하게 사용하고 있는 Deep Learning을 이용한 자연어처리를 수행하기 위해서는, 일단 `text data`를 `numerical data`로 변환하는 과정이 필요하다. 왜냐하면 컴퓨터는 `text data`를 그 자체로 인식할 수 없기 때문이다.
- 주어진` text data`를 단어 단위로 분리하고, 각 단어를 특정한 차원의 `vector`로 표현하는 과정을 거쳐야한다. 이를 가리켜 `Word Embedding`이라 한다.

### RNN Model

- Word Embedding을 거친 문장은 `sequence data`이다. 즉, 순서가 존재하는 데이터이다.
- "나는 밥을 먹었다."와 "먹었다 밥을 나는" 이라는 문장은 단어들은 모두 동일하지만 단어들의 나열 순서가 다르다. 컴퓨터에게 문장에 등장하는 단어의 순서가 다른 경우 이 두 문장은 다른 문장임을 인식하게 해야한다.
- 그래서 이러한 `sequence data`를 처리하기 위해 등장한 모델이 `RNN model`이다. 
- `RNN` 계열 중에서도 `LSTM` 그리고 `LSTM`을 단순화하여 계산속도를 빠르게 한 모델인 `GRU`가 핵심을 이루었다.

### Transformer(Attention is All You Need)

- 2017년, `Google`에서 `NeuraIPS`에 '[Attention Is All You Need](https://arxiv.org/abs/1706.03762)'라는 논문을 발표한다.
- 기존 `RNN` 기반의 자연어처리 모델 구조를 `Self-Attention` 모듈로 대체할 수 있는 `Transformer`의 등장에 관한 내용이었다.
- `Transformer`는 초반에 `Machine Translation` 태스크를 처리하기 위해 제안되었다.
  - 기계번역 분야는 이미 다양한 모델들로 번역의 성능을 향상하려는 시도들이 존재해왔다.
  - ① 기존의 `Rule Based Model`은 다양한 언어별 문법들을 사용하여 언어-언어 변환이 가능하게끔 알고리즘을 구성했었다.
  - 하지만, 언어에는 너무나 많은 예외가 존재했고, 언어의 사용패턴을 일일이 대응시킬수도 없었다. 
  - ② 뒤이어 등장한 `RNN` 모델은 단순히 `sequence` 기반으로 translation을 수행했지만, `Rule Based Model`에 비해 월등히 높은 성능을 보여주었다.
  - ③ 그런데, 이것보다도 더 기계번역의 성능을 향상시킨것이 바로 `Transformer`였다. 
- 이제 `Transformer`는 단순 번역을 넘어 NLP의 다른 분야, 영상처리, 시계열 예측, 신약 개발, 신물질 개발 등 엄청나게 다양한 분야로 적용되고 있는 추세이다.
- 더욱 대단한 것은, `Transformer` 이전의 자연어처리에는 각 task 별로 특화된 model들이 따로 있어, 이들은 자신만의 task에서 좋은 성능을 내고 있었다.
  - 그러나 최근에는 `Self-Attention Module`을 단순히 쌓아나가는 식으로 모델의 크기를 키우고, 이 모델을 대규모 `text data`를 통해 `자가지도학습`(label이 별도로 필요하지 않은 학습)을 수행하여 학습시킨다.
  - 이러한 학습된 `pre-trained model`을 우리가 적용하고자 하는 task에 `전이학습`(transfer-learning) 형태로 적용하면, 각 task별로 특화된 기존의 모델들보다 우월한 성능을 내고 있다는 것이다.
- `pre-trained model`로는 최근 유명한 `BERT`, `GPT-2`, `GPT-3` 등이 존재한다.

### Pre-trained model 학습의 문제점

- `pre-trained model`을 학습시키기 위해서는 대규모의 data와 엄청난 양의 resource(GPUs)가 필요하다.
- 이는 개인이나 웬만한 단체에서도 수행하기 힘든 정도의 양이라고 한다.
- 예를 들어, `OPEN AI`의 `GPT-3`의 경우, 모델 학습을 위해 쓴 전기세만 수십억원에 달한다고 한다.
- 즉, `자본`과 `data`가 없으면 이런 모델을 학습하는 것이 불가능하다는 것이다.