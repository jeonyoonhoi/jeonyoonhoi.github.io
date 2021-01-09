---
layout: post
title:  "Docker (1)"
subtitle:   "2021-01-09-docker(1)"
categories: study
tags: server
comments: Docker 1. 소개 2. 기본 사용법
---

# 1. Docker 소개

## 1) 가상머신(Virtual Machine)

- 소개 : 다양한 종류의 hypervisor 기술을 사용하면 하나의 물리적인 서버에 여러 가상의 서버를 운영 가능
- 장점 : 하나의 물리적인 server에 여러 개의 가상머신 운영 가능, 이를 통한 자원 활용률 증가, 서버 배포 및 관리가 편리하고 빠르다.
- 단점 : VM 수에 따라 관리해야 할 Server 가 많아지는 것과 동일하다. VM으로 서비스를 하면 물리적인 서버(Host Machine)에 비하여 성능 오버헤드 발생, VM 이 구동되기 까지의 지연시간을 무시할 수 없다. (OS 기동, 서비스 실행 등)

## 2) 컨테이너(Container)

- OS레벨에서 CPU, RAM,Disk Network등의 자원을 컨테이너 단위로 격리하여 할당. 각 Application마다 각각의 고유한 User space를 제공(Application 마다 독립적인 실행환경을 제공)
- 기술적 특성
  - 각 container 들은 OS 의 Kernel을 공유.
  - 각 user space(container) 단위로 root filesystem 보유.
  - 각 container 단위로 systemd를 보유(process 간에 kill 하지 못함)

### VM 과 Contrainer 비교

- VM : Hypervisor로 하드웨어를 가상화 하고, Hypervisor 위에서 Guest OS가 설치된 VM들을 구동
- Container : Host OS(linux)의 커널을 공유하며, Guest OS(vm) 가 따로 필요 없다. VM에 비해 성능손실이 거의 없고, 가볍다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84eb3b54-9eae-4ea3-b927-b201d9286674/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84eb3b54-9eae-4ea3-b927-b201d9286674/Untitled.png)

- 신속성 : 크기가 GB 단위인 VM은 배포하는데 수 분에서 수십분의 시간이 소요되지만, Guest OS 가 없어 MB 단위으이 크기를 가지는 컨테이너는 배포 시간이 수초에 불과.
- 라이선스 비용 절감 : VM의 경우 VM개수만큼 Guest OS 의 라이선스 비요잉 발생하나, 컨테이어의 경우 Host OS 1대의 OS 라이선스 비용만 발생
- 안정성 : VM의 경우, 정확히 할당된 자원 내에서 VM이 운영되기 때문에, 컨테이너에 비해 안정적으로 운영할 수 있으나, 컨테이너들은 OS 커널을 공유하기 때문에, 하나의 컨테이너가 과도하게 자원을 사용할 수 있음.  → 이를 해결하기 위해 실행할 컨테이너에 CPU, Memory 와 같은 자원할당량을 사전에 지정하는 관리 기능을 가지는 도구들(Docker Swarm, Kubernetes)이 있음.

## 3) Docker

- 리눅스 컨테이너에 여러 기능을 추가하여 애플리케이션을 컨테이너로 좀 더 쉽게 사용할 수 있게 만든 것(엔진+툴)
- Docker Inc. 에서 개발한 Go 언어 오픈소스로 2013년도에 발표

## 4) Docker 의 필요성

- 각 프로젝트 별로 disk 공간을 다르게 사용 가능
- 필요한 software 의 손쉬운 배포 및 이식
  - 사용자 환경을 docker 이미지로 만들어 다른 환경에서 문제없이 실행 가능
  - 해당 software v필요한 여러가지 기반 프로그램을 포함하고 있어 기반 프로그램의 버전에도 신경 쓸 필요가 없어짐.

# 2. Docker 기본 사용법

## 1) Docker 설치

- CentOS 7에Docker 설치하기

```jsx
$ sudoyum –y update
$ sudoyum –y installdocker docker-registry
```

- Ubuntu에Docker 설치하기(사전조건: curl 설치)

```jsx
$ curl -s [<https://get.docker.com>](<https://get.docker.com/>) | sudosh
```

