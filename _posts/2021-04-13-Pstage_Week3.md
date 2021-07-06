## KLUE - 2강 : 한국어 언어 모델 학습 및 다중 과제 튜닝 - 자연어 전처리

### 1. 자연어 전처리

자연어 전처리는, 원시 데이터를 기계 학습 모델이 학습하는 데 적합하게 만드는 프로세스

왜? Task의 성능을 가장 확실하게 올릴 수 있는 방법이기 때문.

아무리 모델을 바꾸고 튜닝을 하더라도 데이터 자체가 문제가 있다면 성능이 나올 수 없음.

### 자연어 처리의 단계

Task 설계 -> 필요데이터 수집 -> 통계학적 분석 -> 전처리 -> Tagging -> Tokenizing -> 모델 설계 -> 모델 구현 -> 성능 평가 -> 완료

악성 댓글 classifier를 만들자! -> 댓글 데이터 수집 -> token 개수를 세고 아웃라이어 제거, 빈도를 확인하여 사전(dictionary) 정의 -> 개행문자 제거, 특수문자 제거, 중복 표현 제어, 이메일/링크 제거, 제목 제거, 불용어 제거, 조사 제거 등 -> 태깅 -> 어떤 단위로 자연어를 살펴볼 것인가(어절, 형태소, wordpiece tokenizing) 

#### Python string 관련 함수

`upper()`, `lower()`, `capitalize()`, `title()`, `swapcase()`

`strip()`, `replace()`, ... , `join()`, ...

### 2. 한국어 토큰화

자연어를 어떤 단위로 살펴볼 것인지.

#### 토큰화

주어진 데이터를 token이라 불리는 단위로 나누는 작업

토큰이 되는 기준은 다를 수 있음.

문장 토큰화 : 문장을 분리

단어 토큰화



### 실제 코드를 통한 실습

#### newspaper 라이브러리

- url 정보만 입력해주면 텍스트를 추출해주는 라이브러리.

- 매우 많은 언어에 대한 크롤링을 지원해줌.

```python
!pip install newspaper3k
import newspaper
newspaper.languages() # 지원언어 확인
```

- 뉴스 크롤링

```python
from newspaper import Article
news_url = "https://www.wikitree.co.kr/articles/252931"
article = Article(news_url, language="ko")
article.download()
article.parse()

print('title:', article.title)
print('context:', article.text)
```

- html 태그 제거

```python
import re
# 실제 신문기사에 자주 들어가는 예제텍스트 추가
context = article.text.split('\n')
context.append("(서울=위키트리) 김성현 기자 (seonghkim@smilegate.com) <저작권자(c) 무단전재-재배포 금지> ‘<br>이 줄은 실제 뉴스(news,)에 포함되지 않은 임시 데이터임을 알립니다…<br>‘")
context.append("(사진=위키트리, 무단 전재-재배포 금지) ‘<br>이 줄은 실제 뉴스(news,)에 포함되지 않은 임시 데이터임을 알립니다…<br>‘")
context.append("#이세돌 #알파고 #인공지능 #딥러닝 #바둑")

# 태그 제거
def remove_html(texts):
    """
    HTML 태그를 제거합니다.
    ``<p>안녕하세요 ㅎㅎ </p>`` -> ``안녕하세요 ㅎㅎ ``
    """
    preprcessed_text = []
    for text in texts:
        text = re.sub(r"<[^>]+>\s+(?=<)|<[^>]+>", "", text).strip()
        if text:
            preprcessed_text.append(text)
    return preprcessed_text

context = remove_html(context)
for i, sent in enumerate(context):
    print(i, sent)
```

- 문장 분리 : 한국어 문장 분리는 가장 성능이 좋은 것으로 알려진 `kss` 라이브러리를 사용한다.

```python
import kss
sents = []

for sent in context:
    sent = sent.strip()
    if sent:
        splited_sent = kss.split_sentences(sent)
        sents.extend(splited_sent)
for i, sent in enumerate(sents):
    print(i, sent)
```

- 필요없는 다양한 정보들 제거(개인정보, 링크, 해쉬태그 등)

