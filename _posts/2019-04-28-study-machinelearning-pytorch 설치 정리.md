# Window10 (x64) + Anaconda + Pytorch 설치하기 + Jupyter notebook에서 사용

내 동생 노트북은lg그램,, 내 노트북 고치는 동안 빌려서 다시 설치했다.

문제는 아래 문구였다. 내가 보려고 정리해두는 포스팅

```python
The following packages are not available from current channels :
- pytorch
```

구글링 열심히 하고 가상환경만들어서도 해보고 이런 저런 명령어도 바꿔봤는데 계속 만났다.  결국 처음부터 다시해보면서 문제를 찾은 결과,,, 어이없게도 아나콘다를 잘못 설치했던거였다. 64-bit 버전을 설치했어야 했는데 32-bit 버전을 설치했었고, 파이토치는 64-bit 만 지원하고 있다.(내가 작성하는 시기 기준으로, 공식 문서에서 문구를 확인했다. )

이왕 열심히 찾아본 것들,,, 설치가 반이라고 맨날 설치에서 애먹고 같은 실수를 반복하는 나와 같은 분들을 위해 정리한다...

## 1. Anaconda

### Anaconda 란

그냥 깔라고 하니까 따라 했지만 뭔지 알고 쓰면 더 좋을것 같아서 찾아보았다. 

기본 python은 pip패키지를 통합 관리하는 모듈만 있고 여러 패키지를 하나하나 설치해야 한다. 그러나 아나콘다는  

- 1000 개가 넘는 파이썬 패키지를 제공
- 파이썬 버젼을 바꿔가며 사용 가능 (굳이 재설치할 필요가 없다. 버젼들 간에 서로 지원 안하면서 각자 업그레이드 되는 경우 문제 생긴 경우들 많아서 개인적인 의견이지만 ,, 참 좋은것 같다. )

아나콘다는 파이썬 기본 패키지에 각종 수학/과학 라이브러리들을 같이 패키징해서 배포하는 버전입니다. 대표적으로 ```pandas``` ,```numpy``` , ```scipy``` , ```sklearn``` , ```matplotlib``` , ```Jupyter Notebook``` 등이 존재한다. 

![Image](C:\Users\dbsgh\Desktop\jeonyoonhoi.github.io\assets\img\.gitignore)

- 둘 다 설치할 경우 중복되는 파일이 많고 환경변수 충돌 등 문제가 발생하므로 기본 파이썬 공부 이상을 원한다면 아나콘다를 처음부터 설치하는 것을 추천한다. 

### Anaconda 설치

(이미 아나콘다가 있다면 이 과정은 PASS)

