---
title: nn.Embedding 정리
comments: true
---

```python
torch.nn.Embedding(num_embeddings: int, 
                   embedding_dim: int, 
                   padding_idx: Optional[int] = None, 
                   max_norm: Optional[float] = None, 
                   norm_type: float = 2.0, 
                   scale_grad_by_freq: bool = False, 
                   sparse: bool = False, 
                   _weight: Optional[torch.Tensor] = None)
```

### parameters

- `num_embeddings` : vocab_size를 의미함. data에 사용된 unique한 단어의 개수
- `embedding_dim` : 몇 차원으로 embedding할 것인지를 정한다.
- `padding_idx` :  index 값을 지정해주면, 해당 index

### Shape

- input : `(*)`, 즉 어떤 shape이든지 okay
- output : `(*, H)`, H = `embedding_dim`이므로, 기존 input의 차원에 `H` 값을 갖는 차원을 하나 추가한 output이 나옴.

