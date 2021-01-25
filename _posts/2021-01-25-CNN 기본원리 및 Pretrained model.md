---
title: CNN - 기본원리 및 Pre-trained model
categories: [deeplearning]
math: true
comments: true
---

\- 이 강의정리본은 연세대학교 이상엽 교수님의 특강을 정리한 것임을 밝힙니다. \-

<p align="center" style="font-size : 30px; font-weight: bold;"><u>CNN 특강 - 원리 복습 및 pre-trained model</u></p>

### CNN의 원리 복습

- `컬러`, `흑백` 이미지 : 각각 3가지(RGB), 1가지의 색상정보를 갖고 있음. 따라서 컬러 이미지는 3차원 형태의 matrix로, 흑백 이미지는 1차원 형태로 표현된다.

#### CNN의 작동 원리 : 흑백 이미지에 적용하는 경우

< CNN 작동원리 그림>

CNN의 작동 원리는 간단하게 표현하자면, ① n * m 픽셀의 이미지에 `Filter`(혹은 `Kernel`)을 적용시켜, ② `합성곱`(Convolution)을 시행하고, ③ 이를 `내적`한 Scalar 값을 가지고 새로운 `matrix`를 만든 다음 ④ 활성화 함수(일반적으로 CNN에서는 `relu`)를 적용시켜 ⑤ `activation map`을 만들고, ⑤ `Pooling`을 하며 유효한 정보(일종의 엑기스!)를 뽑아낸다. 이 때 `activation map`은 또 하나의 이미지로 간주할 수 있기 때문에, 위의 과정을 반복하면서 layer를 더 깊게 쌓을 수 있다.  최종적으로 나온 `activation map`을 `Flatten`하고, 이를 `Dense layer`를 거쳐 `softmax` 함수를 통과시키면 최종적으로 이미지 분류를 하게 되는 방식이다.

이제 각 과정에 대해서 자세하게 알아보도록 하겠다.

#### Filter(Kernel) 와 Stride

<그림>

- `Filter(Kernel)` : 이미지의 특징을 찾아내기 위한 `파라미터` 역할. CNN에서 Filter와 Kernel은 동일한 의미이다.
- 일반적으로 `Filter`는 정사각행렬로 정의된다. (`2x2`, `3x3`, `4x4` 등)
- `Filter`는 `F x F x D `의 형식으로 이루어져 있는데, `F`는 `Filter`의 크기를 의미하며 `D`는 depth의 약자로 `Channel`의 크기를 의미한다. 따라서 컬러이미지라면 `D` = 3일 것이고, 흑백이미지라면 `D` = 1일 것이다. 이 `D` 값은 자동적으로 정해지므로 사용자가 정해야할 필요가 없다.
- `Filter`의 적용
  - `Filter`는 대상 이미지의 왼쪽 상단에서부터 `stride`의 크기만큼 오른쪽 및 아래로 이동하면서 적용된다.
  - `stride` : `Filter`가 이동하는 스텝이라고 생각하면 된다. `stride`가 2인 경우 두 칸씩 건너뛰며 `Filter`가 이동하며, 오른쪽 끝까지 가면 아래로도 2칸을 내려가서 다시 오른쪽으로 두 칸씩 건너뛰며 진행한다.



#### Convolution(합성곱)

- 이름만 어렵다.(그리고 실제로도 겁나 복잡해보이긴 한다.. 하지만) 합성곱은 `Filter`의 각 원소와 `Filter`가 위치한 대상 이미지의 원소 간의 곱이다.

<그림>

-  `Convolution`을 수행하면 `Filter`의 각 자리에 원소들이 곱해져서 생기게 된다. 예를 들어 `Filter`가 3 x 3 이었다면, 3 x 3 크기의 합성곱 결과들이 생겨난다.
- 이 때, 이들을 더해준 Scalar 값이 `Filter` 하나와 대상 이미지의 해당 구간이 생성해내는 Convolved Feature가 된다.
- `Filter`를 대상이미지 전체에 대해 적용하면 하나의 `map`이 형성된다. 이 `map`은 활성화함수를 통과하게 되는데, 일반적으로 CNN에서 활성화 함수로는 `ReLU`를 사용한다.
- `ReLU`를 통과하여 최종적으로 나온 `map`을 가리켜 `Feature map`, `Activation map`, 혹은 `Response map`이라고 부른다. 이 셋은 다 같은 의미이다.



