---
layout: post
title:  "CentOS 재설치"
subtitle:   "2021-01-09-study-Centos 재설치"
categories: study
tags: server
comments: true
---



### **CentOS 재설치**

- REF

  [[리눅스/CentOS\] CentOS 7 설치 방법 알아보기!](http://blog.naver.com/anysecure3/221571814401)

  [리눅스 (CentOs) - 개발 놀이터 만들기](https://wikidocs.net/16269) - 위키독스에 정리되어있음!

1. 부팅 USB 만들기

   빈 USB 를 준비한 뒤 설치하고자 하는 버전 찾아서 부팅 USB 를 만들어 준다.

   (구글링 하면 자료도 많고 쉽게 됨)

2. PC 설정을 부팅시 USB 로 부팅 우선으로 설정을 변경변경

   이건 부팅시에 BIOS에서 바꿔줬다. 컴퓨터마다 다르고,, 그때 그때 달라서 구글링 해보고 맞는거 따라 가기.

3. USB 부팅을 통해 ISO파일로 centos 설치시작

   기존 OS 가 있고, 재설치인경우 설치대상을 클릭하여 하드를 비워줄 수 있다. 따로 먼저 삭제를 해주는게 아니라, 설치 과정에서 하드를 비워준다고 이해하자. 이 설치과정에서 IP 설정 등을 해주고 가면 따로 ip 설정을 해주지 않아도 된다. (물론 추후에 부팅 후 연결해도 무방함)

   설치 이후 몇 가지 보안 조치를 따라해주자.