---
layout: post
title:  "NLP Study (5.1)transformer"
subtitle:   "2019-04-28-study-nlp"
categories: study
tags: nlp
comments: true
---

### 시작하기 전에

- 2019 1학기 보아즈 NLP스터디 7회차 (4/28)
- 예습 및 스터디 내용 정리
- 기본적인 진도는 graycode 님의 [nlp-tutorial](https://github.com/jeonyoonhoi/nlp-tutorial) 를 따릅니다.

5. Model based on Transformer

   5-1.The Transformer-Translate

   Paper - [Attention Is All You Need(2017)](https://arxiv.org/abs/1810.04805)

   Colab - [Transformer_Torch.ipynb](https://colab.research.google.com/github/graykode/nlp-tutorial/blob/master/5-1.Transformer/Transformer_Torch.ipynb), [Transformer(Greedy_decoder)_Torch.ipynb](https://colab.research.google.com/github/graykode/nlp-tutorial/blob/master/5-1.Transformer/Transformer(Greedy_decoder)_Torch.ipynb)



## Reference

- [The ilustrated Transformer](http://jalammar.github.io/illustrated-transformer/?fbclid=IwAR00RzJ4AwnPjAAKveNWT91DzjksQq6D5Tlhj98HgIXxMPo2Yc5-MVqVGBw) 아래 두개를 다 본다음 구조를 하나씩 파악해나가면 논문에서 ?? 로 끝났던 생각들이 !! 로 변한다. 영어자료라는 것이 너무 슬프지만 한글로 번역하면서 읽을가치가 있음 너무너무!!
- [논문 한글 번역한 깃허브](https://reniew.github.io/43/#nlp) 논문원본번역해둔 포스팅 
- [NLP with Pytorch (E-Book)](https://kh-kim.gitbook.io/natural-language-processing-with-pytorch/00-cover-10/03-transformer) 이게 한국어 설명이 더 잘되어 있는 느낌이다

------

스터디 시간은 두시간이 예정되어 있었고, 범위는 transformer 와 bert 장장 네시간동안 스터디를 했는데 bert는 건들이지도 못했고 transformer에 대한 이해만 하고 끝났다. 이 마저도 다음 스터디때 다시 해보기로,,, 이 포스팅은 오늘 스터디 내용을 내가 이해한 대로 정리 해보려고 한다. 늘 물음표로 끝나는 우리의 스터디,, 틀린 내용이나 지적할 부분이 있다면 언제든 환영입니다...!

전에 공부한 내용이 Seq2Seq with attention, BI-LSTM with attention 이었다. 모델 with attention, 기존 S2S 와 Bi LSTM 이라는 모델에 attention 이 얹혀진 모양이다. [Attiontion is all you need]에선 RNN이나 CNN 을 사용하지 않고, attention 자체가 모델이 된다. 처음에 든 생각은 말 그대로 **어떻게?** 

제일 궁금했던 모델 아키텍쳐 자체를 제세히 공부했다. 우리는 커다란 구조에서 하나씩 파악해갔다. 그러면서 전체에서 부분을 보기도, 다시 전체 그림을 그리기도 했다.  여기서 가장 도움이 되었던 것이 마지막에 정리하면서 봤던 illustrated transformer 인데, 그 전에 봤을 때는 그냥 잘 그려 놨네 하고 넘어갔다.(아니,, 이땐 보이는게 없었는데 공부하다 보니 이 자료의 소중함을 느꼈다) 감히,,, 나중에 이자료를 한국어로 바꾸기만해도 아주 쓸모있을 것 같다는 생각이 든다. 나처럼 영어자료 기피하는 사람들이 꽤 있으므로.. 

이 개념을 공부하기 전에 이전에 필요한 것은 Seq2Seq(RNN), Attention 에 대한 이해, 인코더 디코더 등이 될 것 같다. 트랜스포머도 결국 기존 방식을 새롭게 조합하고 적용한 것이었고, 앞쪽을 정확하게 이해하고 있으면 더 빠르게 습득할 것이다. (우리 스터디는 결국 복습에 시간을 할애하느랴 오래 걸렸다..)



## Introduction

sequence를 다루기 위한 방법으로 RNN 모델들이 기계번역이나 language model에 흔히 사용된다. 하지만

- 문장의 길이가 길어질 수록 성능이 떨어짐
- batch에 제한이 생김 
- 단순 seq2seq 모델은 문장의 alignment를 해결하지 못한다. 

attention mechanism을 통해 기존 RNN의 문제점을 개선했다. 이 논문에서는 여기서 아이디어를 얻어서, 그럼 attention 만 사용해볼까? 하는 생각을 하게 된 것이다. 



## Model Architecture 

![img](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\transformer)

정말 많이 나왔고, 많이 봤지만 이 그림만 처음에 보면 당연히 어렵다 찬찬히 뜯어보고 다시 보면 이해가 된다. 

이 모델 구조도 인코더-디코더 구조를 띄고 있다. 기존 seq2seq 구조가 RNN 기반의 인코더-디코더 모델이었던 것을 생각하면 큰 틀은 비슷하고, s2s에서 사용한 attention 또한 사용한다. 중요한 것은트랜스포머가 사용하는 **self attention** 이라고 하는 개념이다. 이렇게 트랜스포머에는 attention이 두 가지가 있다. 

트랜스포머의 인코더와 디코더를 이루고 있는 서브모듈은 크게 3가지로 나뉘어 진다. 

| 명칭              | 역할                                                         |
| ----------------- | ------------------------------------------------------------ |
| 셀프 어텐션       | 이전 레이어의 출력에 대해서 어텐션 연산을 수행합니다.        |
| 어텐션            | 기존의 sequence-to-sequence와 같이 인코더의 결과에 대해서 어텐션 연산을 수행 합니다. |
| 피드포워드 레이어 | 어텐션 레이어를 거쳐 얻은 결과물을 최종적으로 정리합니다.    |

[출처](<https://kh-kim.gitbook.io/natural-language-processing-with-pytorch/00-cover-10/03-transformer>) : 여기서 보고 아..! 했다죠...!

- 인코더 : 다수의 self attention layer + feed forward layer
- 디코더 : 다수의 (self attention layer + attention layer) 번갈아 나타나고 + 피드포워드 레이어 

- feedforward layer?
- layer? 
- multihead attention?



### Attention 

구조를 어디부터 뜯어볼까 하다가 내 기준 제일 어렵고 햇갈렸던 attention 이해부터! 이거 알고 나면 다른건 아주 쉽닷! 

![img](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\attention)

이 모델에서 사용한 attention은 두가지 종류이다. 하나는 Scaled dot-product attention 이고 나머지 하나는 MUlti-head attention 이다.  (여기서 내가 햇갈렸는데) 둘이 비교되는 동등한 개념이 아니고 original과 그것을 이용(포함)하여 발전한 형태 이다. 위 그림을 봐도 알 수 있듯이 Scaled Dot-Product Attention이 그냥 attention 이고, Multihead attention 은 이 것을 이용해서 head를 여러개 만들어서 다시 합쳐주는 구조의 attention 이다. 

보통 흔히 사용되는 attention 함수는 addictive attention과 dot-product (multiplicative) attention 두 가지다. (이 두가지는 비교되는 동등한 선의 개념이다.) 후자의 경우가 현재 사용하고 있는 attention과 거의 유사하다. 다른점은 1/√1dk로 scailing을 하지 않았다는 점이다. 

- 속도측면 : dot 승 (matrix multiplication에 대한 최적화된 구현이 많음)

- rescaling을 왜 해주냐? 

​	=> vector의 길이가 길어질수록(dimention이 커질수록) 자연스럽게 dot-product 값은 커진다. 이후에 softmax 함수가 있어서 backpropagation과정에서도 미분값이 조금만 넘어오게 되고, 상대적으로 학습이 느려지거나 잘 안된다. 따라서 dimension이 클 경우를 대비하여 dimention의 루트값으로 나눠준다. 



![Transformer model](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\transformer.png)

[출처](<https://lilianweng.github.io/lil-log/2018/06/24/attention-attention.html>) : 이 그림 만들어주신 분,, 저의 잘못된 이해를바로잡아주심...!



##### Scaled Dot-Product Attention

![123](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\Auqdy3w.jpg)

기본적인 어텐션의 수식은 아래와 같다. 

기본적인 어텐션은 원래 그냥 행렬 곱 연산인데 scaled라는 이름이 붙은 이유는 키의 차원이 주어졌을 때 행렬 곱 연산 결과를 이를 이용하여 나눠주기 때문이다. rescaling 이유는 위에 설명했다. 이것 외로는 이전의 attention과 동일하다. 

해당 attention의 input은 3가지다. dk dimension을 가지는 queries와 keys, 그리고 dv dimension을 가지는 values로 구성된다. 우선 하나의 query에 대해 모든 key들과 dot product를 한 뒤 각 값을 dk−−√dk로 나눠준다. 그리고 softmax함수를 씌운 후 마지막으로 value를 곱하면 attention 연산이 끝난다.

실제로 계산할때는 query, key, value를 vector하나하나 계산하는 것이 아니라 여러개를 matrix로 만들어 계산한다. 

* query key value 에 대한 궁금증 실제 같은 값일까?
  * 이건 아직 물음표인데, key 와 value 의 가중치 초기값이 다르기 때문에 가중치를 곱해지면 다르긴 하지만 가중치가 나중에 비슷해 지려나,,,?







* 각각의 역할과 실제 값이 궁금하다. 

![img](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\transformer_self_attention_vectors.png)

우선 쿼리가 어떤 단어랑 관련되어 있느닞 찾기 위해 하나의 쿼리당 **모든키**와 연산을 한다. 쿼리 키를 dot product 하고나서 softmax를 취하는데 이는 계산값을 확률로 바꿔주는 것이다. 즉 쿼리와 어떤 키가 가장 높은 연관성을 지닌는지 알게되고 이후에 value를 곱하는것은 그 특정 key-value 에 대해 scaling한다고 보면된다. 

key 와 value 는 사실상 같은 단어를 의미한다. 하지만 두 개로 나눈 이유는 (내생각엔 그냥 만든사람 마음같지만) key는 각 쿼리와의 연관성 확률을 계산하면서 쓰여지고, value는 값을 변형하지 않고 그대로 가지고 있다가 그 확률을 사용해서 attention 값을 계산하는 용도이다. 





##### Multihead attention

트랜스포머는 여러개의 어텐션으로 구성된 멀티헤드어텐션을 제안한다.

기존의 attention은 전체 dimention에 대해서 하나의 attention만적용시켰다. 여기서 사용한 Multi-head attention이란 전체 demension에 대해서 한번 attention을 적용하는 것이 아니라 전체 dimension을 h로 나눠서 attention을 h 번 적용시키는 방법이다. 



- **왜 head를 늘렸을까?**

  - 이는 마치 CNN 에서 여러 개의 필터(커널) 가 다양한 피쳐들을 뽑아내는 것과 같은 원리 
  - 내가 이해하기로는 기존에 배운 모델들이 초기값들에 따라 결과가 다르게 나오는 경우, 그 수행 횟수를 늘려서 평균을 내거나 근접한 값을 답으로 채택하는 경우가 있었는데 여기서도 그런 원리로 적용한게 아닌가 싶다. illustrated transformer 의 설명에 따르면 multihead 에서 각 헤드별로 초기값은 랜덤설정된다. 즉 다 다른값으로 시작되는데 이에 따라 서로 다른 결과가 추출되는 것이다. 성능 향상에 도움 될 것이라고 직관적으로 이해했다. 

  어텐션은 쿼리(query) 를 만들어 내기 위한 선형변환을 배우는 과정이다. 이 때 다양한 쿼리들을 만들어내어 다양한 정보들을 추출할 수 있다면 훨씬 더 유용할 것이다. 따라서 멀티헤드를 통해 어텐션 여럿을 동시에 수해한다. 

![1556534538503](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\1556534538503.png)

각 Query, Key, Value의 vector 는 linearly 하게 h개로 project 된다. 이후 각각 나눠서 attention을 시킨 후 만들어진 h개의 vector를 concat 하면 된다. 마지막으로  vector 의 dimension을 d model로 다시 맞춰주도록 matrix를 곱하면 끝난다. 수식으로 나타내면 아래와 같다. 

![1556534561389](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\1556534561389.png)

- 셀프어텐션의 경우에는 QKV 모두 같은값, 이전 레이어의 결과를 받아오게 된다.

- 일반 어텐션의 경우 쿼리 Q는 이전 레이어의 결과, K와 V는 인코더의 마지막 레이어 결과가 됩니다. 

이 경우 Q K V 텐서의 크기는 아래와 같다 n은 소스문장의 길이, m은 target 문장의 길이를 의미한다.

![1556534792323](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\1556534792323.png)



멀티헤드 함수에는 선형변환을 위한 수 많은 뉴럴 네트워크 웨이트 파라미터들이 존재한다. 그 크기는 아래와 같다. 

![1556534863843](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\1556534863843.png)

![1556535005032](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\1556535005032.png)



##### Applications in attention in our model 

- 인코더는 self-attention layer를 가진다. 모든 키 밸류 쿼리는 같은 sequence에서 온다. 정확히는 이전 layer의 output에서 온다. 따라서 encoder는 이전 layer의 전체 위치를 attend할 수 있다. 
- e-d attention layer에서 쿼리는 이전 decoder layer에서 온다. 그리고 encoder에서 온 key와 value를 사용한다. 따라서 decoder의 모든 위치의 token은 input senquence의 어느 곳이든 attend 할 수 있게 된다. 
- 디코더의 self- attention layer도 이전 layer의 모든position을 attend할 수 있는데, 정확히는 자신의 position이전의 position까지만 attend 할 수 있다. 직관적으로 이해하면 문장에서 현재 단어이전의 정보만을 참고하도록 한 것이다. 이러한 목적을 scaled dot-product를 masking함으로써 구현했다. 



오늘은 여기까지,,

다음 스터하고나서 디코더 부분이나 전체 구조 추가로 써야겠다!

아래 자료가 attention 전체 구조 이해하는데 정말 큰 도움 되었기에 다음에 꼭 한글로 번역 해보기로...!

[Visualizing s2s with attention](https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/)

