---
title: Week4_2 Word Embedding - Word2Vec
categories: [boostcamp]
tags: [부스트캠프, NLP]
math: true
comments: true
---

\- 이 강의정리본은 KAIST 주재걸 교수님의 강의를 정리한 것임을 밝힙니다.\-

### Word Embedding

- 각 단어들을 특정한 공간 상의 한 점, 즉 vector로 나타내는 기법이다.
- `Cat`, `Kitten` : 고양이, 아기고양이로 유사한 의미를 갖는 단어이다. 따라서 이들의 embedded vector는 유사한 representation을 가질 것으로 예측할 수 있다.
- `Hamburger`, `Cat` : 햄버거와 고양이는 유사성이 매우 낮다. 따라서 representation 상 먼 distance를 가질 것임을 예측할 수 있다.
- 즉, Word Embedding은 다양한 단어들 간 유사도를 잘 반영할 수 있게끔 수행되어야 한다.

### 구체적인 Word Embedding 방법 中 Word2Vec에 대하여

Word2Vec은 **'같은 문장에서 나타난 인접 단어들 간에는 의미가 비슷할 것'**이라는 가정에서 출발하여 만들어진 Word Embedding 기법이다. Word2Vec을 처음 제안한 논문은 [여기](https://arxiv.org/pdf/1301.3781.pdf)서 볼 수 있다. 예를 들어, 'The cat purrs.' 혹은 'This cat hunts mice.'에서 `cat`을 중심단어로 봤을 때, `The`, `This`는 `cat`을 꾸며주는 관계, `purrs`, `hunts`는 `cat`이 할만한 행동,  `mice`는 `hunts`의 대상으로서 `cat`과 관련이 있다. 즉, Word2Vec의 가정이 꽤 신빙성 있는 가정인 셈이다.

Word2Vec은 주어진 학습 data를 바탕으로, 중심단어들의 주변에 나타나는 단어들의 확률 분포를 예측하게 된다. $$p(word\|"Cat")$$을 예측하는 것이다. 보다 구체적인 학습방식으로, Word2Vec에서는 `Sliding Window`기법을 적용하여 `window size`만큼 중심단어의 앞뒤 단어들을 본다. 

"Today I went to school."이라는 문장에서 `window size`를 `2`로 주게 되면, 중심단어의 앞뒤로 각각 `2`개씩의 단어를 보게된다. Word2Vec은 이를 가지고 **(중심단어, 주변단어)의 입-출력 pair**들을 만들게 되는데, 이 data를 `2-layer neural network`에 입-출력시킨다. 

<p align="center"> 
    Today : (Today, I), (Today, went)<br>
    I : (I, Today), (I, went), (I, to)<br>
    went : (went, Today), (went, I), (went, to), (went, school)<br>
	to : (to, I), (to, went), (to, school)<br>
    school : (school, went), (school, to)
</p> 

위의 문장에서 vocab size = 5 이므로, `input layer` 및 `output layer`의 노드 수는 5개가 된다. 중간의 `hidden layer`의 node 수는 사용자가 정하는 hyperparameter이다. 따라서 이 `2-layer neural network`에는 2개의 가중치 벡터가 형성된다.  하나는 `input layer`에서 `hidden layer`로 갈 때 `input vector`와 결합하는 가중치 벡터이고, 다른 하나는 `hidden layer`에서 `output layer`로 갈 때 `hidden layer`와 결합하는 가중치 벡터이다.

위에서 구한 (중심단어, 주변단어)를 이 `2-layer NN`에 각각 입-출력으로 넣는다. `hidden node`의 수를 `2`로 가정해보자. 입력 벡터는 `5 x 1`의 shape을 가지며, 가중치벡터 `w1`(`2 x 5`)와 내적하여 `2 x 1`의 행렬이 나타난다. 이를 다시 가중치 벡터 `w2`(`5  x 2`) 와 내적하게 되면 출력 벡터의 shape은 `5 x 1`이 된다. 

중요한 것은 `2-layer NN`을 거쳐 출력되는 출력벡터의 값이 우리가 갖고 있는 ground truth 값인 `주변단어`의 벡터와 최대한 가까워지는 것이다. 이 때 `Softmax Loss(CrossEntropyLoss)`를 이용하여 Gradient Descent를 진행하며 최적화를 수행하고, 이 과정에서 `w1`과 `w2`가 학습된다. **이 `w1`과 `w2`는 각 단어의 정보를 지니고 있는 행렬이다.** 

`w1`과 `w2`중 어떤 가중치 행렬을 Embedding Vector로 써도 상관은 없지만, 보통은 `w1`에 해당하는 vector를 최종 결과로 사용한다고 한다.

### Word2Vec의 2가지 모델

Word2Vec을 처음 제시한 논문에서는 두 가지 모델을 제안하였다. 첫 번째가 **CBOW**, 두 번째가 **Skip-Gram**이다. 핵심은, CBOW는 `주변단어를 통해 중심단어`를, Skip-Gram은 `중심단어를 통해 주변단어`를 예측하는 알고리즘이다.

#### CBOW(Continuous Bag-Of-Words)

"I am going to school." 이라는 문장이 있다고 하자. Window_size = 2라면, 양옆으로 2만큼 본다는 것이다. 이 모델은 주변단어를 통해 중심단어를 예측하는 모델이다.

'going'을 중심단어로, 'I', 'am', 'to', 'School'을 주변단어로 가정하자. 주변단어들 각각을 one-hot vector로 만들어서 특정 projection layer를 거쳐 embedding을 해주고, 이들을 합친(sum) 다음에 다시 어떤 layer를 거쳐서 one-hot vector를 만들어주는데, 이 one-hot vector가 'going'의 one-hot vector에 가깝도록 하는 방법이 CBOW이다.

#### Skip-Gram

CBOW와 달리, Skip-Gram은 중심단어를 통해 주변단어를 예측한다. 

'going'을 어떤 레이어를 거쳐 embedding시켜준 다음에, 다시 어떤 linear layer를 거쳐, 각각 주변 단어들의 one-hot vector를 만든다.(이 예시에서는 총 4개의 one-hot vector). 그리고 각각이 'I', 'am', 'to', 'school' 각각의 one-hot vector와 최대한 가까운 vector들이 될 수 있도록 학습한다.