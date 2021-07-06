---
title : Dacon 심리성향 예측 AI 경진대회 진척사항
comments: true
---

## Dacon 대회 : 심리 성향 예측 AI 경진대회(월간 데이콘 8)

---

### 1. ROC_AUC_SCORE

---

roc_auc_score 이 점수 기준. roc_auc_score를 정확하게 내려면 predict 값이 아닌, predict_proba 값을 써서 인자로 넣어주어야 함. sklearn에 make_scorer라는 모듈이 있는데, 인자로 needs_proba = True를 주면, predict_proba 값을 이용하여 score 계산을 해준다.

```python
from sklearn.metrics import roc_auc_score, make_scorer
from sklearn.model_selection import cross_validate

scoring = {'roc_auc_score': make_scorer(roc_auc_score, needs_proba=True)}
result = cross_validate(model, X, y, cv=5, scoring=scoring)
auc_score = result['test_roc_auc_score'].mean()
print(auc_score)
```



### 2. pickle 이용하여 model 저장하기

---

```python
model = 모델

def save_model(model, num):
    name = 'model_'+str(num)
    with open('./model/{}.pkl'.format(name), 'wb') as f:
        pkl.dump(model, f)
      
save_model(model, file_number)
```



### 3.  분석 pipeline 만들기

---

모든 과정을 함수화 하여 한번에 실행될 수 있도록 파이프라인을 구축해보았다.

```python
                        ######### train/test에 사용하는 전체 컬럼 #########
using_features = ['QaE', 'QbE', 'QcE', 'QdE', 'QeE', 'QfE', 'QgE', 'QhE', 'QiE', 'QjE', 'QkE', 'QlE',\
                    'QmE', 'QnE', 'QoE', 'QpE', 'QqE', 'QrE', 'QsE', 'QtE',\
                    'tp01', 'tp02', 'tp03', 'tp04', 'tp05', 'tp06', 'tp07', 'tp08', 'tp09','tp10',\
                    'age_group', 'education','gender','married', 'race', 'religion', 'urban', 							'voted']

                        ######### GET_DUMMIES 해야하는 컬럼 #########
cat_col = ['gender', 'age_group','education', 'married', 'race', 'religion', 'urban']

                        ######### 수치형 컬럼 #########
numeric = ['QaE', 'QbE', 'QcE', 'QdE', 'QeE', 'QfE', 'QgE', 'QhE', 'QiE', 'QjE',\
           'QkE', 'QlE', 'QmE', 'QnE', 'QoE', 'QpE', 'QqE', 'QrE', 'QsE', 'QtE',\
           'tp01', 'tp02', 'tp03', 'tp04', 'tp05', 'tp06', 'tp07', 'tp08', 'tp09','tp10']

def preprocessing():
    # QxE -> 이상치 제거
    print('이상치 제거 전:', train.shape)
    for i, q in enumerate(train.columns[:40]):
        if i % 2 == 1:
            train_data = train[train[q] < 1000000]
    

    # ms(밀리세컨드) -> log화
    for i, q in enumerate(train_data.columns[:40]):
        if i % 2 == 1:
            train_data[q] = np.log1p(train_data[q])
            
    # familysize 이상치 제거
    train_data = train_data[train_data['familysize'] < 30]
    print('이상치 제거 후:',train_data.shape)
    return train_data

def feature_extraction(train_data):
    processed_train = train_data[using_features]
    train_hot = pd.get_dummies(data=processed_train, columns=cat_col, drop_first=True)
    return train_hot

def scaling(train_hot):
    voted = train_hot['voted']
    train_hot.drop(columns=['voted'], inplace=True)
    train_hot['voted'] = voted
    ######### voted의 1(yes), 2(no) 값을 1, 0 으로 replace #########
    train_hot['voted'] = train_hot['voted'].replace([2,1],[0,1])
    scaler = MinMaxScaler()
    for col in numeric:
        train_hot[col] = scaler.fit_transform(train_hot[[col]])
    return train_hot

def auto_train(train_hot, what_to_return='best'): ## 모델들을 비교할 수 있는 대신 오래 걸림.#
    data = train_hot
    category = [] # 범주형 column
    clf = setup(data, target='voted', session_id = 4242,\
                numeric_features=numeric, train_size=0.8)
    
    if what_to_return != 'best':
        best_5 = compare_models(n_select=5, sort='AUC', include=['xgboost','lightgbm','gbc','lr','ada','lda'])
        blender = blend_models(estimator_list = best_5, method = 'soft')
        return blender
    else:
        best = compare_models(n_select=1, sort='AUC', include=['xgboost','lightgbm','gbc','lr','ada','lda'])
        return best   
    
def test_preprocessing():
    test_data = test[using_features[:-1]]
    test_data= pd.get_dummies(data=test_data, columns=cat_col) # drop_first하면 뭐가 빠질지 모르니 여기서는 하지 않는다.
    test_hot = test_data[train_hot.columns[:-1]] # voted 컬럼을 제외하고 test_hot 을 생성.
    scaler = MinMaxScaler()
    for col in numeric:
        test_hot[col] = scaler.fit_transform(test_hot[[col]])
    return test_hot

def make_answer(X, y, test, model):        
    pred = model.fit(X, y).predict_proba(test)
    answer_proba = [x[0] for x in pred]
    return answer_proba

def submit(preds, i): # 인자 : 답안(answer), 파일번호
    ex = pd.read_csv('./submission.csv')
    ex['voted'] = preds
    ex.to_csv('./submission/submission_{}.csv'.format(str(i)), index=False)
    print(i,'번째 submission 파일을 생성함.')
    
def auc_score(data, model):
    X = data.iloc[:, :-1].values
    y = data.iloc[:, -1].values
    scoring = {'roc_auc_score': make_scorer(roc_auc_score, needs_proba=True)}
    result = cross_validate(model, X, y, cv=5, scoring=scoring)
    auc_score = result['test_roc_auc_score'].mean()
    print('5 Fold Cross Validation Score is', auc_score)
    
def save_model(model, num):
    name = 'model_'+str(num)
    with open('./model/{}.pkl'.format(name), 'wb') as f:
        pkl.dump(model, f)
```

```python
file_number = 5 # 저장을 위한 파일 넘버

if __name__ == '__main__':
    train_data = preprocessing()
    train_hot = feature_extraction(train_data)
    train_hot = scaling(train_hot)
    test_hot = test_preprocessing()
    
    model = auto_train(train_hot, what_to_return='best') # 'best' : automl 모델 중 가장 성능 좋은 모델
    												 # 'ensemble' : 상위 5개 모델 soft-voting
    answer = make_answer(train_hot.iloc[:, :-1], train_hot.iloc[:, -1], test_hot, model)
    submit(answer, file_number) # submission(답안) 저장
    save_model(model, file_number) # model 저장
    auc_score(train_hot, model) # train data를 train_test_split하여 test 데이터에 대해 cv=5 스코어 내보기(제출 횟수가 제한되어 있으므로, 미리 예상점수를 확인해보는 것.)
```

