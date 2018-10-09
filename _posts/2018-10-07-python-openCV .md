---
layout: post
title:  "[OpenCV]Image Viewer"
subtitle:   "보아즈 멘토링"
categories: study
tags: python
comments: true
---

## 이미지 뷰어 만들기

[이미지 뷰어](https://github.com/zzsza/python-simple-application/tree/master/02-image-viewer)

- 시작은 open CV 설치부터
- ~~시작이 반이다 라는 말은 개발자들이 한 말은 아닐까...~~

- 그리고 이 기록은 틀렸을 수도 있지만 삽질하고 또 까먹어서 같은 실수를 반복하는 나때문에 기록함


### 1. anaconda3 에 opencv 설치(anacnda python 3.6 버젼)

##### 방법 1

```conda install -c menpo opencv```

콘다 콘솔에서 입력하면 된다고 하는데 나는 안된다...



##### 방법 2

[ .whl 파일 다운받는 링크](https://www.lfd.uci.edu/~gohlke/pythonlibs/#opencv)

위 링크에 들어가서 ```opencv_python-3.4.3-cp36-cp36m-win_amd64.whl``` 를 다운받아준다. (18.10시점) 

- 시기에 따라 버젼의 차이가 있을 것
- 나 같은 경우는 참고했던 글에서 다운받으라고 하는 파일이  ```3.4.3``` 가 아니었는데 없어서 이것을 받아줬다.



커멘드 창을 열고 ``` pip install ``` 명령어를 이용해서 설치를 진행한다.

```
다운로드한 파일이 있는 경로에서>pip install opencv_python-3.4.3-cp36-cp36m-win_amd64.whl
```

을 진행하고 나면 

``` C:\Users\YOONHOI\Anaconda3\Lib\site-packages``` 경로 아래에 

```opencv_python-3.4.3.dist-info``` 폴더가 생성되었을 것이다. 

- 방법 1이 되지 않아서 이 폴더를 삭제한뒤 다시 방법 2를 진행했다. 



##### OpenCV 설치 확인하기

파이썬 실행시켜셔 ```import cv2``` 했을 때 오류 안나면 성공!



앞으로 프로젝트를 위해 아래 세 개를 import 해 올수 있어햐 한다. 

```
>>> import cv2
>>> import numpy
>>> import matplotlib
```

- numpy와 matplotlib는 이미 설치되어있어서 패스 

