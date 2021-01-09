---
layout: post
title:  "신규 장치 초기 세팅"
subtitle:   "2021-01-09-study-Centos 재설치"
categories: study
tags: server
comments: true
---



### **신규 장비 세팅**

1. IP/ 도메인 신청

   https://ict.kaist.ac.kr/ 에서 할 수 있고, 1~2일 소요됨.

   개인 계정으로 발급 받아도 무관

2. static ip 설정

   서버로 쓸 os 는 보통 DHCP 를 사용하지 않고 정적 IP를 사용한다.

   윈도우의 경우 정보통신팀에서 메뉴얼을 제공하는데, CentOS나 Ubuntu 같은 경우는 구글의 힘을 빌려야 한다.

   "CentOS"(OS명) + "7"(버전명) + static/ 고정 IP 설정

   등으로 접근하면 잘 나온다.  버전마다 설정 방법이 상이하기 때문에 이것을 잘 확인해야한다.

   - REF

     ([Ubuntu 20.04 LTS 고정 IP할당하기. - 달소씨의 하루 (dalso.org)](https://blog.dalso.org/linux/ubuntu-20-04-lts/9069) 여기서 lo, eth0, eth1,,, 등이 있는데 lo 는 루프백이니까 건들이지 말고, eth0 에 적용해주면 된다.

     **$vi /etc/netplan/50-cloud-init.yaml** 라고 본문에서는 나와 있는데

     저 위치에서 ls -a 해서 파일들을 보면 비슷한 파일명을 가진 파일이 있을 것인데, 그것을 열면 된다. 나 같은 경우에는 00-cloud-init.yaml 이었던 것 같다.

     - ip 주소 : 할당받은 대로 ---.---.xxx.xxx
     - netmast : 255.255.255.0
     - broadcast : ---.---.-.255
     - gateway : ---.---.0.1

     `$ifconfig` 나 `$ip addr` 등의 명령어로 ip 설정이 되었는지 확인해준다.

     나는 처음에 뭣 모르고 lo를 수정해줬다가 다시 원상복구 하는 법을 몰라서 한참 해멨다.

     - ip 초기화 명령어 `$ip addr flush lo`

       lo 자리에 eth0을 넣으면 eth0이 초기화 된다.

3. 정보통신팀 연락 후 장치 등록 (맥주소 인증?)

   전화해서 IP 알려드린 후 서버 장치등록 한다고 얘기하면 바로 해주신다.

   여기서 교내 네트워크에 잡혀야 등록이 가능하다.

   만약 해당 ip는 사용중이 아니라고 하신다면, 아래 두가지 이유일 수 있다.

   1. 인터넷 연결이 안 됨

   ( 이 경우에선 PC 자체로도 확인이 가능하다 GUI일 경우,, 이더넷 연결없음 등등)

   1. 설정을 잘 못 해줌 ( 설정파일 )

   설정 파일을 잘못 기입했거나 반영이 안되었을 수 있다. (재부팅 해볼 것)

4. 이후 잘 되는지 ping 해보기

   `ping 8.8.8.8` :  구글로

   `ping gateway 주소` : 게이트웨이로

5. 원격 접속 준비

[putty 를 이용해서 ubuntu 접속하기(원격으로 리눅스에)](https://m.blog.naver.com/gluestuck/221738041134) : 한글 블로그, 근데 좀 예전버전같다.

[How to install Putty SSH client on Ubuntu 20.04LTS](https://vitux.com/ubuntu_putty_ssh/) : 이건 우분투에 클라이언트 설치하기

[UBuntu 20.04 LTS ssh server 설치](https://com1973.tistory.com/493) : 우분투어 ssh server 설치하기 (이걸 해줘야 한당)

[How to Enable SSH on Ubuntu 20.04](https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-20-04/) : 영어버전이지만! 자세한듯.

`$ netstat -atn`  : tcp 22 포트 확인하기!

- netstat, ifconfig, net-tools 가 없다면 설치해준다.
- REF
  - [우분투 포트포워딩 / 포트, 방화벽 설정](http://blog.daum.net/imuooo/7001730)
  - [리눅스 포트 설정](https://fblens.com/entry/리눅스-포트-확인)
  - [우분투 방화벽 UFW 설정](https://webdir.tistory.com/206)

```
[ 포트 포워딩 하는법]
iptables -A INPUT -p tcp --dport 5080 -j ACCEPT 
iptables -A OUTPUT -p tcp --dport 5080 -j ACCEPT 
RTMP: 1935
RTMPT: 8088

HTTP servlet engine: 5080 (Tomcat uses 8080)
Debug proxy port: 1936

[방화벽 켜기]
sudo ufw enable

[방화벽 끄기]
sudo ufw disable // 

[방화벽 특정 포트/프로토콜 개방]
sudo ufw allow (개방할 포트번호) / (프로토콜)
 ex> sudo ufw allow 3306/tcp
 ex> sudo ufw allow 3306/udp

[방화벽 특정 포트/프로토콜 차단]
sudo ufw deny (차단할 포트) / (프로토콜)
 ex> sudo ufw deny 8080/tcp

[방화벽 규칙 제거]
sudo ufw delete (allow/deny) (포트)/(프로토콜)
ex> sudo ufw delete allow 3306/tcp 

[특정 ip 막기]
sudo ufw deny from (아이피 주소)
sudo ufw deny from 192.168.0.100

[utf 상태 보기]
sudo ufw status
```