---
layout: post
title:  "NLP Study (4.1)Seq2seq"
subtitle:   "2019-03-30-study-nlp"
categories: study
tags: nlp
comments: true
---

### 시작하기 전에

- 2019 1학기 보아즈 NLP스터디 4회차 (3/30)
- 예습 및 스터디 내용 정리
- 기본적인 진도는 graycode 님의 [nlp-tutorial](https://github.com/jeonyoonhoi/nlp-tutorial) 를 따릅니다.

------

4. Attention Mechanism

   4-1. Seq2Seq - Change Word

   [paper : Learning Phrase Representations using RNN Encoder-Decoder for Statistical Machine Transflation (2014)](https://arxiv.org/pdf/1406.1078.pdf)

   4-2. Seq2Seq with Attention - Translate

   [paper:Neural Machine Translation by Jointly Learning to Align and Trnaslate(2014)](<https://arxiv.org/abs/1409.0473>)





# Ref : 

- [딥러닝을 이용한 자연어 처리 입문](https://wikidocs.net/24996) 12. 기계번역 파트 
- [어텐션 매커니즘 , ratsgo's blog](https://ratsgo.github.io/from%20frequency%20to%20semantics/2017/10/06/attention/)





# 딥러닝을 이용한 자연어 처리 입문

12. 기계번역( Neural Machine Translation)

    1) 시퀀스 투 시퀀스 (seq2seq)

    2) attention mechanism

    3) BLEU( Bilingual Evaluation Understudy )



### 1) Sequence to Sequence 

- 입력된 시퀀스로부터 다른 도메인의 시퀀스를 출력하는 다양한 분야에서 사용되는 모델

- Ex1 ) 챗봇, 기계 번역기

  입력 : 출력 = 질문 : 대답 (챗봇) , = 문장 : 번역문장

- Ex2 ) Text Summariazation, STT(Speech to Text) 에서도 쓰일 수 있음 



#### 구성

- 크게 인코더와 디코더로 구성됨
- context vetor : 인코더  입력 문장에 있는 모든 정보들을 압축해서 하나의 벡터로 만듦
- 인코더는 컨텍스트 벡터를 디코더로 전송
- 디코더는 컨텍스트 벡터를 받아서 번역된 단어를 한 개씩 만들어 낸다. 

![img](https://wikidocs.net/images/page/24996/%EC%9D%B8%EC%BD%94%EB%8D%94%EB%94%94%EC%BD%94%EB%8D%94%EB%AA%A8%EB%8D%B8.PNG)



- 인코더, 디코더 아키텍쳐 내부는 LSTM 셀 또는 GRU 셀들로 구성됩니다.
- 여기서 말하는 LSTM 셀과 GRU 셀은 각각 LSTM 챕터와 GRU 챕터에서 배웠던 은닉층의 메모리 셀을 말함 (사진에는 LSTM 으로 설명)



##### 인코더

- 우선 인코더를 자세히 보면, 입력 문장은 단어 토큰화를 통해서 단어 단위로 쪼개지고 단어 토큰 각각은 LSTM 셀의 입력이 됨
- 인코더는 입력단어들을 하나의 정보벡터로 압축한 컨텍스트 벡터를 디코더에게 넘겨준다



##### 디코더 (학습)

- 디코더를 자세히 보면, 초기  입력으로 문자으  이 시작을 의미하는 특수문자 <sos> 가 들어감
- <sos>가 입력되면 디코더는 다음에 등장할 확류링 높은 단어를 예측함
- 이 작업을 반복하고 이 문장의 끝을 의미하는 특수문자인 <eos> 가 다음 단어로 예츠고될 때 까지 반복

- 이것은 테스트 과정 동안의 이야기!

- seq2seq 는훈련 과정과 테스트과정( 또는 실제 번역기를 사람이 쓸 떄 )의 작동 방식이 조금 다릅니다. 

- 훈련 과정에서는 디코더가 인코더에게 보낸 컨텍스트 벡터와 실제 정답 상황인 <sos> ~~ ~~ ~~ 를 입력 받았을 때, ~~ ~~ ~~ <eos> 가 나와야 한다고 정답을 알려주면서 훈련됨

   

##### 디코더 (테스트)

- 하지만 테스트 과정에서는 앞서 설명한 과정처럼 디코더는 입력을 받지 않고 오직 컨텍스트 벡터와 <go>만을 입력으로 받은 후에 다음에 올 단어를 에측하고 그 단어를 다음 LSTM 셀에 입력으로 넣는 행위를 반복



##### 단어임베딩

- 기계는 숫자를 처리함
- 자연어 처리에서 텍스트를 벡터로 바꾸는 방법으로 단어 임베딩이 사용됨                                
- seq2seq 에서 사용되는 모든 단어들은 word2vec 등의 방법론 등을 통해 임베딩 벡터로서 표현되어야합니다. 
- 위 그림은 모든 단어에 대해서 임베딩 과정을 거치게 하는 단계인 임베딩 층의 모습을 보여줍니다. 
- 실제 임베딩 벡터는 수백개의  차원을 가질 수 있다.  



##### LSTM 셀

- LSMT 셀은 각각의 시점(time-step) 마다  두개의 입력을 받음
- 더 정확히는 바닐라 RNN셀이 아니라 LSMT 셀이므로 은닉상태 뿐만아닐 셀 상태 또한 전달됨 (여기선 생략)

- 현제 시점을 t라고 할 때 LSTM셀은 t-1에서의 은닉상태와 t에서의 출력 벡터를 만듦

  단 이때 t에서의 출력벡터는 바로 뒤에 또 다른 층이 존재할 경우에 입력으로 보내거나 필요없는 값은 무시할 수 있다. 

- 다음 시점에 해당하는 t+1의 LSTM셀의 입력으로 현재 t에서의 은닉 상태를 입력으로 보냄

- 에서의 은닉상태는 과거 시점의 동일한 LSTM 셀에서의 모든 은닉 상태의값들을 누적해서 받아온 값이라고 할 수 있다. 

- 따라서 컨텍스트 벡터는 사실 인코더에서의 마지막 LSTM 셀의 은닉 상태값을 말하는 것이며, 이는 입력 문장의 모든 단어 토큰들의 정보를 요약해서 담고 있다고 할 수 있음 



##### 디코더 

- 디코더는 인코더의 마지막 LSTM 셀의 은닉 상태인 컨택스트 벡터를 첫 번째 은닉 상태으 값으로 사용합니다. 













# 어텐션 매커니즘, ratsgo's blog

딥러닝 모델이 특정 벡터에 주목하게 만들어 모델의 성능을 높이는 기법인 어텐션 매커니즘!



### 동기

어텐션 매커니즘은 machine translation을 위한 S2S 모델에 처음 도입되었다. 















