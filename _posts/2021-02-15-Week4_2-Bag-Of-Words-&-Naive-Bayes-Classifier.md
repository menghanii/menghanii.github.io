---
title: Week4_2 Bag Of Words & Naive Bayes Classifier
categories: [boostcamp]
tags: [부스트캠프, NLP]
math: true
comments: true
---

\- 이 강의정리본은 KAIST 주재걸 교수님의 강의를 정리한 것임을 밝힙니다.\-

### Bag-Of-Words

Bag-Of-Words는 간단하게, 하나의 가방에다가 모든 단어들을 담는 것을 의미한다. Unique한 단어들을 모아 단어장을 만들듯이 Vocabulary를 구축하는 것이다.

#### Step 1

예를 들어, "John really really loves this movie."와, "Jane really likes this song."이라는 문장이 있다고 해보자. 이 두 문장에 나온 unique한 단어들만을 모아서 Bag-Of-Words를 만들면 다음과 같다.

<p align='center' markdown="1">{"John", "really", "loves", "this", "movie", "Jane", "likes", "song"}<p>

#### Step 2

이제 이 단어들을 컴퓨터가 이해할 수 있도록 `numerical data`로 변환하는 과정을 거쳐보자. 이러한 변환 과정에는 간단한 `One-Hot Encoding`이 존재한다. Vocabulary 길이만큼의 차원을 갖는 벡터를 생성한 후 해당하는 단어의 dimension을 `1`로, 여타 단어를 `0`으로 mapping하는 과정이다.

위에서 만든 Bag-Of-Words (vocabulary의 길이 8)에서 One-Hot Encoding을 수행하면 다음과 같다.

<p align="center">John : [1 0 0 0 0 0 0 0]<br>
really : [0 1 0 0 0 0 0 0]<br>
    loves : [0 0 1 0 0 0 0 0] <br>
.<br>
.<br>
.</p>

`One-Hot Encoding`의 특성은 다음과 같다.

- 1) 어떤 단어든지 단어들 간의 `Eucleadian Distance`는 $$\sqrt(2)$$로 동일하다.
- 2) 또한 모든 단어 간의 `Cosine Similarity`는 0이다.
- 즉, 단어의 의미와 상관없이 모두 동일한 관계를 갖는다.

#### step 3

`One-Hot Encoding`을 거쳐 나온 단어들을 이제 문장 단위로 합쳐야한다. 처음에 제시했던 "John really really loves this movie."를 단어들의 결합으로 나타내보자. `sum`으로 나타낸다.

<p align="center">[1 2 1 1 1 0 0 0]</p>

### Naive Bayes Classifier

나이브 베이즈 분류기는, `베이즈 정리`를 이용하여 주어진 document가 어떤 클래스에 속하는지를 분류해주는 Classifier의 일종이다. 예를 들어, 다양한 기사들 documents가 있고 이들의 클래스가 각각 `정치(p_p)`, `경제(p_e)`, `사회(p_s)`의 3개 클래스로 나누어져있다고 하자.

나이브 베이즈 분류기의 분류방식은, 새로운 기사(d)가 주어졌을 때, 각 클래스(c)에 속할 확률 중 가장 높은 확률을 보이는 클래스로 분류(Maximum a Posteriori ; Most Likely Class)하는 방식이다.
$$
C_{MAP} = argmax P(c | d)
$$
`Bayes 정리`에 의해 `P(c|d)`는 `P(d|c) * P(c) / P(d)` 로 나타낼 수 있다. 여기서 분모인 `P(d)`는 '특정한 문서 d가 뽑힐 확률'을 의미하는데, 이미 d는 특정한 문서로 고정되어 있으므로 이를 `상수`로 취급할 수 있다. 이는 곧, `P(d)`를 operation 상에서 무시할 수 있음을 의미한다.

