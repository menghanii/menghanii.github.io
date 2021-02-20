---
title: Week4_4 Beam Search & BLEU Score
categories: [boostcamp]
tags: [부스트캠프, NLP]
math: true
comments: true

---

\- 이 강의정리본은 KAIST 주재걸 교수님의 강의를 정리한 것임을 밝힙니다.\-

### Beam Search

- 자연어 생성 모델에서, 더 좋은 품질의 생성 결과를 얻을 수 있도록 적용하는 기법

- `Joint Probability` : Decoder가 생성하는 문장은 여러 단어로 이루어져 있고, 앞 단어가 주어졌다는 가정하에 특정 단어가 나올 확률을 모두 곱한 `결합확률(joint probability)`이 최대가 되도록 하는 것이 최선의 문장 생성방법이다.
  $$
  P(Y|X) = P(y_1|X) *P(y_2|y_1, X)*P(y_3|y_1, y_2, X) * ... * P(y_\tau | y_1,y_2,...,y_{\tau-1},X)
  $$

  - $$Y$$는 출력 문장, $$X$$는 입력 문장, $$y_1, ... , y_\tau$$는 출력 문장을 이루는 각 단어들이다.

- Decoding의 여러 방법

  - `Greedy Decoding` : 매 time step마다 가장 높은 확률 분포를 보이는 단어 하나만을 선택해서 Decoding을 진행하는 방법
    - 문제점 : 특정 time step에서 단어를 ground truth와 다르게 생성한 경우에도, 뒤로 돌아갈 수 없음.
    - 문제점2 : Greedy Decoding을 통해 생성한 문장이 가장 높은 `Joint Probability`를 가진다고 할 수 없음. 예를 들어, 첫 번째 time step에서 가장 높은 확률을 가지는 단어(가령 'a')를 선택했지만, 이후 (n-1)번의 sequence의 `joint probability`값이 최대라고 볼 수 없다. 
    - 오히려 첫 번째 time step에 두 번째로 높은 확률 분포를 가지는 단어 'b'를 선택해야 그 이후의 `joint probability`가 최대가 될 수도 있음. 그러나 Greedy Decoding은 이를 고려하지 않는다.
  - `All Possible Answers` : 매 time step마다 모든 단어의 확률 분포를 가능성에 두고 Decoding을 진행하는 방법
    - 문제점 : 모든 time step에 대해, 모든 vocab에 대한 시나리오를 모두 만들어주어야 하므로, 현실적으로 계산이 불가능하다.
    - time step이 $$t$$번이고, $$V$$개의 단어에 대해 각 time step별로 가능한 경우의 확률 분포를 모두 계산하면, $$V^t$$의 all possible answers 가 발생하게 된다.
  - `Beam Search` : `Greedy Decoding`과 `All Possible Answers`의 중간으로, Decoder의 매 time step 마다 `k`개 의 가능한 상황만을 고려하는 것. 

#### Beam Search의 작동원리

- 일반적으로 `k` = 5 ~ 10의 값을 갖는다. 즉, 결합 확률을 계산할 문장(hypothesis라 한다.)을 5~10개 고려하겠다는 것이다. `k`는 `Beam Size`라 한다.
- 위에서 구했던 결합확률분포에 `log`를 씌우면 결합확률분포의 곱셈이 `덧셈`으로 바뀌게 된다.

$$
Score(y_1, ..., y_t) = logP_{LM}(y_1, ...,y_t|X)=\sum_{i=1}^t{logP_{LM}(y_i|y_1,...,y_{i-1},X)}
$$

