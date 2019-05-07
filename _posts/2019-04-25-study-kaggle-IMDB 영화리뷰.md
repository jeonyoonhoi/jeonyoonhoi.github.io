---
layout: post
title:  "[kaggle]IMDB 영화리뷰"
subtitle:   "2019-04-25-study-nlp"
categories: study
tags: kaggle
comments: true

---



# REF

* 캐글 [ Kaggle 경진대회 링크](https://www.kaggle.com/c/word2vec-nlp-tutorial)

* 인프런 강좌[[NLP]IMDB 영화리뷰 감정분석을 통한 파이썬 자연어 처리](<https://www.inflearn.com/course/nlp-imdb-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9E%90%EC%97%B0%EC%96%B4-%EC%B2%98%EB%A6%AC/dashboard>)



### 캐글의 다른 자연어 처리

* sentiment anlysis on movie revies
* Toxic comments 악플 찾는 경진대회
* Spooky author 문장의 작가를 판별하는 경진대회



### 튜토리얼 구성

#### part 1. 

- 초보자를대상으로 기본 자연어 처리를 다룬다 



#### part 2,3.

- Word2Vec 을 사용하여 모델을 학습시키는 방법과 감정분석에 단어 벡터를 사용하는 방법을 본다. 
- part3는 레시피를 제공하지 않고 w2v을 사용하는 몇 가지 방법을 실험해본다. 
- part3에서는 K-means 알고리즘을 사용해 군집화를 해본다. 
- 긍정과 부정 리뷰가 섞여있는 100000개의 IMDB감정분석 데이터세트를 통해 목표를 달성해본다. 



#### 평가 - ROC 커브(Reciever-Operating Characteristic curve)

- TPR( ), FPR( )
- 민감도 TPR
  - 1인 케이스에 대해 1로 예측한 비율
- 특이도 FPR
  - 0 인 케이스에 대해 1로 예측한 비율
- ROC 커브의 밑 면적이 1에 가까울수록 (왼쪽 꼭지점에 다가갈수록) 좋은 성능을 낸다. 
- X,Y 둘 다 (0,1) 범위이고 (0,0)에서 (1,1)을 잇는 곡선이다. 
- 



