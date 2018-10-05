---
layout: post
title:  "4. Solidity smart contract - 트러플 & 컨트랙 배포"
subtitle:   "솔리디티 스마트 계약 실습"
categories: study
tags: blockchain
comments: true
---

## 4.5 트러플 & 컨트랙 배포 (구조설명, 배포, 트러플 콘솔 사용)

Truffle Framework : 개발속도를엄청 높여준다. 

이더리움 스마트 컨트랙을 사용한 분산 어플리케이션 개발에 가장 많이 쓰이는 프레임워크



설치는 이미 되어있으니 powershell 열고 blockchain 안에 truffle 폴더를 만들고 init 으로 초기화를 하자

```

C:\Users\YOONHOI\Blockchain>mkdir -p truffle

C:\Users\YOONHOI\Blockchain>cd truffle

C:\Users\YOONHOI\Blockchain\truffle>truffle init
Downloading...
Unpacking...
Setting up...
Unbox successful. Sweet!

Commands:

  Compile:        truffle compile
  Migrate:        truffle migrate
  Test contracts: truffle test

C:\Users\YOONHOI\Blockchain\truffle>code .
// 생성된 프로젝트를 열자 
```



* condtracts : 솔리디티 컨트랙트를 저장

* migrations : 배포시 migrations 안의 script 파일들을 실행.

  - 배포할 때 중요한 파일들이 들어있다. 

* script : 배포 과정의 로직이 담겨져 있다. 

  - 1 _  : 배포시 접두사로 붙여진 숫자에 따라 순차적으로 실행

  - migration contract 를 노드에 배포하는역할을 한다. 

* test : 여기의 파일들은 컨트랙을 테스트하는데 사용한다. 

* truffle.js 파일은 환경설정을 담당한다. 

  - 어느 네트워크에 배포할지 나중에 여기에 정의한다 

* truffle-configue.js 파일은 cmd 를 이용하는사람들을 위해 존재

  - truffle.js 의 이름을 재변경해서 갖고 있는 것이다. 
  - cmd사용할때 충돌할 수 있어서 (powershell은 괜찮)



#### truffle develop

트러플 내부에서 이더리움 노드를 실행시키고

테스트용 계정 10개 생성

밑의 트러플 자바스크립몬솔을 실행시킨다. 

```
PS C:\Users\YOONHOI\blockchain\truffle> truffle develop
Truffle Develop started at http://127.0.0.1:9545/

Accounts:
(0) 0x627306090abab3a6e1400e9345bc60c78a8bef57
(1) 0xf17f52151ebef6c7334fad080c5704d77216b732
(2) 0xc5fdf4076b8f3a5357c5e395ab970b5b54098fef
(3) 0x821aea9a577a9b44299b9c15c88cf3087f3b5544
(4) 0x0d1d4e623d10f9fba5db95830f7d3839406c6af2
(5) 0x2932b7a2355d6fecc4b5c0b6bd44cc31df247a2e
(6) 0x2191ef87e392377ec08e7c08eb105ef5448eced5
(7) 0x0f4f2ac550a1b4e2280d04c21cea7ebd822934b5
(8) 0x6330a553fc93768f612722bb8c2ec78ac90b3bbc
(9) 0x5aeda56215b167893e80b4fe645ba6d5bab767de

Private Keys:
(0) c87509a1c067bbde78beb793e6fa76530b6382a4c0241e5e4a9ec0a0f44dc0d3
(1) ae6ae8e5ccbfb04590405997ee2d52d2b330726137b875053c36d94e974d162f
(2) 0dbbe8e4ae425a6d2687f1a7e3ba17bc98c673636790f1b8ad91193c05875ef1
(3) c88b703fb08cbea894b6aeff5a544fb92e78a18e19814cd85da83b71f772aa6c
(4) 388c684f0ba1ef5017716adb5d21a053ea8e90277d0868337519f97bede61418
(5) 659cbb0e2411a44db63778987b1e22153c086a95eb6b18bdf89de078917abc63
(6) 82d052c865f5763aad42add438569276c00d3d88a2d062d36b2bae914d58b8c8
(7) aa3680d5d48a8283413f7a108367c7299ca73f553735860a87b08f39395618b7
(8) 0f62d96d6675f32685bbdb8ac13cda7c23436f63efbb9d07700d8669ff12b7c4
(9) 8d5366123cb560bb606379f90a0bfd4769eecc0557f1b362dcae9012b548b1e5

Mnemonic: candy maple cake sugar pudding cream honey rich smooth crumble sweet treat

?좑툘  Important ?좑툘  : This mnemonic was created for you by Truffle. It is not secure.
Ensure you do not use it on production blockchains, or else you risk losing funds.
```



