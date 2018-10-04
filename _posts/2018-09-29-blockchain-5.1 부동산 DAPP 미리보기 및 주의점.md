---
layout: post
title:  "이더리움 부동산 스마트 계약 개발"
subtitle:   "전부"
categories: study
tags: blockchain
comments: true
---

## 5.1. 부동산 DAPP 미리보기 및 주의점

스마트 계약 적용 가능 사례

부동산 시나리오(예시)

- 부동산 중계인 통해 매수의사전달
- 계약 요청
- 상호동의
- 계약 완료
- 대금 지불
- 명의 이전
- 등

단계별로 블록체인에 영구 저장

거래에 대한 내용들을 추적하면 한번에 나오기 때문에, 블록에 저장된 그 자체만으로도 효력을 지닌다. 

시스템만 잘 갖춰져 있다면 매수인과 파는 인의 스마트 계약과 어플리케이션을 통해 중개인 없이 해결할 수 있을 것이다.



bUT

우리가 만드는 것은 간단한 사이클 예제!



매물을 소유한 사람이 매물을 올린다.

다른 유저들이 매물을 골라서 자신의 정보를 입력하고

매입가를 트랜잭션을 통해 송금하면 

매물을 소유하게 된다. 



#### 주의점

처음부터 끝까지 다 블록체인에 저장하는 것이 아니다.

블록체인에는 꼭 필요한 내용만 저장하고 나머지는 관계형 데이터베이스 등 다른 곳에 저장해야 함. 

블록체인 저장 자체가 비용을 필요로 한다. 

새로운 매물이 나올 때마다 매물정보를 블록체인에 기록해야 한다면 시간도 오래걸리고, 비용도 걸린다.

만약 퍼포먼스가 중요하다면 다른 것을 채택하는 것이 좋다. 

매입자의 정보, 매입 id 등 아주 중요한 정보만 블록체인에 저장해야 한다. 



#### 결론

모든 부분에 무조건 블록체인 고집 X

 



## 5.2 스타터 템플렛 받기

``` truffle init```

빈 프로젝트를 생성했다.

댑개발의 아주 기초적인 환경만 주어진다. 

앱 개발하면서 많이 사용하는 것들이 미리 추가되어 있다면 편리할 것이다. 곧바로 개발에 들어갈 수있도록.

#### Truffle Box

댑 개발에 필요한 정형화된 표준 템플릿을 다운 받을 수 있는 곳

