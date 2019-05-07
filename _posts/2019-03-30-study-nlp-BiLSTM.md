---
layout: post
title:  "NLP Study (3.3)Bi-LSTM"
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

이제 자연어 스터디 4회차에 들어섰다.  









# Ref : 

- [BiLSTM Hegemony-ratsgo's blog](<https://ratsgo.github.io/natural%20language%20processing/2017/10/22/manning/>)





- attention이 적용된 Bi LSTM 모델이 최고 성능을 낸다는 이약. 



### BiLSTM with attention

![img](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\vanillaRNN)

<vanilla RNN> 구조는 다음과 같다

- gradient vanishing/exploding 문제에 취약하다. 
- 체인룰에 의해 빨간색 선에 해당하는 그래디언트를 계쏙 곱해주어야 하기 때문!
- LSTM 은 'cell state'를 도입해 그래디언트 문제를 해결하고자  함
- LSTM은 직전 시점 정보와 현 시점 정보를 더해줌으로써 그래디언트가 효과적으로 흐를 수 있게 한다.

![img](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\0h3kgMY.png)



![img](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\lcGUt9N.png)

vanilla Seq2Seq 모델은 다음과 같다. 소스문장을 벡터화 하는 인코더(encoder) 와 인코딩된 벡터를 타겟 문장으로 변환하는 디코더(decoder) 들로 구성돼 있습니다. 이때 인코더, 디코더는 LSTM 셀을 씁니다. 

Encoder 

- 문장 길이가 길고 층이 깊으면, 인코더가 압축해야 할 정보가 너무 많아서 정보 손실이 일어남
- 디코더는 인코더가 압축한 정보를 초반 예측에만 사용하는 경향을 보임
- 이때문에 Encoder-Decoder 사이에 bottle-neck 문제가 발생한다. 
- 이에 디코더 예측시 가장 의미있는 인코더 입력에 주목하게 만드는 attention 매커니즘이 제안됨
- 또한 앞->뒤, 뒤-> 모두 고려하는 bidirectional 네트워크 또한 성능 개선에 도움이 된다. 



![img](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\jO4oAb6.png)

<BiLSTM with attention> 은 특히 기계번역에서 좋은 성능을 내 주목을 받았다. 

그 비결은 다음 네 가지

- 종단간(End-to-end)학습 : 출력값(output)에 대한 손실을 최소화 하는 과정에서 모든 파라메터들이 동시에 학습된다. 
- 분산표상(distrubuted representation)사용 : 단어와 구(pharse) 간 유사성을 입력벡터에 내재화해 성능을 개선했다
- 개선된 문맥탐색(exploitation) : LSTM , attention 등 덕분에 문장 길이가 길어져도 성능 저하를 막는다. 
- 자연스러운 문장생성 능력 :  다범주 분류에 좋은 성능을 내는 딥러닝 기법을 써서 문장생성 능력이 큰 폭으로 개선됐다



### Stanford NLP group 연구성과 소개



##### Stanford attentive Reader

- 문맥에서 질의에 대한 응답을 찾는 모델
- passage 와 question을 각기 다른 양방향 인코더에 넣어 p와 q벡터를 만듦
- 단락의 i번째 단어에 대한 어텐션 스코어는 A =  softmax(qWp) 입니다. 디코더의 output vector o 는 sum(Ap) 이다. 
- 이를 정답 답변과 비교해 손실을 구하고 역전파 학습이 이루어지는 구조 