#### truffle develop --log

- 현재 실행중인 트러플 내부의 노드와 연결

```
PS C:\Users\YOONHOI> cd .\blockchain\truffle

PS C:\Users\YOONHOI\blockchain\truffle> truffle develop --log
Connected to existing Truffle Develop session at http://127.0.0.1:9545/
```



#### migrate

migrate를 실행

방금 truffle ethereum node 에 두 개의 컨트랙트를 배포했다. 

- 1_initial_migration.js => 이 스크립트 파일을 실행, 안에 있는 컨트랙트 배포
- 2_deploy_contracts.js => 이 안에 있는 MyContract 배표



````

truffle(develop)> migrate
Compiling .\contracts\Migrations.sol...
Compiling .\contracts\MyContract.sol...
Writing artifacts to .\build\contracts

Using network 'develop'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0x14bd51b7b2cc87e6296fa1ac12fba0a89f229f848475ad4946eedd9556904056
  Migrations: 0x8cdaf0cd259887258bc13a92c0a6da92698644c0
Saving successful migration to network...
  ... 0xd7bc86d31bee32fa3988f1c1eabce403a1b5d570340a3a9cdba53a472ee8c956
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying MyContract...
  ... 0xfbde8f4b61ed9b3c1157c2bb26216d18c4364be07f37da366d2d9dda114adf22
  MyContract: 0x345ca3e014aaf5dca488057592ee47305d9b3e10
Saving successful migration to network...
  ... 0xf36163615f41ef7ed8f4a8f192149a0bf633fe1a2398ce001bf44c43dc7bdda0
Saving artifacts...
truffle(develop)>

````

```Migrations: 0x8cdaf0cd259887258bc13a92c0a6da92698644c0```

migrations... 아래 정보가 해시, 어느 주소로 배포가 되었는지 알려주는 것

```sol
pragma solidity ^0.4.23;

contract Migrations {
  address public owner;
  uint public last_completed_migration;

  constructor() public {
    owner = msg.sender;
  }

  modifier restricted() {
    if (msg.sender == owner) _;
  }

  function setCompleted(uint completed) public restricted {
    last_completed_migration = completed;
  }

  function upgrade(address new_address) public restricted {
    Migrations upgraded = Migrations(new_address);
    upgraded.setCompleted(last_completed_migration);
  }
}

```



```  uint public last_completed_migration;```

가스비용절감, 기존 컨트랙트를 또 배포하지 않기 위해서 있는 장치

블록체인은 수정이 불가능하기 때문에 같은 주소에 배포할 수 없다. 



```

truffle(develop)> migrate
Using network 'develop'.

Network up to date.
```



#### 개발 과정에서는 수정해서 배포할 필요성이 있음

개발중에서 컨트랙트 해보고 싶을 때 ```migrate --compile-all --reset``

- all : 모든 컨트랙트를 재 컴파일 시키고

- reset : migrations 폴더안에있는 스크립트를 강제적으로 다시 실행시킨다. 

  =>새로운 주소에 재컴파일된 컨트랙트가 네트워크에 배포된다. 

  mirate 명령어를 쓰면build 라는 폴더가 생긴다.

  contract 안에 2개의 json 파일이 생김 ==> artifact 라고 부르는 파일이 생겨난다.

  artifact는 해당 컨트랙트의 abi 정보와 관련된 정보를 담고 있음

  블록체인에 컨트랙을 배포했을때 그 함수를 호출하고 데이터가 내가 원하는 형식으로 돌아올 수 있도록 보장하고, 컨트랙트와 상호작용 할 수 있는 것을 규정하고 있음 


```
truffle(develop) > web3.eth.accounts

truffle(develop) > web3.fromWei(web3.eth.getBalance(web3.eth.accounts[0]),"ether")

truffle(develop) > web3.fromWei(web3.eth.getBalance(web3.eth.accounts[0]),"ether").toNumber()

truffle(develop) > web3.fromWei(web3.eth.getBalance(web3.eth.accounts[1]),"ether").toNumber()

truffle(develop) >
MyContract.deployed().then(function(instance) { app = instance;})
//만약 MyContract 가 deployed () 되었다면, 그러면 instance를 받는 콜백함수 실행

//setStudentInfo로 학생 이름 넣기
truffle(develop) >
app.setStudentInfo(1111,"sejong", "male",7, {from:web3.eth.accounts[1]})
// 트랜잭션 성공하면 recipts 받을 수 있다. 