- `log`는 단조증가함수이므로 곱셈을 덧셈으로 바꾸어도 크기 순서는 변하지 않는다.
- '그는 나를 파이로 때렸다.'라는 문장을 한 번 `k`=2의 Beam Search를 통해 decoding 해보자.
  - 먼저, encoding은 모두 마쳤고, encoder의 마지막 time step에서 hidden state를 받았다고 가정하자.
  - Decoder의 첫 번째 input으로 $$h_0$$(encoder 마지막 h_t)와 `<SOS>` 토큰이 주어진다.
  - Decoder는 모든 vocab의 확률분포를 계산한 후, 가장 높은 `2`개의 단어를 생성한다. (`he`, `I`)
    - `he`의 $$Score=logP_{LM}(he|\<sos\>)$$ = -0.7
    - `I`의 $$Score=logP_{LM}(I|\<sos\>)$$ = -0.9 로 나왔다고 하자.
  - 이제 이 `2`개의 hypothesis에 대해 다음 time step의 단어들을 예측해보자. 
    - `he`의 다음 단어 `2`개를 뽑았더니, `hit`과 `struck`이 나온다.
    - `I`의 다음 단어 `2`개를 뽑았더니, `was`와 `got`이 나온다.
    - 각각의 경우에 대해, `hit`과 `was`가 가장 확률이 높았으므로 `he hit`과 `I was` 두 개의 hypothesis로 추려낸다.
  - 추려낸 `2`개의 hypothesis에 대해 3번째 time step의 단어들을 예측해보자.
    - `he hit` 다음 단어 `2`개를 뽑고, 확률이 높은 한 개를 추린다.
    - `I was` 다음 단어 `2`개를 뽑고, 확률이 높은 한 개를 추린다.
  - 이러한 방법으로 계속하여 `k=2`를 유지한 채로 hypothesis를 `<EOS>`가 나타날 때까지 이어가면, 총 `k`개의 가능한 문장이 생성된다.
    - 이 때, 각 hypothesis에서 `<EOS>`가 나오는 time step은 각기 다를 수 있다. 
    - 그러나, 각 hypothesis는 독립적으로 진행되므로 각 hypothesis는 서로에 영향을 미치지 않는다.
    - 그런데 Beam Search에서도 `cut-off point`는 존재한다.
      - ① 정해놓은 time step `T`에 도달하는 경우
      - ② 최소 `n`개의 완성된 hypothesis가 있는 경우, 더 이상의 문장을 생성하지 않고 종료한다.
  - Beam Search가 완료되면 우리는 완료된 문장들의 리스트를 받게된다.
  - 이 중 가장 높은 `Score`를 갖는 단 하나의 hypothesis를 뽑아야 한다. 그러나 이 때 한 가지 문제가 있다.
  - Sequence의 길이가 긴 경우, 즉 어떤 문장은 짧고 다른 문장은 긴 경우, 긴 문장이 상대적으로 Score가 낮을 가능성이 높다. 이는 짧은 문장이 좋은 문장이라는 의미가 된다.
  - 이를 해결하기 위해 우리는 `Normalization`을 수행한다.
    - hypothesis별로 해당 문장이 가지는 단어의 개수(`t`)로 score를 나눠주는 방식이다.
    - 이렇게 하면 모든 문장이 길이에 영향을 받지 않는 score를 가질 수 있게 된다.
  - 따라서, `Normalization`을 수행한 hypothesis의 score 중 가장 높은 확률값을 가지는 hypothesis를 최종 선정한다.

### BLEU Score

- 생성해낸 문장이 ground truth 문장과 비교하여 잘 생성되었는지를 평가하는 지표이다.

<p align='center' style='font-weight:bold'>reference : Half of my heart is in Havana ooh na na</p>

<p align='center' style='font-weight:bold'>prediction : Half <span style='color:red'>as</span> my heart is in <span style='color:red'>Obama</span> ooh na <span style='color:red'>__</span></p>

- 위와 같은 ground truth 문장과, decoder가 생성해낸 문장이 있다고 가정하자.

