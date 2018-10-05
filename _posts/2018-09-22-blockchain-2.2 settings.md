---
layout: post
title:  "2. 환경설정"
subtitle:   "2.3. Geth로 프라이빗 노드 구축 I (제네시스 블록, 계정 생성)"
categories: study
tags: blockchain
comments: true
---

## 2.3. Geth로 프라이빗 노드 구축 I (제네시스 블록, 계정 생성)

###  Genesis 블록 생성

일단 private node 에 대한 모든 정보를 저장할 수 있는 폴더를 생성

```
PS C:\Users\YOONHOI> mkdir -p ~/Blockchain


    디렉터리: C:\Users\YOONHOI


Mode                LastWriteTime         Length Name                             
----                -------------         ------ ----                             
d-----       2018-09-23   오후 2:01                Blockchain                       



PS C:\Users\YOONHOI> cd Blockchain
```



```
C:\Users\YOONHOI>cd Blockchain

C:\Users\YOONHOI\Blockchain>puppeth
+-----------------------------------------------------------+
| Welcome to puppeth, your Ethereum private network manager |
|                                                           |
| This tool lets you create a new Ethereum network down to  |
| the genesis block, bootnodes, miners and ethstats servers |
| without the hassle that it would normally entail.         |
|                                                           |
| Puppeth uses SSH to dial in to remote servers, and builds |
| its network components out of Docker containers using the |
| docker-compose toolset.                                   |
+-----------------------------------------------------------+

Please specify a network name to administer (no spaces or hyphens, please)
> MyNetwork

Sweet, you can set this via --network=MyNetwork next time!

[32mINFO [0m[09-23|14:11:46.981] Administering Ethereum network           [32mname[0m=MyNetwork
[33mWARN [0m[09-23|14:11:47.062] No previous configurations found         [33mpath[0m=.puppeth\\MyNetwork

What would you like to do? (default = stats)
 1. Show network stats
 2. Configure new genesis
 3. Track new remote server
 4. Deploy network components
> 2

Which consensus engine to use? (default = clique)
 1. Ethash - proof-of-work
 2. Clique - proof-of-authority
> 1

Which accounts should be pre-funded? (advisable at least one)
> 0x

Specify your chain/network ID if you want an explicit one (default = random)
> 4386
[32mINFO [0m[09-23|14:16:42.006] Configured new genesis block

What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> 2

 1. Modify existing fork rules
 2. Export genesis configuration
 3. Remove genesis configuration
> 2

Which file to save the genesis into? (default = MyNetwork.json)
>
[32mINFO [0m[09-23|14:16:59.251] Exported existing genesis block

What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> [35mCRIT [0m[09-23|14:17:43.424] Failed to read user input                [35merr[0m=EOF

C:\Users\YOONHOI\Blockchain>code MyNetwork.json

```



1. Genesis block 을 생성하자. 노드를 초기화하는데 필요함 

- puppeth 라는 Geth 설치할 때 같이 install 되는 이 툴을 사용해서 설치할 것이다. 

- 네트워크 이름 : MyNetwork

- 2번 Genesis 생성하기

- 어떤 합의 엔진을 사용할 것인가 ?  
  - Ethash : proof-of-work
  - Clique : proof-of-authority

- network ID 정하기
  - Public network에서 사용하고 있는 id는 피해야 한다.  (예약어들)
    - 1 :  Main network
    - 2 : 모던 테스트 네트워크(이제 안씀)
    - 3 : Ropsten 테스트 네트워크
    - 4 : Rinkeby 테스트 네트워크
    - 5 :  Kovan 테스트 네트워크
- 제네시스 블록을 json 파일로 추출
- 완료가 되면 MyNetwork.json파일 확인
  - 블록 체인의 내용이 어떤 내용인지
  - 맨 첫번째 블록들의 정보를 나타낸다
  - 이더리움 노드에서 이 파일을 통해 제네시스 블록의 내용을 읽고 체인의 첫 시작을 어떻게 해야하는지 보는 것. 