[Truffle boxes](https://truffleframework.com/boxes)

내가 원하는 환경에 맞추어 다운받으면 된다.

js와 j Query 를 이용하는 템플릿을 받아보자. 

(강좌용 템플릿은 공식은 아니고 수정된 것)

```

PS C:\Users\YOONHOI\blockchain> mkdir -p real-estate


    디렉터리: C:\Users\YOONHOI\blockchain


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----     2018-09-29   오후 9:01                real-estate


PS C:\Users\YOONHOI\blockchain> cd real-estate
PS C:\Users\YOONHOI\blockchain\real-estate> truffle unbox kkagill/rreal-estate-starter
Downloading...
Error: Truffle Box at URL https://github.com/kkagill/rreal-estate-starter doesn't exist. If you believe this is an error, please contact Truffle support.
    at Request._callback (C:\Users\YOONHOI\AppData\Roaming\npm\node_modules\truffle\build\webpack:\packages\truffle-box\lib\utils\unbox.js:50:1)
    at Request.self.callback (C:\Users\YOONHOI\AppData\Roaming\npm\node_modules\truffle\build\webpack:\~\request\request.js:185:1)
    at emitTwo (events.js:126:13)
    at Request.emit (events.js:214:7)
    at Request.<anonymous> (C:\Users\YOONHOI\AppData\Roaming\npm\node_modules\truffle\build\webpack:\~\request\request.js:1157:1)
    at emitOne (events.js:116:13)
    at Request.emit (events.js:211:7)
    at IncomingMessage.<anonymous> (C:\Users\YOONHOI\AppData\Roaming\npm\node_modules\truffle\build\webpack:\~\request\request.js:1079:1)
    at Object.onceWrapper (events.js:313:30)
    at emitNone (events.js:111:20)
PS C:\Users\YOONHOI\blockchain\real-estate> truffle unbox kkagill/real-estate-starter
Downloading...
Unpacking...
Setting up...
Unbox successful. Sweet!

Commands:

  compile: truffle compile
  migrate: truffle migrate
  test:    truffle test
PS C:\Users\YOONHOI\blockchain\real-estate> code .

```

- src 폴더 : 댑의 프론트앤드를 담당하는 구조를 설정
- truffle, web3 =>노드와 소통하기 위함
- index.html => 프론트 뷰를 담당
- real-estate.json => 유저들에게 리스트로 보여지게 된다. 



## 5.3 컨트랙 소유자 설정 

contracts> RealEstate.sol 을 열어주세요

- 생성자를 만듭시다. 

  - 생성자의 역할 : 클래스의 맴버 필드 초기화
  - 특징 : 배포할 때 딱 한번만 실행된다
  - 즉, 특징을 이용해서 컨트랙트의 소유자(배포할 때 사용된 계정)을 설정. 
  - 코드

  ```js
  pragma solidity ^0.4.23;
  
  contract RealEstate {
      address public owner; //getter자동으로 만들어짐
  
      constructor() public { 
          owner = msg.sender;  
      }
  }
  
  ```

   ```owner = msg.sender;```

  - msg.sender의 주소값이 상태변수 owner에 들어간다
  - msg.sender : 현재 사용하는 계정으로 이 컨트랙 안에 있는 생성자나 함수를 불러오는 것, 값은 주소값을 지님, 
  - 이 경우에 이 컨트랙트에 주인은 배포하는데 쓰인 계정이다라고 선언하게 된다. 

- **가나슈 실행**해서 노드에 컨트랙을 배포해보자

-  +오너 주소가 잘 저장되었는지 확인해보자

- powerShell 에서 먼저 배포하자 

  ```truffle migrate --network ganache```

  ```js
  PS C:\Users\YOONHOI\blockchain\real-estate> truffle migrate --network ganache
  Compiling .\contracts\Migrations.sol...
  Compiling .\contracts\RealEstate.sol...
  Writing artifacts to .\build\contracts
  
  Using network 'ganache'.
  
  Running migration: 1_initial_migration.js
    Deploying Migrations...
    ... 0x760c9a8b0141402db629cbc8df3c58a2a3c4f8f4e319d84f9a0ac25525b3ecf9
    Migrations: 0xea865999f00e4621b64d187447e27b253ca3d30d
  Saving successful migration to network...
    ... 0xa635cae46e0ca04f1ff9ad5ce40f5cc0d120a27d6b692c2b7982a959c15441bf
  Saving artifacts...
  Running migration: 2_deploy_contracts.js
    Deploying RealEstate...
    ... 0x94515ecc9fc570722782d918997772295feefbdd56b75377bacde7d157ad1e4d
    RealEstate: 0xb49e20d6abb5430c43522ad7bf1886d3ef8622c8
  Saving successful migration to network...
    ... 0x43f6a55bf77d7f20f2f600c92ea3aa9e2d050b815e39612acb40b3dbca3d36b6
  Saving artifacts...
  ```

  이 경우 가나슈 보면 디폴트로 첫 계정에서 밸런스가 줄었음을 확인 가능

- ```truffle console --network ganache``` 트러플 콘솔로 들어가자

- ```truffle(ganache)> RealEstate.deployed().then(function(instance) {app = instance;})``` 앱 인스턴스 생성!

- ```app.owner.call()``` 앱 변수를 이용해서 소유자는 누구일까 확인해보자


## 5.4 첫 테스팅 

- 첫 테스트 스크립트 작성
- 블록체인에 스마트 컨트랙트를 배포하면 수정이 불가능 하기 때문에 최대한 많은 테스팅 후에 배포해야 한다. 
- test 폴더에 TestRealEstate.js



- 모카 테스팅 프레임워크 어설션은 차이를 사용
- 이미 많이 검증된 프레임워크를 사용한다. 

```js
var RealEstate = artifacts.require("./RealEstate.sol");

//컨트랙트를 테스트 할 것인데 두 개의 인자(이름, 함수를 받음)
//계정이라는 이름을 콜백으로 받는 함수 
contract('RealEstate',function(accounts) {
    var realEstateInstance;
    //전역변수 선언 

    //it()을 통해 무슨 내용의 테스트를 할것인지 정의
    it("컨트랙의 소유자 초기화 테스팅", function() {
        return RealEstate.deployed().then(function(instance) {
            //만약 배포가 되었다면 콜백함수로 인스턴스를 받고,
            // 전역변수에 저장, owner를 불러와서 반환
            realEstateInstance = instance;
            return realEstateInstance.owner.call();
        }).then(function(owner) {	
            //then 을 통해 owner를 받고 assert 를 이용해서 비교한다. 다르면 에러 메세지 세  가지가 들어온다. 
            //리턴된 실제값(소문자) . 대문자화
            //가나슈의 첫 번째 계정(배포할때 쓴 계정)(대소문자) . 대문자화
            //Error msg 적어준다.             
            assert.equal(owner.toUpperCase(),accounts[0].toUpperCase()), " owner가 가나슈 계정과 동일하지 않습니다. "
        });
    })
})
```

- 저장 한 다음 powershell을 열자

  ``` truffle test --network ganache```


```

PS C:\Users\YOONHOI> cd blockchain/real-estate
PS C:\Users\YOONHOI\blockchain\real-estate> truffle test --network ganache
Using network 'ganache'.



  Contract: RealEstate
    √ 컨트랙의 소유자 초기화 테스팅 (103ms)


  1 passing (290ms)
```

- ```chcp 949``` 는 현재 페이지를 한글로 보여주는 것, 만약 한글깨진다면 사용

``` js

PS C:\Users\YOONHOI\blockchain\real-estate> chcp 949
활성 코드 페이지: 949
PS C:\Users\YOONHOI\blockchain\real-estate> truffle test --network ganache
Using network 'ganache'.



  Contract: RealEstate
    √ 컨트랙의 소유자 초기화 테스팅 (106ms)


  1 passing (286ms)

```



- 일부러 계정을 틀리게 해서 에러메세지가 잘 뜨는지 확인해보자 

  ``` js
  assert.equal(owner.toUpperCase(),accounts[1].toUpperCase()), " owner가 가나슈 계정과 동일하지 않습니다. ")
  ```


## 5.5 매물구입 함수 

```RealEstate.sol``` 에 들어가서 생성자 아래에 함수를 만들자

```js
pragma solidity ^0.4.23;

contract RealEstate {
    // 매입자의 정보를 struct로 저장하고 나중에 한꺼번에 가져올 것이다. 
    struct Buyer { 
        address buyerAddress;
        bytes32 name;
        uint age;
    }

    mapping(uint => Buyer) public buyerInfo;
    address public owner;
    address[10] public buyers; //상태변수로 고정타입의 배열 선언. 

    constructor() public { 
        owner = msg.sender;
    }

    //매물의 아이디, 메물의 이름, 매입자 나이 세 개를 받는다 
    // 고정사이즈 타입 
    // 가시성, 타입제어자 
    // payable 이더를 받아야 할 때 쓴다. 
    // 매입자가 매입했을 때 메타마스크가 뜨고 매입가를(이더) 이 함수로 보내는 것이다. 
    function buyRealEstate(uint _id,bytes32 _name,uint _age) public payable {
        require(_id >=0 && _id<=9); // id는 0~9 사이인지 체크를 합니다 
        							// require 키워드 사용
        buyers[_id] = msg.sender;	// 매물을 사려고 하는 사람을 입력한다. msg.sender는 함수를 사용하고 있는 계정,
        							// 매개변수로 받은 아이디를 이용해서 저장을 할 것이다. 
        buyerInfo[_id] = Buyer(msg.sender, _name, _age);
        
        owner.transfer(msg.value); //이더를 계정에서 계정으로 이동할 때 transfer를 사용한다. msg.value는 wei 만 허용한다. 
        
    }
}

```



- truffle console에서 함수를 실행해보자 . 

- 재컴파일해야하므로 다음 명령어로 migrate 함

  ``` truffle migrate --compile-all --reset --network ganache```

- 트러플 콘솔을 열고

  ``` truffle console --network ganache```

-  전역변수에 저장하자!

  ``` RealEstate.deployed().then(function(instance) { app = instance;})```

- app 의 buyRealEstate()함수를 실행해보자

  ``` app.buyRealEstate(0, "yn" , 24, {from: web3.eth.accounts[1],value : web3.toWei(1.50,"ether")})```

- **문제는 오류가 난다.**  오타 였음 ㅠㅠ 

  ```
  TypeError: unit.toLowerCase is not a function
      at Web3.toWei (C:\Users\YOONHOI\AppData\Roaming\npm\node_modules\truffle\build\webpack:\~\web3\lib\utils\utils.js:357:1)
      at getValueOfUnit (C:\Users\YOONHOI\AppData\Roaming\npm\node_modules\truffle\build\webpack:\~\web3\lib\utils\utils.js:300:1)
  ```

  ==> toWei 함수의 인자가 (1.50 , "ether")였는데 내가 1,50 , "ether" 로 했음 ㅠㅠㅠ


## 5.6 이벤트(Event) 

```js
pragma solidity ^0.4.23;

contract RealEstate {
    struct Buyer { 
        address buyerAddress;
        bytes32 name;
        uint age;
    }

    mapping(uint => Buyer) public buyerInfo;
    address public owner;
    address[10] public buyers;

    //event
    event LogBuyRealEstate(
        address _buyer,
        uint _id
    );
    // 이벤트의 이름을 명시합니다. 
    // 어떤 이벤트가 생성되었을때 이벤트의 내용도 블록에 저장이 된다. 
    // frontend에서 어떤 계정에서 몇번 매물을 샀다고 알려줄 것이다. 계정 주소와 아이디가 필요하다. 

    // 소유자 설정
    constructor() public { 
        owner = msg.sender;
    }


    //매물구입함수
    function buyRealEstate(uint _id,bytes32 _name,uint _age) public payable {
        require(_id>=0 && _id<=9);
        buyers[_id] = msg.sender;
        buyerInfo[_id] = Buyer(msg.sender, _name, _age);

        owner.transfer(msg.value);
        emit LogBuyRealEstate(msg.sender, _id);
    }
}


```

- 매물이 팔리면 이벤트를 이용해서 알려주는 역할

- 완료 메세지를 보내주는건 오너에게 매입가를 송금한 후에 

  ```        emit LogBuyRealEstate(msg.sender, _id);```

  이벤트에 매입자와 매물의 id를 넘겨서 이벤트를 발생시킨다. 

- 콘솔에서 봅시당

- (가나슈에 연결되 상태에서) 재배포 해줍시당, 전역변수도 만들어 준다. 

- 이벤트를 초기화 하고 이벤트 발생시 감지할 수 있는 코드

  ```app.LogBuyRealEstate({},{fromBlock : 0, toBlock : 'latest'}).watch(function(error, event){consle.log(event);})```



- 매물을 하나 매입한 뒤 이벤트가 잘 작동하는지 확인하자. 

  ```app.buyRealEstate(0,"yoonhoi",13,{from : web3.eth.accounts[1], value : web3.toWei(1.50,"ether")})```

  실행하면 ```트랜잭션 영수증```과 ```logs``` 부분에 예전에는 없던 필드들이 등장(console.log(event))에서 나오는 것. 

- 나중에 로그에 args 부분을 통해 프론트에서 완료 메세지를 보여줄 수 있다. 


## 5.7 읽기 전용 함수들 : VIEW

컨트랙의 마지막 함수들을 만들어 보자.

1. 매입자의 정보를 가져오는 함수

    mapping 으로 만든 buyerInfo 사용

   ```js
   // 그대로 추가해준다. 
   
   
       function getBuyerInfo(uint _id) public view returns (address, bytes32, uint) {
           //리턴타입 명시는 buyerInfo와 맞춰준 것
           //매개변수로 받은 매물의 Id를 사용해서 buyerInfo의 키값으로쓰고 해당 값을 가져오자. 
           
           Buyer memory buyer =  buyerInfo[_Id];
           //매개변수 _id를 넘겨서 키값으로 쓰고 해당 buyer를 가져와서 변수에 저장 
           //memory 함수가 끝나면 휘발한다. 
           
           //변수 안에 있는 필드들을 리턴하면 된다. 
           return (buyer.buyerAddress, buyer.name, buyer.age);
       }
   
       // 매입자들의 계정 주소를 저장하는 buyers 배열을 리턴하는 함수
       function getAllBuyers() public view returns(address[10]) {
           return buyers;
       }
   ```

- 콘솔에서 재컴파일 재배포 후 확인해보자

  ```truffle(ganache)> migrate --compile-all --reset```

  ``` RealEstate.deployed().then(function(instance) {app = instance;})```


- 매물을 하나 매입하자

  ```app.buyRealEstate(0,"se",15, {from:web3.eth.accounts[1],value : web3.toWei(1.50, "ether")})```

- 매입자의 정보를 불러오자

  ```
  > app.getBuyerInfo(0);
  [ '0x0028c84514496cccfb56271923fdd608ce226c92',
    '0x7365000000000000000000000000000000000000000000000000000000000000',
    BigNumber { s: 1, e: 1, c: [ 15 ] } ]
  ```

- 이름을 bytes32 타입으로 저장했기 때문에 헥스값으로 나온다

- 나중에 프론트엔드에서 값 변환 해줘야 함



- 매입자들의 계정을 저장하는 buyers 를 불러오는 함수 

  ```
  truffle(ganache)> app.getBuyerInfo(0);
  [ '0x0028c84514496cccfb56271923fdd608ce226c92',
    '0x7365000000000000000000000000000000000000000000000000000000000000',
    BigNumber { s: 1, e: 1, c: [ 15 ] } ]
  truffle(ganache)> ap.getAllBuyers();
  ReferenceError: ap is not defined
  truffle(ganache)> app.getAllBuyers()
  [ '0x0028c84514496cccfb56271923fdd608ce226c92',
    '0x0000000000000000000000000000000000000000',
    '0x0000000000000000000000000000000000000000',
    '0x0000000000000000000000000000000000000000',
    '0x0000000000000000000000000000000000000000',
    '0x0000000000000000000000000000000000000000',
    '0x0000000000000000000000000000000000000000',
    '0x0000000000000000000000000000000000000000',
    '0x0000000000000000000000000000000000000000',
    '0x0000000000000000000000000000000000000000' ]
  ```

- **VIEW는 가스 비용이 들지 않는다는 점** 



## 5.8 마무리 테스팅

```js
var RealEstate = artifacts.require("./RealEstate.sol");

contract('RealEstate',function(accounts) {
    var realEstateInstance;

    it("컨트랙의 소유자 초기화 테스팅", function() {
        return RealEstate.deployed().then(function(instance) {
            realEstateInstance = instance;
            return realEstateInstance.owner.call();
        }).then(function(owner) {
            assert.equal(owner.toUpperCase(), accounts[1].toUpperCase()), " owner가 가나슈 계정과 동일하지 않습니다. "
        });
    });

    it("가나슈 두 번째 계정으로 매물 아이디 0번 매입 후,이벤트 생성 및 매입자 정보와 buyers 배열 테스팅", function() {
        return RealEstate.deployed().then(function(instance) {
            realEstateInstance = instance;
            return realEstateInstance.buyRealEstate(0, "yoonhoi",24,{from : accounts[1],value : web3.toWei(1.50, "ether")});
            //어느 계정으로 함수를 불러들이는지, 1.5를 오너에게~ 보냅시당
        }).then(function(receipt) {// 매입성공시 then을 통해 영수증을 받는다. 
            //콜백으로 받은 receipt를 통해 
            //1. 이벤트가 생성되었는지 확인합니다. 로그의 길이, 예상값1, 생성 되었다는 뜻,ERRORMSG
            assert.equal(receipt.logs.length, 1, "이벤트 하나가 생성되지 않았습니다. ");
            //event, 예상값은 Log Buy Real Estate 
            assert.equal(receipt.logs[0].event, "LogBuyRealEstate", "이벤트가 LogBuyRealEstate가 아닙니다. ");
            //매입자 계정이 1번인지 확인합니댱가나슈 두 번째 계정 
            assert.equal(receipt.logs[0].args._buyer, accounts[1], "매입자가 가나슈 두 번째 계정이 아닙니다. ");
            
            assert.equal(receipt.logs[0].args._id, 0, "매물 아이디가 0 이 아닙니다. ");
            //이벤트 관련 테스트 여기까지, 읽기전용 함수를 합니당
            return realEstateInstance.getBuyerInfo(0);
        }).then(function(buyerInfo) {
            //getBuyerInfo는 세개의 필드를 리턴하는데 각각을 잘 리턴하는지 확인할 것이다. 
            assert.equal(buyerInfo[0].toUpperCase(), accounts[1].toUpperCase(), "매입자의 계정이 가나슈 두번째 계정과 일치하지 않습니다. ")
            //실제값이 헥스로 리턴된다 bytes32타입 ==> 아스키 코드로 변환
            //뒤에 00000 빈공간을 '' 바꿔줘야 한다. 
            assert.equal(web3.toAscii(buyerInfo[1]).replace(/\0/g,''), "yoonhoi","매입자의 이름이 yoonhoi이 아닙니다. ");
            return realEstateInstance.getAllBuyers()
        }).then(function(buyers) {
            assert.equal(buyers[0].toUpperCase(), accounts[1].toUpperCase(), "Buyers 배열 첫 번째 인덱스의 계정이 가나슈 두 번째 계정과 일치하지 않습니다. ");
        });
    })
})
```

사실 테스트 케이스를 각각 나누는게 바람직하지만

한 테스트 안에 여러 개를 넣을 수도 있음