- prediction 문장이 ground truth에 비해 얼마나 잘 생성된 것인지 평가하는 지표에는 어떤 방법이 있을까?

  - ① 각 time step별로 해당 단어가 출현했는지

    - `Half-Half`, `of-as`, `my-my` ... 이런 식으로 각 time step 별로 단어가 매치되는지 확인하는 방법이다.
    - 그러나, prediction 문장의 중간에 단어가 하나만 빠져도, 혹은 하나만 더 생겨도 이후의 단어들이 모두 틀리는 문제가 발생한다.
    - 따라서 올바른 성능지표로 채택할 수 없다.

  - ② Precision & Recall 및 F-measure

    - `Precision(정밀도)` 

    $$
    precision=\frac{맞은\ 단어의\ 개수}{예측한\ 문장의\ 길이}
    $$

    

    - 위의 문장에서 정밀도를 계산해보면, prediction 문장의 길이는 `9`이고, 맞은 단어의 개수는 `7`이므로 `7/9`이 된다.
    - `Recall(재현율)`

    $$
    recall=\frac{맞은\ 단어의\ 개수}{타겟\ 문장의\ 길이}
    $$

    

    - 위 문장에서 재현율을 계산해보면, reference 문장의 길이는 `10`이고, 맞은 단어의 개수는 `7`이므로 `7/10`이 된다.
    - 직관적으로, precision은 예측된 결과가 노출되었을 때 실질적으로 느끼는 정확도를 의미하고, recall은 실제의 정답에서 몇 개나 맞았는지를 보여주는 정확도를 의미한다.
      - 만약, 우리의 예측에 얼마나 부합하는지를 보고싶을 때는 precision이 중요한 measure가 되고,
      - 실제 정답에 얼마나 부합하는지를 보고싶을 때는 recall이 중요한 measure가 된다.
    - `F-measure` : `Precision`과 `Recall`을 모두 고려한 성능 지표이다.
      - 평균을 내는 방법은 크게 `산술`, `기하`, `조화` 평균이 존재한다. `산술평균`은 값을 더하여 개수로 나누는 방법, `기하평균`은 값을 곱하여 `1/개수` 제곱을 해주는 방법, 그리고 `조화평균`은 각 값의 `역수`끼리 더한후 이를 개수로 나눈 것을 다시 `역수`취해주는 방법이다.
      - 동일한 값들에 대하여, `산술 >= 기하 >= 조화` 의 크기를 갖는다.
      - 즉, 조화평균으로 갈수록, 보다 낮은 값에 대해 더 큰  `가중치`를 부여하는 평균 방법인 셈이다.
      - `F-measure`는 `precision`과 `recall`의 조화평균이다. 즉, 둘 중 더 작은 값에 치중된 평균인 셈이다.
    - `precision`, `recall`, 그리고 `F-measure`는 모두 문장의 `순서`를 고려하지 못한다는 문제점을 갖는다. 예를 들어,

    <p align='center' style='font-weight:bold'>prediction2 : Havana na in heart my is Half ooh of na</p>

    ​	라는 문장이 있다고 해보자.  이 경우 `precision` = 100%, `recall` = 100%, `F-measure` = 100%로 모든 문장을 맞췄다고 말하지만, 위 문장은 타겟 문장에 있는 단어들만 들어있을 뿐 순서는 뒤죽박죽이다. 

    - 이를 개선하기 위해 `BLEU` 가 나왔다.

  #### BLEU Score(Bilingual Evaluation Understudy)

  - `BLEU`의 특징

    - ① `N-gram Overlap` : 연속된 2, 3 ,.. n개의 단어를 함께 보았을 때의 문구가 ground truth 문장과 얼마나 일치하는지를 계산하는 방법을 도입했다. 이는 문장의 순서를 고려할 수 있는 방법이다.
    - ② `Precision` 만을 고려한다. 왜냐? 꼭 ground truth 문장에 부합해야지만 좋은 prediction인 것은 아니기 때문이다. 
      - 'I love this movie very very much.'라는 ground truth 문장이 있고, 'I love this movie so much.'라는 prediction 문장이 있다고 해보자.
      - 두 문장은 의미론상 거의 동일한 문장이다. 
      - 이에 대한 recall score는 5/7이고, precision score는 5/6 이다. 즉, precision score가 우리의 생각과 보다 부합하는 결과를 내놓는 것을 알 수 있다.

  - `BLEU`의 계산 방법

    - ① `1~4`gram을 사용한 경우의 `precision`을 각각 계산한다.
    - ② `Brevity Penalty`를 준다. 이는, 타겟 문장보다 짧은 문장을 생성함으로써 받는 penalty를 의미한다.
    - 구체적으로 이를 수식으로 나타내면,

    $$
    BLEU = min(1,\frac{length\ of\ prediction}{length\ of\ reference})(\Pi_{i=1}^{4}precision_i)^{1/4}
    $$

    가 된다.

    - prediction과 prediction2 문장의 BLEU score를 각각 구해보면,
      - Prediction의 경우
        - `1-gram precision` : 7/9
        - `2-gram precision` : 4/8
        - `3-gram precision` : 2/7
        - `4-gram precision` : 1/6
        - `brevity penalty` : 9/10
        - `BLEU Score` : 52%
      - Prediction2의 경우
        - `1-gram precision` : 10/10
        - `2-gram precision` : 0/9
        - `3-gram precision` : 0/8
        - `4-gram precision` : 0/7
        - `brevity penalty` : 10/10
        - `BLEU Score` : 0%
    - 아까 precision, recall 및 F-measure에서 고려하지 못한 문장의 순서가 고려되었다는 것을 확인할 수 있다.