- Docker 서비스의자동시작구성여부확인하기

```jsx
$ sudo systemctl list-unit-files| grep docker
```

- 시스템이시작할때자동으로Docker 서비스시작하기

```jsx
$ sudo system ctl enable docker
```

- Docker서비스시작하기

```jsx
$ sudo systemctl start docker
```

- Docker 서비스실행여부확인하기

```jsx
$ sudo systemctl status docker
```

## 2) Docker 정보확인

- 실행된 Docker 서버 및 클라이언트 버전 확인

```jsx
$sudo docker version
```

- Docker 세부정보 확인

```jsx
$sudo docker info
```

## 3) Docker  관리자 추가

- **docker.sock**  파일은 Docker API의 주요한 entry point 가 된다.
- docker daemon 이 Listening 하고 있는 Unix socket
- Docker cli client 는 기본적으로 이 socket을 사용하여 도커 명령어를 실행한다.
- 도커 명령어란 ? docker 로 시작하는 명령어들을 말한다.
- docker.sock 파일의 Permission을 확인하면 어떤 사용자 및 그룹이 Docker daemon을 관리할 수 있는지 알 수 있다.

```jsx
#ls -l /run/docker.sock
#find / - name docker.sock 
```

- /run/docker.sock 파일의 소유 그룹이 docker 이기 때문에 docker 그룹에 사용자를 추가하면 그 사용자는 docker 명령어 실행 가능
- wheel 그룹의 구성원인 adminuser 로 docker 명령어 실행해 보기

```jsx
$id adminuser # 나는 adminuser 대신에 test 로 진행
$su adminuser # 여기도 test로 진행 (user 명)
$whoami
$docker version 
```

→ *Docker daemon socket에 연결하는 동안 /run/docker.sock파일에 대한 permission이 없어서 Docker 서비스에 대한 정보를 불러올 수 없다고 한다.*

### 그룹에 사용자 추가하기

- adminuser 사용자를 docker 그룹에 포함시키기

```jsx
su root
cat /etc/group|grep docker
usermod -aG docker adminuser
cat /etc/group |grep docker
id adminuser
```

- 다시 adminuser 계정으로 docker 명령어 실행하기

```jsx
su adminuser
docker version
docker images
```

*→ adminuser 가 docker 그룹의 구성원이어서 /run/docker.sock 에 대한 permission 이 있어서 **docker로 시작하는 명령어 사용 가능함**을 확인.*

## 4) Docker 이미지 다운로드 및 사용

### Host Os 는 CentOS인데, docker는 최신의 Ubuntu 버전 이미지 다운로드하여 사용하기

- **docker run** : image를 로컬호스트나 docker hub에서 찾아서 Container를 생성한 후 실행중인 container에 접속하는 명령어이다.

  docker를 실행시킨다 == docker container를 실행시킨다.

  - -i : interactive
  - -t : pseudo-tty
  - /bin/bash : 자동으로 실행할 파일

```jsx
docker run -it ubuntu /bin/bash
```

- Docker 버전 확인하기

```powershell
$ hostname -i
$ cat /etc/*-release
$ apt update

$ apt install iputils-ping -y
$ ping 8.8.8.8 -c 3
```

- Docker 에 접속한 상태에서 Host OS 버전 확인하기

```powershell
$ uname -a # OS 커널정보를 확인하는 명령어 - Host O 정보
$ cat /proc/version # 위나 아래 둘 중하나

# 접속한 docker 에서 빠져나온 후 container를 중지하기
$ exit 
```

- image 와 container 확인하기
  - docker images : docker hub에서 다운로드 받은 원본 image
  - docekr ps : 다운로드한 image를 실행한 것(Container), 원본image의 내용이 수정된 것

```powershell
# 다운로드한 docker 이미지 확인하기
$ docker images

# 실행중인 Docker image 인 Container 가 있는지 확인하기
$ docker ps

#로컬 컴퓨터에 실행중인 것과 저장된 컨테이너 모두 확인하기
$ docker ps -a 
# 이경우 실행중인 이미지컨테이너와 다운로드된 것 모두 나옴
```

