---
title : 색인순차검색
categories: [프로그래밍]
math: true
---

### 색인 순차 검색

```python
def sequential_search(a, key):
    result = list()
    for i in range(0, len(a)):
        if a[i] == key:
            result.append(i)
    return result

def index_sequential_search(array, index_table_size, search_key):
    # index table 생성
    interval = int(round(len(array) / index_table_size, 0)) # 간격
    index_table = dict()
    cnt = 0
    while len(index_table) != index_table_size: # index_table의 길이가 index_table_size와 
        index_table[cnt] = array[cnt]
        cnt += interval
        
    # 인덱스 먼저 찾기
    index_start = 0 # index_start 변수 초기화
    index_end = 0 # index_end 변수 초기화
    last_index = 1 # 마지막 인덱스까지 갔는지를 확인하기 위한 변수
    
    idx_list = list(index_table.keys()) # index_table의 index들을 리스트화
    
    for i in range(len(idx_list)-1):
        if index_table[idx_list[0]] > search_key: # 맨 처음 인덱스보다 작은 값을 찾으려는 경우
            return []
        
        # 키 값들 사이에 찾으려는 값이 있는 경우
        elif index_table[idx_list[i]]<= search_key and index_table[idx_list[i+1]] > search_key: 
            index_start = idx_list[i]
            index_end = idx_list[i+1]
            last_index = 0
            break
                    
    if last_index == 1: # 만약 마지막 인덱스보다 큰 값을 찾으려는 경우
        index_start = idx_list[-1] # index_start는 마지막 인덱스
        res = sequential_search(array[index_start:], search_key)
        if res == []: # 만약 순차검색으로 안찾아진 경우
            return res
        else:
            return [index_start+res[0]]
    else: 
        res = sequential_search(array[index_start:index_end], search_key)
        if res == []:
            return res
        else:
            return [index_start+res[0]]
```

