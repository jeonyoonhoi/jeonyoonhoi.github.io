---
layout: post
title:  "lecture 2"
subtitle:   "2018-08-11-study-cs231-lec2"
categories: study
tags: cs231
comments: true
---



# Lecture 3. Loss Functions and Optimization



- Assignment1이 나왔습니다...

- piazza
- Google Cloud 



## lecture 2 복습

이미지 인식에서의 어려움 ㅠㅠ

iluumination 등의 문제

k-nearest 

cross validation 

linear classifier



1. Define a loss function that quantifies our unhappiness with the score across the training data
2. come up with a way of efficiently finding the parameters that minimize the parameters that minimize the loss function.(optimization)



특정알고리즘으로 W 의 최적값을 찾아내기!

Loss function 가능한 W의 값 범위에서 



0~9사이에서 CIFAR-10  CLASSIFICATION

training data 의 loss를 최소화 하는 W의 값



hinge loss?

Ss :  predicted score



+1 은 왜 하는가

arbitary choice, 



loss functions의 직관적인 이해



Q2. min max of SVM : min 0   max infinity

Q3. At initialization W is small so all s=0 What is the loss?

A. number of classes - 1

if looping over all of the incorrect classes, so we're looping over C-1 classes, within ech of those classes 2 S will be same so we'll get a loss of 1 because of the margin and we'll get C-1



debugging stratage. 



Q4. What if the sum was over all classess

A. The loss increases by one



Q5. Why mean not sum



Q6. What if we used 

differenct trade off non-linear function 



linear vs square



#### Multicalss SVM Loss :  Example code

$$
Li =sum(j=!y_i) max(0,sj-syi +1)
$$

``` python
def L_i_vectorized(x,y,W) : 
    scores = W.dot(x)
    margins = np.maximum(0,scores - sdcores[y]+1)
    margins[y] = 0
    loss_i = np.sum(margins)
    return loss_i
```



## Softmax Classifier (Multinomial Logistic Regression)



scores = unnormailzed loog probabiliies of the classes.





### Recap

![1535520510377](C:\Users\YOONHOI\AppData\Local\Temp\1535520510377.png)





## Opimization

idea : 

1. Random Search

- Bad algorithm

2. Follow the slope

- the gradient is the vector of along each dimenion
- The slopein any diirection is the dot product of the direction with the gradient 
- The direction of steepest descent is the negative gradient
- current W is the primary vector and gradient dW each slot tell us how much that change 



Histogram of ORiented Gradients(HoG)

- Divide image into 8by8 pixel regions 
- Within each region quantize edge direction into 9 bins



Bag of Words

step1. Build codebook

- extranc randon patches
- Cluster patches to form codebook of viusal words

step2. Encode images

- 



### Image features vs ConvNets

- after extracting feature, it just fixed block
- once remove cnn , learn from data 



 