위에서 `P(d)`를 무시하게 되면, 
$$
C_{MAP} = argmax P(d|c)P(c)
$$
로 나타낼 수 있다. 이제 이 수식을 살펴보자.

- 1) $$P(d|c)$$ : 특정 클래스가 주어졌을 때, 해당 기사가 나타날 확률이다. 해당 기사 `d`가 나타날 확률은 곧 `d`에 나타난 모든 단어들이 동시에 나타나는 사건을 의미하므로, $$P(d|c) = P(w_1,w_2, ..., w_n|c)$$가 된다. 또한, 각 단어들이 등장할 확률이 `독립`이라고 가정할 수 있다면, $$P(w_1, w_2, ... w_n | c) = P(w_1|c) * P(w_2|c) ... P(w_n|c)$$ 로 나타낼 수 있다.
- 2) $$P(c)$$ : 기사(`d`)가 주어지기 이전에 각 Class가 나타날 확률을 의미한다. 즉, `test data`가 주어지기 전에 `train data`에서 각 `Class`의 비율을 의미하게 된다.
  - 예를 들어, `train data`에 정치기사가 3개, 경제 기사가 4개, 사회기사가 3개 있다면, `P(c_p) = 0.3`, `P(c_e) = 0.4`, `P(c_s) = 0.3` 이 된다.

위의 두 값을 모두 구할 수 있다면 Naive Bayes Classifier에서 필요로 하는 모든 parameter들을 추정할 수 있게 된다.

### Example

#### Training Set

① "Image recognition uses convolutional neural networks" - CV

② "Transformer can be used for image classification task" - CV

③ "Language modeling uses transformer" - NLP

④ "Document classification task is language task" - NLP

### Test set

"Classification task uses transformer"

#### Test set의 문장이 CV, NLP 중 어디에 속할지 Naive Bayes로 분류해보자.

- 1) $$P(c)$$ : `P(c_CV)=0.5`, `P(c_NLP)=0.5` 이다.
- 2) $$P(d|c)$$ 
  - `P(d|c_CV)` : `CV`에 속하는 unique한 단어의 개수는 14개이다. 
  - `P(d|c_CV) = P('classification'|c_CV) * P('task'|c_CV) * ... * P('transformer'|c_CV)` 이고 `P('classification'|c_CV) = 1/14` 로 계산된다. ('Classification'이라는 단어가 CV 클래스의 단어 14개 중에 한 번 등장했기 때문)
  - 이렇게 계산해보면,
- $$P(c_CV) * P(d|c_CV)$$ = 0.5 * 1/14 * 1/14 * 1/14 * 1/14 
- $$P(c_NLP) * P(d|c_NLP)$$ = 0.5 * 1/10 * 2/10 * 1/10 * 1/10 
- 주어진 document가 `NLP`일 확률이 더 높으므로, NaiveBayes 분류기는 해당 문장을 `NLP` 클래스로 분류할 것이다.

#### 문제점?

- `test set`의 단어 수가 많아질수록 작아지는 확률값 : 단어 수가 많아질수록 해당 클래스에 속할 확률값은 계속해서 작아진다. 문제는 컴퓨터가 지나치게 작은 값을 정확하게 계산하지 못하는 데에 있다. 따라서 보통 이러한 문제를 해결하기 위해 log-likelihood를 사용하여, 확률의 곱을 `합`의 문제로 바꾼다.
- `test set`에 `training set`에는 등장하지 않았던 단어가 하나라도 등장하는 경우 : 이 경우 해당 단어가 특정 클래스에서 나타날 확률값이 0 이 되기 때문에 모든 클래스에 속할 확률이 `0`이 되는 문제가 발생한다. 이를 해결하기 위해 추가적인 regularization 방법이 활용된다.
  - `Smoothing`값을 주어 0을 방지하기도 한다. 즉, 등장하지 않았던 단어라 하더라도 확률값을 0이 아닌 값으로 만들어주어 전체적인 확률 값을 계속 유지시켜주는 방법이다.

