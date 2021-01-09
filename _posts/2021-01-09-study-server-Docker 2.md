---
layout: post
title:  "Docker (2) "
subtitle:   "2021-01-09-docker(2)"
categories: study
tags: server
comments: 3. Docker 구성요소 및 Docker Image 관리
---

# 3. Docker 구성요소 및 Docker Image 관리

## 1) Docker 구성요소

- Docker Engine (Docker Daemon, Docker Runtime)
  - shipping yard(적하 야적장) - 물건을 배송할 떄 옮기는 장소 및 환경 필요
  - $systemctl status docker.service
- Docker Images
  - shipping Manifest(적하목록) : 배송할 물건들 (피아노,쿠키, 자동차, 쌀)
  - $docker images
- Docker Containers
  - Shipping Containers(적하 컨테이너) : 물건들을 컨테이너에 넣어서 배송
  - $docker ps -a
- Docker Registry
  - Docker Repository를 운영하는 Server
  - ex_) [ktds12.azurecr.io](http://ktds12.azurecr.io) ## azure Cloud
  - Docker Repository 를 생성하려면 먼저 Docker Registry 를 구축해야 하는데, 특별하게 지정하지 않고 docker image를 검색할 때는 http://hub.docker.com 에서 진행한다.
- Docker Repository
  - Docker Image들을 저장해 둔 장소를 말한다.
    - ktds12.azurecr.io의 nginx 가 docker repository 이다.
    - [hub.docker.com](http://hub.docker.com) 의 ezra1044/centosyslee 가 docer repository 이다.
  - docker image 는 docker registry 의 repository 에 저장되어 있다.
  - 이러한 이미지를 로컬 컴퓨터로 다운로드하여 실행하여 container 로서 사용한다.
  - $docker **search** apache

## 2) Docker Image 관리

### Docker Image란?

- A template of container
- 사전에 Application 과 그것의 실행환경이 설치되어 있다.
- Dockerfile 이나 container를 사용하여 Image를 생성할 수 있다.
- Image 가 생성된 후에는 Read-Only 가 된다.

### Docker Container 란?

- Application을 실행하고 서비스하는 모든 환경을 갖춘 소프트웨어
- Image로부터 Container를 생성한다.
- 실행중인 프로세스를 말한다
- 컨테이너를 실행한 후에 수정도 가능하고 Access도 가능하다
- Container에서 수정된 것을 적용하여 새로운 Image를 생성할 수 있다.

### Image vs Container

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/48e71296-2fb0-4b05-9668-b2cc67997b6b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/48e71296-2fb0-4b05-9668-b2cc67997b6b/Untitled.png)

Class : Instance = Image : Container

Image는 수정할 수 없으며, 확장 할 수 있다. Image 가 실행중일떄 Container로 변형된다.

### Docker Repository 란?

- Docker Image 를 저장해 둔 곳 (Registry)
- http://hub.docker.com 에 저장함
- Docker hub가 제공하는 저장소에는 공개/비공개 두가지가 있는데 공개 저장소는 사용 개수에 제한이 없이 무제한으로 사용할 수 있다.
- 비공대 저장소는 기본적으로 1개 무료 제공

### Docker Image 관리 명령어

- 사용하고자 하는 Docker Image 검색하기 : docker search

```powershell
Docker Image 는 Docker 에 저장되어 있다.
Docker search 명령으로 원하는 image를 찾을 수 있다. 
docker help
docker search --help
docker search --filter=stars=3 apache
docker search --filter=stars=3 --no-trunc apache ## 설명란 모두 보기
docker search --filter=is-official=true apache
docker search --filter=is-official=true --no-trunc apache
docker search ubuntu:14.04 ## 특정한 버전 지정하여 검색하기 
```

- NAME : 이미지 이름. 이름에서 /로 구분되어 있는 것들은 개별 사용자들의 공개 저장소에있는 이미지임을 알려준다.

- DESCRIPTION: 이미지에 대한 상세 설명

- STARS :  이미지가 사용자들로부터 몇개의 별을 받았는지 보여준다.

- OFFICIAL: 이 이미지가 공식 이미지 여부 확인

- AUTOMATED : 이 이미지가 github 나 bitbucket 같은 외부 소스 저장소에 있는 소스를 dockerhub에서 자동으로 빌드하고 업데이트 한 것인지 여부

- 찾은 이미지를  로컬로 다운로드만 하기 : docker pull

  필요한 image를 (build-time)로컬 컴퓨터에 저장해 놓고 필요할 때마다 실행하여 (container(=runtime)) 사용한다.

  ```powershell
  - $docker pull —help
  - $docker pull fedora
  - $docker images
  - $docker images fedora
  ```

- 저장된 이미지 또는 docker hub에서 다운로드 받은 이미지 실행하기

  ```powershell
  - $docker run -it fedora /bin/bashe
  - $exit
  - $docker run -it —name myfedora fedora /bin/bash
  - $Ctrl+p, ctrl+q
  - $docker ps -a
  ```

  다운로드 받은 이미지를 실행할 때는 금방 실행되고,

  Docker 이미지를 실행하면 새로운 container가 생성된다.

- 실행이 중지된 container 실행하기

  Docker 이미지를 실행하면 Container가 되며, 내용을 편집하여 다시 저장할 수 있다. 다시 저장되면 Docker image 가 아니고 Container 라고 한다. Docker 를 실행하여 사용하는 방법은 1) 중지된 container를 시작하거나 2) docker 원본 Image를 실행 할 수 있다.

  ```powershell
  #중지된 container 를 다시 실행하기 위해 실행되고 있거나 실행이 중지된 container 목록을 확인한다. 
  $docker ps -a
  #status를 확인후 중지된 container를 실행한다. 
  $docker start "container id"
  
  $docker ps
  $docker attach "container id"
  
  $mkdir /lab 
  $exit
  ```

- 실행중인 Container 중지하가 : docker stop

  ```powershell
  $docker ps -a 
  $docker start myfedora
  $docker ps #실행중인 container 확인하기
  $docker stop "container name"
  $docker ps #서비스중인 container가 중지되면 여기서 안보인다. 
  ```

- 실행중인 container를 재시작하기 : docker restart

```powershell
$docker ps -a 
$docker start myfedora
$docker ps #실행중인 container 확인하기
$docker attach myfedora
Ctrl+p ctrl+q
$docker ps
$docker restart myfedora
$docker ps|grep myfedora
```