```json
{
  /*----------------------------
  config : 체인의 파라미터를 정하는데 쓰인다. 
  time stamp : 블록생성의 난이도를 조절하는데 쓰인다. 
  차이가 작으면 난이도가 올라가고 크면 내려간다 
  블록들이 올바른 순서대로 진행되고 있는지 확인
  gas limit :  블록  내트랜잭션이 소비할 수 있는 최대 가스 량
  각 블록마다 처리할 수 있는 트랜잭션을 한도를 둠
  difficulty : 난이도 채굴을 하기 위해 연산을 몇 번을 해야하는지와 직접적 연관이 있음 높을수록 채굴 시간이 길어지고 
  alloc : 지갑 주소에 자금을 미리 할당하는 내용이다. 
  number  : 블록 넘버를 말한다. 제네시스라서 0
  gasUsed : 여러 트랜잭션을 하면서 사용되는 모든 가스의합계
  parentHash :  제네시스는 부모가 없기 때문에 0
  ------------------------------*/
  "config": { /* 생략*/},
  "nonce": "0x0",
  "timestamp": "0x5ba72099",
  "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "gasLimit": "0x47b760",
  "difficulty": "0x80000",
  "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "coinbase": "0x0000000000000000000000000000000000000000",
  "alloc": { /*생략*/  },
  "number": "0x0",
  "gasUsed": "0x0",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```



- 생성한 제네시스 노드를 이용하여 프라이빗 노드를 초기화하자. 

``` geth -datadir . init mynetwork.json```

게스 커맨드를 사용해서 현재 디렉토리 안에 프라이빗 노드 데이터를 저장

```init``` : 프라이빗 노드를 초기화 하기 위한 파일이름을 명시하는데 사용

게스가 실행이되고 제네시스 블록을 사용해서 노드 초기화를 완성





### 새로운 계정 생성

- 앞으로 만드는 새로운 계정들은 keystore에 저장된다. 
- 무작위로 새로운 이더리움 계정을 생성한다 다음 명령어로 

``` geth --datadir . account new```

- 계정의 프라이빗키를 보호하기 휘해 passphrash : bimilbunho

* 세 개의 이더리움 계정을 리스트로 보여주는 커맨드

  ``` geth --datadir . account list```

* 첫 번쨰 계정은 모든 채굴 보상금이 들어오게 된다. 



프라이빗 노드를 초기화 하기 위한 제네시스 블록 및 계정 생성 끝!



## 4. Geth로 프라이빗 노드 구축  II (노드 첫 실행, DAG 파일 생성)

노드초기화를 헀으니 노드를 실행하자

스크립트를 만들어서 파라미터 지정이 필요하다. 

nodestart.cmd 라는 파일을 만들장

```code nodestart.cmd``` 

```cmd
geth --networkid 4386 --mine --minerthreads 2 --datadir "./" --nodiscover --rpc --rpcport "8545" --rpccorsdomain "*" --nat "any" --rpcapi eth,web3,personal,net --unlock 0 --password ./password.sec
```

한 줄씩 설명하면

- geth 
- --networkid 4386 : 네트워크 식별자 파라미터 
- --mine : 이 노드에서 채굴을 시작하도록
- --minerthreads 2 몇개의 스레드에서 채굴? (코어수에 맞춰서)
- --datadir "./" : 우리 채인 파일을 어디에 저장? 현재 디렉토리
- --nodiscover  : 탐색 프로토콜 해지 다른노드가 우리노드에 연결하는 것을 막는다. 테스트 용으로 노드를 사용할때 붙여준다. 
- --rpc :rpc endpoint 를 사용하면 geth로 싨행된 노드에 연결할 수있다. 메타마스크 같은 곳에서! 
- --report "8545"  : 어떤 포트에 접속해야 하는지 명시하는 명령어
- --rpccorsdomain "*" : 아무 도메인에서나 우리의 rpc endpointt에 접근할 수 있도록 *을 설정
- --nat "any" : 네트우ㅝ크에 쓰이는 파라미터
- --rpcapi eth,web3,personal,net  : 어떤 api를 사용할것인지 명시해서 나중에 노드에서 마치 라이브러리처럼 쓸 수있게 한다. 
- ---unlock 0 : 첫번째 노드가 보상을 받기 위해서는 (index 0) 우선 계정 unlock  
- --password ./password.sec : 비밀번호를 담고 있는 파일을 명시 

##### 무조건 첫 번째 줄에 다같이 있어야 한다!

```  code password.sec``` 파일을 만들어서 bimilbunho를 넣어주자



#### 노드를 실행하자

```.\nodestart.cmd``` 하면

#### ERROR!!

> Fatal: Error starting protocol stack: listen tcp 127.0.0.1:8545: bind: Only one usage of each socket address (protocol/network address/port) is normally permitted.

아.. 뭐래...  

각 소켓 주소의 오직 한 가지 사용만이 허용됩니다...??

<b> 해결 !! 가나슈 프로그램 종료했더니 해결되었다.</b>

