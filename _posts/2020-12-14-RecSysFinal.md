---
title : 추천시스템 기말 정리
categories: [추천시스템]
comments: true

---

## Week 9 (Surprise)

---

## Week 10 (Binary Data & Keras 로 MF 구현)

### Binary data

- Click, like, read 등의 행동 데이터

- 주로 Implicit data

- Binary data로 순서를 예측하는 경우

  - Binary data가 나타내는 행동으로부터 다음 행동을 예측(Markov Model)
  - 예) A, B, C 제품을 클릭한 사용자가 그 다음으로 클릭할 제품은?

- Binary data를 선호도로 변환하는 경우

  - Binary data는 선호도를 반영한다고 보고 적절한 가중치를 주어 rating으로 사용
  - 예) 클릭은 1, 북마크는 2 등의 점수를 부여하여 각 아이템에 대한 합을 아이템에 대한 선호도로 사용.

  

### Markov Model(MM)

- 방문한 페이지의 선후관계를 분석
  - A, B, C를 열어본 사용자는 D를 클릭할 가능성이 가장 높다.
- 페이지 간의 상호 의존성을 분석
- All Kth-Order Markov Model
  - Coverage와 recall을 높이기 위한 방법
  - 가장 높은 순서부터 차례로 적용

![image](https://user-images.githubusercontent.com/37925813/102020280-7d949400-3dbb-11eb-874a-d97107ce6764.png)



### Binary Data 순서 예측의 한계

- 데이터 크기가 큰 경우가 많다(계산 속도)
- 클릭의 경우 페이지 디자인에 제한 받으며, 페이지 디자인은 자주 바뀐다
- Binary data는 noise가 많다
- 클릭의 예측으로 할 수 있는 일에 제한이 많다.
  - 구현가능성 : 실시간으로 페이지나 링크의 레이아웃을 바꿀 수 있어야 한다.
  - 효용성 : 사용자가 실시간 페이지 레이아웃에 얼마나 가치를 부여할까?

---

## Week 11 (Deep Learning 기반 추천)

### MF와 DL의 차이점

- MF는 기본적으로 linear model로서 linear pattern을 학습
- DL은 non-linear pattern도 학습 가능
- 어떤 경우에 DL이 나을까?
- DL을 개선하기 위한 방법은? - 다양한 layer 및 모델 시도
  - MF처럼 bias term을 넣으면? (w11-DL2.py)
  - 추가적인 user 변수(나이, 지역)와 item 변수(장르, 개봉일 등)을 넣으면?(DL3.py)

---

## Week 13 (Hybrid / Context-aware 추천)

### Hybrid 추천 시스템

- 최근, 두 가지 이상의 필터링 기술을 혼합하여 사용하는 hybrid 형태가 많음.

  - 복수 기술을 사용할 때 정확도 향상
  - 아마존 → 협업필터링이 주가 되지만, 다양한 기술이 함께 사용됨.

- Hybrid Recommender의 형태

  - 복수의 추천 엔진(MF와 DL)의 **예상 rating을 합치는 것.** (hybrid1 , 2 .py)

  - 복수의 추천 엔진의 **추천 결과(List)를 합치는 것.**

    - 목표 : 협업 필터링 기술을 포함시킨 content-based model 개발

      - 1) movie title과 user 데이터 불러오기

      - 2) Content Based model을 활용하여 25개의 가장 비슷한 영화 추출

      - 3) 협업 필터링(CF) 사용하여 유저가 위에서 추출한 25개의 영화에 줄 Predicted rating 계산

      - 4) Predicted rating이 가장 높은 영화 10개 추천

        

### Context-aware 추천

- 추천 시스템의 정확성은 다양한 상황(context)에 영향을 받음.
  - 가족과 함께, 친구와 함께?
  - 오전 혹은 저녁?
  - 주중 혹은 주말?
  - 날씨
  - 현재 검색하고 있는 도메인
  - 이러한 상황들을 어떻게 반영할 것인가??