```python
def remove_email(texts):
    """
    이메일을 제거합니다.
    ``홍길동 abc@gmail.com 연락주세요!`` -> ``홍길동  연락주세요!``
    """
    preprocessed_text = []
    for text in texts:
        text = re.sub(r"[a-zA-Z0-9+-_.]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+", "", text).strip()
        if text:
            preprocessed_text.append(text)
    return preprocessed_text
    
def remove_hashtag(texts):
    """
    해쉬태그(#)를 제거합니다.
    ``대박! #맛집 #JMT`` -> ``대박!  ``
    """
    preprocessed_text = []
    for text in texts:
        text = re.sub(r"#\S+", "", text).strip()
        if text:
            preprocessed_text.append(text)
    return preprocessed_text

def remove_user_mention(texts):
    """
    유저에 대한 멘션(@) 태그를 제거합니다.
    ``@홍길동 감사합니다!`` -> `` 감사합니다!``
    """
    preprocessed_text = []
    for text in texts:
        text = re.sub(r"@\w+", "", text).strip()
        if text:
            preprocessed_text.append(text)
    return preprocessed_text

def remove_url(texts):
    """
    URL을 제거합니다.
    ``주소: www.naver.com`` -> ``주소: ``
    """
    preprocessed_text = []
    for text in texts:
        text = re.sub(r"(http|https)?:\/\/\S+\b|www\.(\w+\.)+\S*", "", text).strip()
        text = re.sub(r"pic\.(\w+\.)+\S*", "", text).strip()
        if text:
            preprocessed_text.append(text)
    return preprocessed_text

def remove_bad_char(texts):
    """
    문제를 일으킬 수 있는 문자들을 제거합니다.
    """
    bad_chars = {"\u200b": "", "…": " ... ", "\ufeff": ""}
    preprcessed_text = []
    for text in texts:
        for bad_char in bad_chars:
            text = text.replace(bad_char, bad_chars[bad_char])
        text = re.sub(r"[\+á?\xc3\xa1]", "", text)
        if text:
            preprcessed_text.append(text)
    return preprcessed_text

def remove_press(texts):
    """
    언론 정보를 제거합니다.
    ``홍길동 기자 (연합뉴스)`` -> ````
    ``(이스탄불=연합뉴스) 하채림 특파원 -> ````
    """
    re_patterns = [
        r"\([^(]*?(뉴스|경제|일보|미디어|데일리|한겨례|타임즈|위키트리)\)",
        r"[가-힣]{0,4} (기자|선임기자|수습기자|특파원|객원기자|논설고문|통신원|연구소장) ",  # 이름 + 기자
        r"[가-힣]{1,}(뉴스|경제|일보|미디어|데일리|한겨례|타임|위키트리)",  # (... 연합뉴스) ..
        r"\(\s+\)",  # (  )
        r"\(=\s+\)",  # (=  )
        r"\(\s+=\)",  # (  =)
    ]

    preprocessed_text = []
    for text in texts:
        for re_pattern in re_patterns:
            text = re.sub(re_pattern, "", text).strip()
        if text:
            preprocessed_text.append(text)    
    return preprocessed_text

def remove_copyright(texts):
    """
    뉴스 내 포함된 저작권 관련 텍스트를 제거합니다.
    ``(사진=저작권자(c) 연합뉴스, 무단 전재-재배포 금지)`` -> ``(사진= 연합뉴스, 무단 전재-재배포 금지)`` TODO 수정할 것
    """
    re_patterns = [
        r"\<저작권자(\(c\)|ⓒ|©|\(Copyright\)|(\(c\))|(\(C\))).+?\>",
        r"저작권자\(c\)|ⓒ|©|(Copyright)|(\(c\))|(\(C\))"
    ]
    preprocessed_text = []
    for text in texts:
        for re_pattern in re_patterns:
            text = re.sub(re_pattern, "", text).strip()
        if text:
            preprocessed_text.append(text)    
    return preprocessed_text

def remove_photo_info(texts):
    """
    뉴스 내 포함된 이미지에 대한 label을 제거합니다.
    ``(사진= 연합뉴스, 무단 전재-재배포 금지)`` -> ````
    ``(출처=청주시)`` -> ````
    """
    preprocessed_text = []
    for text in texts:
        text = re.sub(r"\(출처 ?= ?.+\) |\(사진 ?= ?.+\) |\(자료 ?= ?.+\)| \(자료사진\) |사진=.+기자 ", "", text).strip()
        if text:
            preprocessed_text.append(text)
    return preprocessed_text
```