아마도... 가나슈가 사용하고 있는 RPCSERVER 의 포트도 8545 였는데 둘이 충돌을 한것이 아닐까... 하는 추측

 

```Generating DAG in progress```

- DAG : directed Acyle Graph (방향성 비순환 그래프)

  노드가 채굴을하기 위해서는 먼저 DAG 파일이 있어야 한다. 
  - 30000block = 1 epoch
  - 1 epoch 마다 DAG 파일을 생성해쥰당
  - DAG 파일은 AppData>Ethrach 에 저장된다 
  - epoch 0,1 의 percent = 99!! 까지 다 꽉차면 끗

- 한번 DAG 가 생성되면 다음 실행시에는 바로 채굴부터 합니당!



```cmd
INFO [09-23|16:00:45.825] Successfully sealed new block            number=45 sealhash=41917b…f7c7c0 hash=f58048…3fc804 elapsed=1.974s
INFO [09-23|16:00:45.837] 🔗 block reached canonical chain          number=38 hash=f0619a…6ecd7d
INFO [09-23|16:00:45.868] 🔨 mined potential block                  number=45 hash=f58048…3fc804
INFO [09-23|16:00:45.878] Commit new mining work                   number=46 sealhash=434fce…b1096f uncles=0 txs=0 gas=0 fees=0 elapsed=40.891ms
```



- 트랜잭셔닝 없어도 블록채굴은 한다. 
- 목적 : 트랜잭션 처리/ 새로운 ETH 생성 



정리하자면~

1. Genesis Block 생성
2. 계정생성
3. cmd 파일 잘 만들기
4. DAG 진행
5. 첫 노드 실행 성공





## 2.4. Geth로 프라이빗 노드 구축 III (Geth 콘솔)

#### 백그라운드로 계속 돌고있는 노드에 연결시켜서 자바스크립트 콘솔을 열어보자

```cmd
 C:\Users\YOONHOI> geth attach ipc:\\.\pipe\geth.ipc
Welcome to the Geth JavaScript console!

instance: Geth/v1.8.15-stable-89451f7c/windows-amd64/go1.10.3
coinbase: 0x0eccd56117549800158d7ec9c7e09f53d9483a30
at block: 78 (Sun, 23 Sep 2018 16:05:52 KST)
 datadir: C:\Users\YOONHOI\Blockchain
 modules: admin:1.0 debug:1.0 eth:1.0 ethash:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0
```

- coinbase : 우리가 만든 계정중 맨 처음에 만든 계정

- module : 이 창에서 쓸 수 있는 api를 나열한 것

  nodestart.cmd 에서 파라미터로 rpcapi 에서 설정했던것 - 콘솔에서 쓸 수 있다. 

```

> eth.coinbase
"0x0eccd56117549800158d7ec9c7e09f53d9483a30"
> eth.accounts
["0x0eccd56117549800158d7ec9c7e09f53d9483a30", "0x660632a676804d4363268dca823cdf8090de59e4", "0x1fe4ab19ec99e53867501f9915b6e8774b5c271f", "0xe6e5e2ab8374d944b0d51607d5990fb4b73ec619"]
> eth.getBalance(eth.accounts[1])
0
> eth.getBalance(eth.accounts[0])
345000000000000000000
```



- coinbase 계정을 볼 수 있다. 
- 현재 노드에 연결된 모든 계정들이 나온다. 
- 계정들이 얼마나 이더가 있는지 잔액 확인



```
> web3.fromWei(eth.getBalance(eth.coinbase),"ether")
372
> miner.stop()
null
> miner.start(2)
null
```

- coinbase 잔액을 보면 ether 가 나와있는데 이 금액이 wei 단위이다. 
- web3를 이용하여 wei 를 ether로 변환해서 보자
- 채굴을 멈춘다
- 채굴을 다시 시작한다. (2)  몇개의 스레드에서 할것인지?



```
> personal.unlockAccount(eth.accounts[1],"bimilbunho",200)
true
>
> eth.sendTrasaction({from:eth.coinbase, to:eth.eth.accounts[1], value:we
b3.toWei(20,"ether")})
```

- 계정 unlock은 private key 
- 디폴트는 lock 되어 있다. 
- 두번째로 만든 계정을 unlock 해보자
- 200은 시간



```cmd
> eth.sendTransaction({from:eth.coinbase, to:eth.accounts[1], value:web3.
toWei(20,"ether")})
"0x47f84eb26eeb3698437e0166cc9cd83e914e46aaca5c70b6d977ce672f7c8c71"
```

- 송금 : 첫 번째에서 두 번째 계정으로 이더를 보내보자. 