- 설치 파일은 [아나콘다 공식 홈페이지](<https://www.anaconda.com/distribution/>)에서 받을 수 있다. 
- 설치 경로는 recommended 해주는대로 All user 체크한다. 
- Advanced Options 에서 **Add Anaconda to my PATH environment variable** 를 체크해준다. 

> 여기서 기존에  파이썬이 설치되어 있다면 환경변수가 충돌하여 문제가 발생한다. (파이토치 말고 텐서플로우에도 해당되는 문제)
>
> 해결방법은 
>
> 1. 환경변수편집
> 2. 기존 파이썬 삭제 후 아나콘다 설치
> 3. PATH추가 체크하지않고 아나콘다 프롬프트에서 사용
>
> 이렇게 있는데 개인적으로는 2번방법을 사용하였다.  아나콘다 설치하면  파이썬이 같이 설치되기 때문에 굳이 기존 파이썬만 설치되어 있을 필요가 없고 대부분 작업을 아나콘다 환경 아래에서 하는게 편하기 때문. 그리고  처음에 1번 방법을 하다가 익숙하지 않았던 내게는 어렵게 다가왔었다... 물론 아무렇지 않게 바꿀 수 있는 분들도 계시겠지만!

### Pytorch 설치, 윈도우에서,

- 파이토치는 텐서플로우와 다르게 CPU GPU 버전이 나뉘어져 있고, 단일 패키지만 존재한다. 
- CUDA 지원 그래픽 카드가 있으면 GPU 설정을 해주면 되고 없으면 기본적으로 CPU 를 통해서만 학습이 이루어진다. 
- 공식적으로 리눅스와 맥(OS X)을 지원하며 리눅스의 경우 CUDA는 어떤 형태로 설치하던지 상관없이 지원하지만 맥은 직접 소스 컴파일을 해야 CUDA를 지원한다. (윈도우 사용자라면 상관이 없는 이야기,,)

- 공식적으로 윈도우 환경을 지원하지 않는다. (약간의 꼼수 필요) 



  ### 가상환경 만들고 그 안에서 설치하기

```
conda create -n pytorchenv python=3.6
# 이름이 pytorchenv 인 python 3.6 버전을 사용하는 가상환경 만든다
# 이름은 맘대로 지어준다. -n == -name

conda info --envs
# 만들어진 가상환경들을 볼 수 있다.

activate pytorchenv
# 파이썬가상환경을 활성화 시켜준다. 
```

```
#(pytorchenv)이 안에 들어와서 
(pytorchenv)$ conda install -y -c peterjc123 pytorch
# 구글링 해보면 이 명ㄹ령어 많이 나오는데 공식명령어로도 될 것 같다
# -y 는 같이 넣는게 편하다 y/n 물어보면 yes로 알아서 넘어간다. 
# 공식명령어는 : 
conda install pytorch-cpu torchvision-cpu -c pytorch 

(pytorchenv)$ conda install ipykernel
#주피터 노트북에서 사용해주기 위해 해준다. 
# ipykernel : 이 패키지가 설치된 호나경
```

주피터 노트북을 키고 import torch하면 된다!

이유는 모르겠는데 나는 activate 가상환경 아니고 그냥 (base) 인 상태에서도 된다. 

이것 저것 해보다가 뭔가 잘(?) 했나보다 ㅎㅅㅎ....



### Jupyter 에 새 kernel등록하기

```
(pytorchenv)$ python -m ipykernel install --user --name pytorch --display-name "PyTorch"
#"PyTorch"는 쥬피터에서 보이는 네이밍이라 다른걸로 바꿀 수 있다. 
```



### ** 궁금해서 찾아본 것들 추가

##### PATH추가 이유?

환경변수 설정을 하면 cmd창이나 windows powershell 에서 아나콘다 패키지를 바로 사용할 수 있다.  그렇지 않았다면 Anaconda Prompt를 이용해 아나콘다 패키지를 이용할 수 있다. (나는 패스설정했지만 습관적으로 anaconda prompt를 사용했다)

- (만약 설치 전에 Python 이 설치되어 있었다면 파이썬을 uninstall )
- 설치 완료 후 cmd/powershell/anaconda prompt 등을 열어서 확인해본다.



##### 환경변수확인

- 파일탐색기를 연다
- 내PC>오른쪽버튼>속성 (혹은) 제어판>시스템및보안>시스템
- 고급 시스템 설정 > 시스템 속성 (창이뜬다)>고급 탭에 들어가서
- 맨 아래 환경변수 



##### 공식 홈페이지 설명

- Supported Windows Distributions

  PyTorch is supported on the following Windows distributions:

  - [Windows](https://www.microsoft.com/en-us/windows) 7 and greater; [Windows 10](https://www.microsoft.com/en-us/software-download/windows10ISO) or greater recommended.
  - [Windows Server 2008](https://docs.microsoft.com/en-us/windows-server/windows-server) r2 and greater

- Python

  - Chocolatey
  - Python website
  - Anaconda

- Package Manager

  **파이썬 3.x,  64-bit graphical installer  를 사용해야 한다.** 

  - Anaconda/NoCuda 로 사용했다. 
  - PyTorch 공식 홈페이지 [링크](<https://pytorch.org/get-started/locally/>) 에서 필요한 명령어 확인

  ```
  conda install pytorch-cpu torchvision-cpu -c pytorch 
  ```



##### CUDA란?

- Computed Unified Device Architecture 는 NVIDIA 사에서 개발한 GPU(Graphic Processing Unit) 개발 툴이고 많은 양의 연산을 동시에 처리하기 위해 사용한다. 
- CUDA 사용가능한 GPU 목록의 [링크](<https://www.geforce.com/hardware/technology/cuda/supported-gpus>)



##### 그래픽 카드 확인은? (내장 및 외장)

- DirectX 진단 도구 
- 시작>검색>dxdiag 를 입력하면 메뉴창이 뜬다
- CPU OS Memory 등을 확인할 수 있는데 노트북 그래픽카드는 디스플레이에서확인
  - 이름 : Intel(R) Graphics 520 
  - 나는 CUDA 사용 X
- 외장카드 확인 : 제어판>시스템 및 보안>시스템>장치관리자>디스플레이 어댑터 



##### Torchvision 이란?

- The [`torchvision`](https://pytorch.org/docs/stable/torchvision/index.html#module-torchvision) package consists of popular datasets, model architectures, and common image transformations for computer vision.

  [출처:공식문서](<https://pytorch.org/docs/stable/torchvision/index.html>)

- 컴퓨터 비전에 쓰이는 유명한 데이터셋, 모델구조로 이루어진것!



##### peterjc123