truffle(develop)> web3.fromWei(web3.eth.getBalance(web3.eth.accounts[1]),"ether").toNumber()
99.9829656
//트랜잭션이 처리되었으므로 잔액이 줄어든 것을 확인할 수 있다. 

//getStudentInfo로 학생 이름 얻기
truffle(develop) > app.getStduentInfo(1111)

.exit
로그창도  컨트롤씨 두번!
```



### (가나슈 사용)

비쥬얼적으로 쉽게 진행사항을 볼 수 있다.

truffle과 같이 쓰면 최고의 콤보



#### 트러플에서 가나슈 네트워크에 연결

- 환경설정

  일단 ```truffle.js``` 파일 수정

  ```js
  module.exports = {
    networks : {
      ganache: {
        host : "localhost",
        port : 0545,
        network_id : "*"
      }
    }
  };
  ```


- 컨트랙트를 가나슈 노드에 배포하기

  ```truffle migrate --compile-all --reset --network ganache```



  (트러플 콘솔에 접속하지 않은 상태에서 바로 배포하기)

  ```
  
  PS C:\Users\YOONHOI\blockchain\truffle> truffle migrate --compile-all --reset --
  network ganache
  Compiling .\contracts\Migrations.sol...
  Compiling .\contracts\MyContract.sol...
  Writing artifacts to .\build\contracts
  
  Using network 'ganache'.
  
  Running migration: 1_initial_migration.js
    Deploying Migrations...
    ... 0x14bd51b7b2cc87e6296fa1ac12fba0a89f229f848475ad4946eedd9556904056
    Migrations: 0xea865999f00e4621b64d187447e27b253ca3d30d
  Saving successful migration to network...
    ... 0xa635cae46e0ca04f1ff9ad5ce40f5cc0d120a27d6b692c2b7982a959c15441bf
  Saving artifacts...
  Running migration: 2_deploy_contracts.js
    Deploying MyContract...
    ... 0xfbde8f4b61ed9b3c1157c2bb26216d18c4364be07f37da366d2d9dda114adf22
    MyContract: 0xb49e20d6abb5430c43522ad7bf1886d3ef8622c8
  Saving successful migration to network...
    ... 0x43f6a55bf77d7f20f2f600c92ea3aa9e2d050b815e39612acb40b3dbca3d36b6
  Saving artifacts...
  ```

- 컴파일 되면서 컨트랙트 폴더 안에 있는 아티팩트 파일들은 업데이트 된다. 

- mycontract.js 파일을 열어보면 id 5777 (새로운 네트워크 아이디) 가 새로 생성된 것을 볼 수 있다. 

- 가나슈 창에서 보면 맨 처음 트랜잭션이 깎여있는 것을 알 수 있다. 



```contract creation```두개의 컨트랙트를 배포하면서 쓰인 트랜잭션

```contract call``` migration contract의 lastcommpleted migration 변수 값을 업데이트 하면서 쓰인 트랜잭션



#### 트러플 콘솔에 들어가서 가나슈와 의사소통 해봅시다

- 마이컨트랙트의 인스턴스를 전역변수에 저장.

  ```
  
  truffle(ganache)> app.setStudentInfo(1111,"name1","fe",40,{from : web3.eth.accounts[1]})
  { tx: '0x632bb343b8b095fe08d7d49f8b07aabb44aaf24c806a488e4ad8b57a22fc3848',
    receipt:
     { transactionHash: '0x632bb343b8b095fe08d7d49f8b07aabb44aaf24c806a488e4ad8b57a22fc3848',
       transactionIndex: 0,
       blockHash: '0x9887417c2d46cefbbb8cb46a21ebd97e5ae61e1c313c17f4dc55b5f0f0c8eef1',
       blockNumber: 5,
       gasUsed: 85044,
       cumulativeGasUsed: 85044,
       contractAddress: null,
       logs: [],
       status: '0x01',
       logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000' },
    logs: [] }
  ```


- 가나슈 보면 계정의 이더가 줄었을 것이고, Transaction봐도 알 수 있다. 

```
truffle(ganache)> app.getStudentInfo(1111)
[ 'name1', 'fe', BigNumber { s: 1, e: 1, c: [ 40 ] } ]
```



트러플은 빌드와 배포를 간편하게 해준다. 

배포하는사이트를 확인한다.

 트러플 콘솔을 통해 컨트랙트와 소통할 수 있다. 

가나슈와 같이 사용하면 비쥬얼적인 툴을이용할 수 있다. 