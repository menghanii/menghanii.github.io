Key variables (주요변수)

1. News engagement

   1. Number of shares
   2. Number of comments
   3. Number of reactions

   이 세 개 변수의 상관계수 -> positive & 통계적으로 유의.

2. The extent of positiveness of news text
   - 뉴스컨텐츠의 긍정성의 정도 -> sentiment analysis
   - 해당 뉴스 ->

3. Discrete emotions of news visuals
   - azure face api를 통해 나온 감정 정도
   - anger, contempt, disgust, fear, happiness, neutral, sadness, surprise
   - 1에 가까운 값일수록 강하다는 것
   - 8개 감정의 sum = 1
   - 사람 수가 많은 경우는 avg score를 활용함.
   - 여기서는 3명 이하만 사용함. 너무 많아지면 사실상 face detect를 제대로 하지 못한다고 판단.



Control variables(통제변수)

1. number of news postings per day
2. subjectivity : 주관성을 측정 by nltk. 1에 가까울수록 greater subjectivity
3. number of faces : 너무 얼굴 개수가 많으면 사진에 얼굴이 조그맣게 나옴. -> emotional transfer가 제대로 안될 가능성. 즉, 한명의 얼굴이 크게 나와있는 사진의 전달성이 높음.
4. Proximity :  -> 우리 df엔 없음.
5. News Publisher : 우리 분석에서는 nyt로 통일됨.
6. News Section : 우리 분석에서는 politics로 통일됨.

---

모집단 모형
$$
\begin{align*}
y = &b_0 + b_1 *\text{num_faces} + b_2 * \text{description_subj} + b_3 * \text{message_subj}+b_4*\text{avg_N_second_comments}\\
& b_5 * \text{description_senti}+ b_6 * \text{message_senti} + b_7 * \text{avg_happiness} + b_8 * \text{avg_sadness}+\\
& b_9 * \text{avg_anger}+ b_{10} * \text{avg_fear} + b_{11} * \text{avg_disgust} + b_{12} * \text{avg_contempt} + b_{13} * \text{avg_surprise} + u
\end{align*}
$$