- 빠져나오기 및 중지

```powershell
# 접속한 docker container 가 계속 실행중인 상태에서 잠시 빠져나오기
CTRL+p, Ctrl+q

# 컨테이너를 중지하고 빠져 나오기
$exit
Ctrl + d
```

- **docker attach** : 실행중인 container에 접속하기 / 중지하기

```powershell
# 실행중인 컨테이너 id 와 name 으로 접속
docker ps
docker attach "container id / container name"

# 잠시 빠져나와서 정보 확인
Ctrl+p, Ctrl+q
$ docker info
$ docker images

# 빠져나와서 실행중인 container wndwlgkrl
$ docker ps
$ docker stop "container id"
$ docker info # 모든 컨테이너가 중지됨
3. Docker 구성요소 및 Docker Image 관리
```

### Docker Container 에 접속하는 두가지 방법

- 로컬 및 인터넷에서 도커 이미지를 찾아서 container 를 시작하면서 접속하기

```powershell
$ docker run -it --name hello_c centos /bin/bash
$ exit
```

- 로컬에 다운로드된 docker image를 실행하여(container) 접속하기

```powershell
#중지되었거나 실행중인 모든 container 확인하기
$ docker ps -a 

#중지된 container 확인하기
$ docker start "container id" (또는 "container name")

# 실행중인 컨테이너에 접속하기 
$ docker attach "container id"
$ exit
```

### Docker 이미지 다운로드 및 사용

- 다양하게 이미지 실행하기

```powershell
$ docker run --name mycon1 centos cat /etc/hostname
#mycon1 : container 이름

$ docker run --name mycon2 centos ping google.com -c 3
$ docker run --name mycon3 centos mkdir /imsidata
$ docker run --name mycon4 -it centos mkdir /imsidata
$ docker run --name mycon5 -it centos /bin/bash
	$ mkdir /imsidata
	$ exit
	
	## 중지된 container를 실행만 하기
	$ docker start mycon5
	
	## 실행중에 container 접속하기	
	$ docker attach mycon5 ls -l /imsidata ##

$ docker run --name mycon6 -it -d cenos /bin/bash
	# -d: --detach(## 백그라운드에서 container 실행함)
	$ docker ps ## mycon6가 실행중임
	$ docker attach mycon6
	$ ls -l /imsidata
		# 이 디렉토리는 존재하지 않는다. 다른 컨테이너에!
	$ exit

$ docker run --name myweb1 -d -p 80:80 httpd
	# -p : --publish 
	# mysql 이나 httpd, nginx와 같은 서비스 이미지를 실행할때는 반드시 -p 옵션을 사용해야 한다. 
	$ docker ps  ## myweb1이 실행중임
	$ curl localhost ## Web service 가 되고 있음 
```

## 5) Docker 기본 사용법 실습

### Centos2에 docker 설치 및 container 실행하기

```powershell
# Docker 설치하고, 실행하기
$curl -sSL <http://get.docker.com|sh>
$systemctl enable docker
$systemctl start docker
$systemctl status docker

# Docker 정보 확인하기
$docker --help
$docker -v
$docker version
$docker info
# 최신의 Ubuntu 버전 Docker Image 다운로드하여 사용하기
$docker run -it ubuntu /bin/bash
$hostname -i
$cat /etc/*-release
$apt-get update
$apt-get install iputils-ping -y
$ping 8.8.8.8 -c 3
$exit

$docker ps
$docker ps -a
$docker images 
$docker info
# 최신의 CentOS 버전 Docker image 다운로드 하여 사용하기 
$docker run -it centos /bin/bash
$ping 8.8.8.8 -c 3
$mkdir /lab
Ctrl+p, Ctrl+q
$docker info|more
$docker images
$docker ps
$docker attach "container id"
$ls -ld /lab/
Ctrl+p Ct4rl+q
$docker ps
$docker stop "container id"

# 작은 용량을 가지고 busybox 및 alpine이미지 사용하기
$ docker run -it busybox /bin/sh
$ exit

$ docker run -it alpine /bin/sh
$ Ctrl+p, ctrl+q

$docker info|more
$docker images
$docker ps
$docker stop "container id"
```