#### Activation map

- 합성곱 → `ReLU`를 통해 이미지의 특징을 나타내기 위해 생성된 새로운 이미지(?). 

- 그럼 `Activation map`의 크기는 어떻게 알 수 있을까?
  - (`N` - `F`) / `stride` + 1 
  - `N` 은 대상 이미지가 정사각형의 이미지라고 가정했을 때 한 변의 길이, `F`는 `Filter` 한 변의 길이를 의미한다.
  - 만약 32 * 32 이미지에 4 * 4 `Filter`를 적용하고, `stride`가 2라면 (32 - 4) / 2 + 1 = 15가 된다. 즉 `activation map`의 크기는 15 * 15가 되는 것이다.

- `Activation map`은 또다른 이미지로 간주될 수 있기 때문에, 위의 과정을 반복할 수 있다. 그러다보면 layer가 깊어지겠지 ?! 

- layer가 깊어질수록, 즉 출력층(output)에 가까운 layer일수록 parameter들은 우리가 **수행하고자 하는 task**와 더 직접적으로 연관성을 갖고 있을 것이다.
  - 즉, `task specific`한 parameter들일 것이다.
- 반면, 입력층(input)에 가까운 layer들의 parameter들은 **이미지의 일반적인 특징**을 포함하는 parameter들일 것이다.

- 이것이 우리에게 시사하는 바는 무엇일까?
  - 만약, 우리가 pre-trained model을 사용하고자 할 때, 우리의 task가 이 pre-trained model의 task와 유사하다면 출력층에 가까운 parameters까지 이용해도 좋다.
  - 하지만, 우리의 task가 다른 성격의 task라면, 우리는 이미지의 일반적 특징만을 아주 잘 추출할 수 있는 parameters들만 가져와서 사용해야 한다. 즉, 입력층에 가까운 layer들만을 이용해야 함을 의미한다.



#### Filter 심화

- 대상 이미지가 Color 이미지인 경우, `Channel` 은 RGB로 총 3개를 갖게 된다. 만약 여기에 `Filter`를 적용하게 되면 → 생성되는 `Activation map`은  몇 개일까?
- `1`개이다. 아까도 언급했지만, `Filter`의 `D`는 자동적으로 3으로 정해질 것이다. 따라서 `F * F * 3`의 `Filter`와 대상 이미지의 합성곱-내적을 통해 나오는 값은 하나이다.
- 다만, 동일한 이미지에 여러 개의 `Filter`를 적용하게 된다면 `Activation map`을 여러개 만들 수 있다. 만약 `Filter`를 3개 적용시킨다면 3개의 `Activation map`이 나올 것이고 이들을 3의 `depth`로 쌓을 수 있다. 
  - 즉, 원래의 이미지 shape(N * M * 3)을 따라갈 수 있는 것이다.
  - 근데 더 쌓을 수도 있다. `Filter` 32개 적용하면 더 뚱뚱하게 쌓을 수 있다.
  - 이렇게 `Activation map`의 을 또다른 이미지로 간주할 수 있다.
  - 결국 `Activation map`의 `depth` = `Filter`의 개수 !!!!



#### Padding

- 테두리를 특정한 값으로 채워주는 기능을 한다.
- Why?
  - 이미지 가장자리의 정보가 중요한 경우에, 가장자리의 정보 손실을 방지하기 위해!
  - 만약 `7 * 7 * 3`의 이미지에 `3 * 3 * 3` 필터를 `stride`=3으로 적용한다고 해보자. 
  - <그림>
  - 필터는 첫 번째 라인에서 두 번 적용될 것이고, 7 % 3 = 1이 남게 된다. 그러면 이 남는 가장자리는 어떻게 되냐고 ?! → 가차없이 버려진다. 
  - 근데 이 가생이에 만약에 2를 더해주는 `padding`이 일어난다면 `1+2=3`이 되어 필터가 한 번 더 적용될 수 있고, 이 경우 가장자리 정보의 손실을 방지할 수 있다. 뭔 말인지 느낌이 오는가?