- 상황변수를 반영하는 방법
  - ① 개별 추천엔진의 입력변수로 사용? → 정확하지 않다. 
    - **why?**
  - ② 개별 추천엔진을 결합하는 데 입력으로 사용

---

## Week 14 (Scale-up & Personalized Neighbor size & Gray Sheep Problem)

### Scale-Up

- Sparse matrix : SciPy 이용
  - 장점 
    - 데이터가 sparse한 경우 데이터를 압축해서 처리하므로 메모리 소비가 적다.
    - 대부분의 일반적인 연산(indexing, +-*, T, dot)이 NumPy syntax와 동일하게 구현되어 있다.
    - 데이터의 access에 추가적인 overhead cost(시간)이 들어가지만 -> 유일한 단점, element 수가 적은만큼 효율성이 높다.
  - Sparse Matrix의 종류
    - csc_matrix : Compressed Sparse **Column** Format - efficient column slicing
    - csr_matrix : Compressed Sparse **Row** Format - efficient row slicing
    - bsr(block sparse row), lil(list of lists), dok(dictionary of keys), coo(triplet format), dia(diagonal)



### Personalized Neighbor Size

- KNN에서 neighbor size는 전체적으로 하나를 사용하는 것이 보통.
- 개인별로 neighbor size가 다를 수 있다.
  - 만약 neighbor size도 개인별로 차별화 한다면?
    - 개인별로 최적인 neighbor size를 simulation을 통해 찾는다.
    - 이를 위해 training set을 sub-training / sub-validation set으로 분리
    - item을 바꿔가면서 개인별 최적의 neighbor size를 구한다.
  - netflix data를 사용해서 simulation
    - data를 5000명씩 sampling해서 계산함.
    - personal neighbor size와 significance weighting을 같이 사용함.

### Gray Sheep Problem

- gray sheep : 취향이 독특해서 다른 사용자와는 매우 다른 평가 경향을 보이는 사용자. (outlier)
  - 평가 패턴에 노이즈를 증가시켜서 정확도를 낮추는 경향이 있음.
  - gray sheep을 분리하면 정확도가 향상됨.
  - gray sheep을 분리하는 방법?
    - correlation, cosine 등의 유사도가 기준 이상 되는 사용자가 없는 gray sheep을 분리.
    - 평가 패턴을 network analysis 등을 통해 gray sheep 찾아 냄.
    - 그 외에 다양한 방법이 가능.
      - SNA(Social Network Analysis)를 사용하는 방법
        - 사용자-영화 2원 연결망을 사용자-사용자 1원 연결망으로 변환
        - 연결정도 중앙성(degree centrality)이 작은 사용자를 찾아내서 분리함. (분리의 기준이 되는 최적의 degree centrality는 simulations를 통해 결정)
        - gray sheep과 나머지 사용자에 대해 추천 알고리즘을 각각 적용

![image](https://user-images.githubusercontent.com/37925813/102020306-c0ef0280-3dbb-11eb-8a5d-02c8cea1f6e8.png)

- 사용자별 공통으로 본 영화 개수를 weight로 하여 edge를 연결

- Explanations in Recommender Systems

  - 추천시스템에 설명이 추가되면 어떻게 될까?

    - Herlocker et al. → 21가지 설명 방식을 비교.

    - 만족도와 설득력 등을 조사함.

      - 사용자들이 가장 만족하는 설명 방식은 **막대그래프.**
      - **부정확한 설명**은 역효과를 가져옴.
      - **Too Much Information**도 부정적인 효과를 가져옴.

    - ex) Content-Based의 경우

      - '귀하가 00를 좋아했기 때문에' → '비슷한 아이템으로는 00가 있습니다.'

      - 변형

        - 키워드 방식으로 설명 → 효과적인 것으로 알려짐.

          ![image](https://user-images.githubusercontent.com/37925813/102020318-d106e200-3dbb-11eb-8e60-ea47d3c9b617.png)

        - Tag 로 설명. 일종의 워드클라우드 형식

          ![image](https://user-images.githubusercontent.com/37925813/102020321-debc6780-3dbb-11eb-8fca-4b43fc08da9a.png)