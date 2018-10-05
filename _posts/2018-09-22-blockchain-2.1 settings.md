---
layout: post
title:  "2. 환경설정
subtitle:   "2.1. 이더리움 DAPP 개발환경 셋업 I (Geth, 가나슈, 노드.js, 트러플)"
categories: study
tags: blockchain
comments: true
---

## 2.1. 이더리움 DAPP 개발환경 셋업 I (Geth, 가나슈, 노드.js, 트러플)

이더리움 dapp과 스마트 컨트랙트를 개발하기 위한 개발환경 셋팅



### Geth (게스)

- Go Ethereum의 약자
- 풀 이더리움 노드를 내 로컬 환경에서 cmd를 통해 실행시키는 프로그램
- 사실 geth가 저수준에서 작동하는것이라 쉽게 쓸 수 있는 가냐슈나 트러플보다 귀찮고 오래걸림.



- 파이어 폭스나 크롬을 사용. (호환성 문제)

- [다운로드](https://geth.ethereum.org/downloads/)

- setup파일에서 development tools 꼭 체크 해주기
- (만약 오류가 난다면 패스 문제일 것이다. (환경변수) )



- 설치가 끝난다면 power shell을 실행해서 
- ``` >>>geth version``` 을 입력하고 설치가 잘 되었는지 확인

``` 
PS C:\Users\YOONHOI> geth version
Geth
Version: 1.8.15-stable
Git Commit: 89451f7c382ad2185987ee369f16416f89c28a7d
Architecture: amd64
Protocol Versions: [63 62]
Network Id: 1
Go Version: go1.10.3
Operating System: windows
GOPATH=
GOROOT=C:\go

PS C:\Users\YOONHOI> 
```



### Ganache(가나슈)

[ganache download](https://truffleframework.com/ganache)

홈페이지에서 최신버전을 받아도 된다.

강의에서는 안정성을 위해 조금 낮은 버전 설치를 진행했다. 



- 보기 좋은 UI 를 제공하는 이더리움 블록체인 툴
- 사용자 친화적
- Test RPC 업그레이드 버전
- 이더리움 노드를 인 메모리로 돌려서 빠르게 트랜잭션을 처리하도록 함. 
- 계정이 미리 생성되어 있기에 , smart contract를 테스트 하기에는 최적의 툴



- current block : 노드에서 채굴한 마지막 블록 넘버를 말한다.

- gas price :  노드가 트랜잭션을 채굴하기 위한 최소한의 가스 가격

- gas limit : 트랜잭션을 무사히 마치기 위해 필요햔 최소한의 가스 

- network id : 가나슈 서버의 내부 블록체인 식별 아이디가 ~로  초기화 되어서 이더리움 노드를 실행하는 것

- RPC server : 게스나 메타마스크에서 이 주소로 연결하면 가나슈 환경을 그대로 쓸 수 있게 해주는 것

- mining status : 새로운 블록을 채굴하는 속도 (디폴트로 자동 채굴하게 되어있어서 , 테스트 하기 편하라고 이렇게 되어 있음 , 이 기능을 끄고 채굴단위를 초로 설정할 수도 있고 ㅎㅎ?)

- MNEMONIC(님모닉) : 한국어로는 연산기호? 여러 단어들의 조합. 이 단어의 조합을 사용해서 아래에 있는 계정들을 생성하는 비밀문자. 이 문장을 이용해서 메타마스크 같은 곳에서 가나슈 계정을 쉽게 옮길 수 있도록 도와준다. 가냐슈 설치시 랜덤으로 생성되는데 맘대로 바꿀 수도 있다. 

  ```
  shaft clap gun expire course crouch magnet fumace grant shop used vacant
  ```

  => 키 세팅하는 곳 가서 이렇게 바꿔주자

- tx count : 이 계정이 몇번 트랜잭션 했는지

- 인덱스 : : 이 계정이 몇번째 인덱스인지!

- 키 모양 : 계정의 비밀키 볼 수 있음



<BLOCKS>  현재 노드가 채굴한 노드가 몇개인가? 

<Transcations> 이 노드에서 진행한 트랜잭션 리스트를 볼수있음

<LOGS> 현재노드의 모든 로그를 볼 수 있다. 



### Node.js & npm 



(기존 설치가 되어있어 upgrade만 진행)



### Truffle

v.4 이상으로 설치해줄 것

만약 그 이하라면 다음 명령어로 삭제 

``` 

```

트러플 설치 후 버전 확인

```
PS C:\Users\YOONHOI> npm install -g truffle
C:\Users\YOONHOI\AppData\Roaming\npm\truffle -> C:\Users\YOONHOI\AppData\Roaming\npm\node_modules\truffle\build\cli.bundled.js
+ truffle@4.1.14
updated 4 packages in 7.042s

PS C:\Users\YOONHOI> truffle version
Truffle v4.1.14 (core: 4.1.14)
Solidity v0.4.24 (solc-js)
```



## 2.2. 이더리움 DAPP 개발환경 셋업 II (비쥬얼 스튜디오 코드, 메타마스크)

### VIsual studio code

- Cross platfoam 지원 - mac, window, linux 다 돌아감
- git 제어 등 여러 편리한 기능 포함하고 있음 



#### 설정언어 변경

```ctrl``` + ```shift``` +```p```  

'display' 검색

json 파일을 열고 language = 'ko' 로 바꿔줌



#### solidity 설치

언어를 서포트 해주는 익스텐션



### MetaMask

- 이더리움 개인 지갑
- 댑의 모든 트랜잭션을 메타마스크를 통해 처리할 예정
- [링크](http://metamask.io)
- 크롬의 브라우저 인스텐션으로 제공됨
- 인퓨라(infura)를 통해 원격네트워크를 통해 이더리움에 접속한다. 

* seeds pleasure

가나슈 실행했을 때 님모닉이라는 단어 조합(계정 옮길 때 사용)

앞으로 내가 메타마스크에서 사용할 계정들을 복구할 때 사용

절대 잃어버리면 안됨





#### 가나슈 메타마스크 연결

1. 가나슈 님모닉을 사용해서 메타마스크의 계정도 옮겨오기!

- 가나슈 실행
- RPC Server 변경 -- 메타마스크에 연결하기 쉽게
  - HOSTNAME :  localhost (나는 이게 변경이 안된다...)
  - PORT NUMBER : 8545 
- 메타마스크 오른쪽 상단의 네트워키
  - 이더리움 메인넷 : 진짜 ETH를 쓰는 곳
  - 테스트넷 : 환경은 메인넷과 동일
  - 이름이 로컬호스트, 포트가 8545 이더리움 노드에 접속하게 해준다. 

- 로컬호스트 8545 선택하면 가나슈에 연결이 된 것이다. 
- 가나슈 님모닉 복사해서 로그인 창에서 연결
- 새로운 계정이 뜨고 100 이더, 가나슈번째 주소랑 같은 점 확인
- 테스트 네트워크에서 연결된상태에서 클릭하면 랜덤으로 계정을생성한다. 



2. private key 를 사용해서

- 메타마스크 > 사용자 모양 > =import account
- 가나슈에서 복사한 private key를 넣어준다.
- seeds 써서 복구할때 여기서 생성된건 복구되지 않는다. 