- `Padding`은 기본적으로 `zero-padding`을 쓴다.
  - `keras`에서는 생성자에 `padding` 인자가 있는데, 이 때 이 값을 `valid`로 하면 패딩을 적용하지 않겠다는 의미이며, `same`으로 하면 `zero-padding`을 수행한다.
- 결국 `Padding`은 가장자리의 정보가 우리가 수행해야하는 task에 있어 중요한 경우 꼭 해주어야 하는 중요한 작업이다.



#### Pooling

<그림>

- 합성곱 이후 이미지의 **엑기스**를 뽑아내는 과정.
- 꼭 해야되는 건 아니라고 한다. 그래도 엑기스를 뽑아내면 이미지의 특징을 더 잘 반영하니까, 거의 항상 하는 것 같다.
- Pooling size 안에 들어오는 원소들을 가지고 Pooling 작업을 하여 이미지의 크기를 줄이면서 동시에 필요한 정보만을 뽑아내는 과정으로 이해하면 될 것 같다. 
  - `Pooling`을 위해서도 별도의 `Filter`가 필요하다. 그러나 해당 `Filter`는 parameter가 존재하지 않는다 → 즉 크기만 존재한다. 이 `Filter`의 size를 Pooling size라고 한다.
  - 이 `Filter` 역시 왼쪽 상단에서부터 오른쪽/아래로 이동하면서 `Pooling`을 수행한다.
  - Pooling size는 보통 `stride`의 크기와 같게 설정한다.
- `Pooling`의 종류
  - `Max Pooling` : Pooling size 내에 있는 원소들 중 가장 큰 원소만을 뽑아내는 Pooling 방식이다. 
  - `Average Pooling` : Pooling size 내에 있는 원소들의 평균값을 뽑아내는 방식이다.
- 여기서 잠깐!
  - 이미지를 나타내는 값은 0~255로 표현되는데, `0`에 가까울수록 `검정색(어두운 색)`을, `255`에 가까울수록 `흰색(밝은 색)`을 나타낸다.
  - 즉, `Max Pooling`을 하게 되면 해당 `Filter` 내에 가장 밝은 색을 추출한다는 의미가 된다.
  - 실제로 이미지 관련 task 들에서, **이미지들의 밝은 부분이 이미지의 특성을 두드러지게 나타낸다고 한다.** → 그렇기 때문에 `Max Pooling`이 효과적인 것이다.
  - 만약, 반대로 어두운색이 이미지의 특성을 두드러지게 나타낸다면 `Min Pooling`이 보다 효과적일 것이다. 그러나 실제로 그런 경우는 거의 없고, 따라서 잘 쓰이지 않는다고 한다.



#### Flatten & Dense Layer → Softmax

- 마지막 단에 나오는 `Activation map`은 대부분의 경우 depth > 1 일 것이다. 이 정보를 통해 이미지를 최종적으로 분류하기 위해서는 평탄화 작업이 필요한데, 이를 `Flatten`이라 한다.
- `Flatten`은 단순히 `depth`가 1을 초과하는 이미지들의 벡터를 `depth`=1로 나란히 연결시켜주는 것이다. 이렇게 해서 나란히 연결된 layer를 `Fully-Connected Layer(FC)`라고 한다.
- `FC`는 `가로x세로x채널`만큼의 shape을 가지고, 이것이 `Dense` layer를 거치게 되면, `FC`의 shape * `Dense` layer의 노드 수 만큼의 파라미터가 생긴다. 진짜 엄청나게 생겨버린다. 이 파라미터 수를 좀 줄여줘야한다고 한다.
- <model summary 그림>



#### model summary 해석

- 우리가 만들어낸 모델의 summary는 `pip install pydot`을 통해 `pydot` 라이브러리를 설치하고 이를 통해 시각적으로 확인해볼 수 있다.

