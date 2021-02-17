---
title: Week4_1 Intro to NLP
categories: [boostcamp]
tags: [부스트캠프, NLP]
comments: true
---

\- 이 강의정리본은 KAIST 주재걸 교수님의 강의를 정리한 것임을 밝힙니다.\-

자연어처리(Natural Language Processing)은 크게 NLU(Natural Language Understanding)과 NLG(Natural Language Generating)으로 나누어볼 수 있다. 전자는 자연어를 이해하는 것, 후자는 자연어를 생성하는 것을 의미한다. 자연어처리 분야는 컴퓨터비전 분야와 함께 가장 활발하게 연구되고 있는 분야이다.

자연어와 관련된 분야는 크게 `NLP`, `Text Mining`, `Information Retrieval`로 이루어져 있는데, 각 분야에서 어떤 task가 있는지 간단하게만 살펴보자.

### NLP

- 자연어를 다루는 주요 분야 : [ACL](https://www.aclweb.org/portal/)(Association for Computational Linguistics), [EMNLP](https://2021.emnlp.org/)(Empirical Methods in Natural Language Processing), [NAACL](https://naacl.org/)(The North American Chapter of the Association for Computational Linguistics)
- 자연어 처리를 이루는 주요 레벨별 task들
  - Low-Level Parsing
    - `Tokenization` : "I study math."라는 문장을 "I", "study", "math" 등 컴퓨터가 이해할 수 있는 최소 단위인 `token`으로 쪼개는 과정.
    - `Stemming` : 'Study', 'Studying', 'Studied' 등 같은 의미를 갖는 단어들도 어미에 따라 변화가 다양하다. 특히 한국어에서는 그 변화가 더 심한데, '맑다', '맑고', '맑지만' 등 다양한 어미의 변화가 있지만 컴퓨터는 이 모든 단어들을 동일한 의미로 인식해야한다. 따라서 `어근`의 추출이 필요한데, 이를 추출하는 과정을 `stemming`이라 한다.
  - Word & Phrase Level
    - `NER`(Named Entiy Recognition) : 여러 단어로 이루어진 고유명사를 인식하는 작업. 
      - 예를 들어, `New York Times`는 하나의 고유명사로 인식되어야지, 3개의 다른 단어로 인식되면 안된다. 이를 하나의 고유명사로 인식하게 해주는 작업이 NER이다.
    - `POS Tagging` : Part-Of-Speech 태깅. 문장 내 단어들이 어떤 형태로 쓰이는지를 태깅하는 작업이다. 
      - 예를 들어, "나는 밥을 먹었다." 라는 문장에서 `주어`에 `나` 가 태깅되고, `목적어`에 `밥`이 태깅되며, `동사`에 `먹었다`가 태깅되는 작업이 pos tagging이다.
  - Sentence Level
    - `Sentiment Analysis` : 감성분석. 주어진 문장을 바탕으로 해당 문장의 감정이 긍정인지, 부정인지를 가려내는 작업이다. 
      - 예를 들어, '그 영화는 정말 재밌었어' 는 긍정(`1`)으로, '그 영화 더럽게 재미없더라'는 부정(`0`)으로 분류해내는 작업이다.
      - 꼭 0과 1의 binary classification이 아니라도, 어느 정도의 감성점수를 갖는지를 0과 1사이의 continuous 값으로 나타낼 수도 있다.
    - `Machine Translation` : 기계번역. 말 그대로 한 언어로 되어 있는 문장을 다른 언어로 번역하는 작업이다.
  - Multi-Sentence & Paragraph Level
    - `Entailment prediction` : 두 문장 간 논리적인 관계를 예측하는 작업이다. 내포, 모순 등을 예측한다.
      - 예를 들어, 'John은 어제 결혼을 했다.' 와 '어제 최소한 한 명이 결혼을 했다.', 그리고 '어제 아무도 결혼하지 않았다.'라는 문장이 각각 있다고 하자.
      - 문장 1이 참인 경우 문장 2는 자동적으로 참이 된다.
      - 또한 문장 1이 참인 경우 문장 3은 논리적인 모순 관계를 갖게 된다.
      - 이러한 문장 간 관계를 예측하는 것이 entailment prediction이다.
    - `독해 기반의 질의응답(QnA)` : 말 그대로, 자연어로 된 어떠한 질문을 했을 때, 이에 대한 답을 내는 자연어 처리 task를 의미한다.
      - 구글에 , 'Where did Napoleon die'라고 검색해보면, 단순히 검색결과가 뜨는 것이 아닌, 실제로 나폴레옹이 어디서 죽었는지에 대한 대답을 내놓는 것을 볼 수 있다. 이것이 자연어 기반의 QnA 태스크이다.
    - `Dialog systems` : 챗봇을 생각하면 된다. 대화가 가능한 시스템을 개발하는 task를 의미한다.
    - `Summarization` : 주어진 document를 한 줄 요약의 형태로 summarize 하는 task이다.

### Text Mining

- NLP의 한 부분 같지만, Text Mining은 기술적인 부분보다는 대용량 텍스트 데이터의 처리, 빈도 기반, 통계적 기반의 언어 처리를 통해 insight를 발견하는 쪽에 보다 연관성이 높은 분야이다.
  - 예를 들어, 최근 1년간의 수백만건의 document들을 분석하여 높은 빈도로 출현하는 특정 단어들을 보는 작업같은 것은 Text Mining의 영역이다.
  - 또한, 리뷰 데이터들을 분석하여 소비자의 반응을 살피는 것 역시 Text Mining의 영역이다.
- 텍스트 마이닝을 다루는 주요분야: [KDD](https://www.kdd.org/)(Knowledge Discovery and Data Mining), [The Web Conference](https://www2021.thewebconf.org/), [CIKM](https://dl.acm.org/conference/cikm)(Conference on Information and Knowledge Management), [ICWSM](https://www.icwsm.org/2021/index.html)(International Conference on Web and Social Media)
- `Document Clustering` : 문서군집화. 대표적인 예로 `Topic Modeling`이 있다. 비슷한 의미를 갖는, 혹은 같은 주제를 내포하는 단어들을 군집화하는 작업을 의미한다.
  - 예를 들어, 아이폰12에 대한 리뷰데이터를 군집화한다고 해보자. 다양한 리뷰데이터들을 추출하여 이들을 Topic Modeling하게 되면, (토픽 군집이 잘 잡혔다는 가정하에) `배터리 이슈`, `그립감`, `AS이슈`, `디자인` 등을 나타내는 토픽군집들이 잡힐 수 있다.
- `Computational Social Science` : `Twitter`나 `Facebook` 등 다양한 Social Network Service 등에서 데이터를 추출 및 분석하여 이를 사회현상과 관련지어 분석하는 것을 의미한다.
  - 예를 들어, 2020년에는 twitter에서 `COVID-19` 관련 트윗을 추출하여, 해당 데이터들을 분석하여 `코로나 블루`가 실제로 우리 사회에서 일어나고 있는 현상인지를 분석하는 작업이 이에 속한다고 볼 수 있을 것 같다.

### Information Retrieval

- 구글, 네이버같은 주요 포털 사이트에서 사용하는 검색기술을 연구하는 분야이다. 이 분야는 어느 정도 연구가 성숙한 상태에 이르렀기 때문에 활발한 연구가 이루어지지는 않는다. 하지만, `Recommendation System`이 이 분야의 주요 화두로 남아있다.

- 포털들은 이제 사용자가 주로 검색하는 키워드들을 통해, 해당 유저가 관심을 가질만한 키워드나 상품 등을 자동적으로 추천해주는 기술을 개발하고 있다.
- 이를 통해 사용자가 추가적으로 스스로 검색하는 수고를 덜어주기도 하고, 상품을 추천하여 구매를 유도하는 등의 방향으로 나아가고 있다. 대표적으로 `Netflix`의 추천시스템이 가장 유명하다. 