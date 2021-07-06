---
title: nn.CrossEntropyLoss 정리
comments: true
---

### parameters - 보통 안 건드린다.

- `weight` : 각 클래스 별로 수동적으로 weight 값을 조정해주는 것이다. 따라서 클래스 개수만큼의 Tensor가 있어야 한다.
- `ignore_index` : 
- `reduction` : `output` 값을 어떤식으로 reduce 할 것인지. `none`, `mean`, `sum`이 있다. 기본 값은 `mean`(weighted mean of the output)이며, `none`으로 하는 경우  reduction이 일어나지 않는다.

### input (y_pred값 : 예측값)

- `(N, C)` : N = 배치 사이즈, C = 클래스의 개수(MNIST의 경우엔 10).
- 즉, 각 data point 별 `softmax` 값(그냥 `linear regression` 값이 들어갈 수도 있다.)이 그대로 input으로 들어간다.
- (batch_size = 1을 가정하여) 예를 들어, [[0.1, 0.1, 0.1, 0.4, 0.0, 0.1, 0.1, 0.1, 0.0, 0.0]] 이런식으로 들어간다.

### target (y_target값 : 실제값)

- `(N)` : target 값은 배치사이즈 개수만큼의 라벨 인덱스 값으로 들어간다.
- 만약, 배치사이즈가 10개 이고 정답이 `0, 1, 2, 3, 4, 5, 6, 7, 8, 9`라면, `CrossEntropyLoss`에 들어가는 target 값은 tensor([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]) 이다.

### output(결과값)

- 1개의 `Scalar` 값으로 나온다. (만약 `reduction`이 `none`인 경우는 `N`개 나온다.)
- 이 값이 `loss`값이다.