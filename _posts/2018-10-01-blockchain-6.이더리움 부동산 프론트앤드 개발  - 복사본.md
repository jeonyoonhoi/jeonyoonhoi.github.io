---
layout: post
title:  "이더리움 부동산 프론트앤드 개발"
subtitle:   "-"
categories: study
tags: blockchain
comments: true
---

## 6. 이더리움 부동산 프론트앤드 개발

### 6.1 RPC Error 해결법 미리알기

개발해서 만날 수 있는 메타마스크 에러 해결법

#### 메타마스크 RPC Errror 

메타마스크가 private network(ganache 서버)에 접속했을 때

메타마스크를 쓰면서 dapp을 테스트 하다가 만약 밸런스를 다시 채우고 싶어서 리스타트 해서 재 배포를 하는 경우, 다시 dapp을 실행해서 다시 매물을 구입해보고 transaction을 보낸다. 이때 RPC ERROR가 발생한다. 

가나슈를 리스타트 했기 때문에 메타마스크에 남아있는 캐시들이 무용지물이 되어버리면서 메타마스크는 어디에 연결되어있는지 햇갈려한다. 

1. 가나슈의 네트워크 ID를 바꿔주는 방법

   바꾸고, 재배포를 한다.

   맽타 마스크의 네트워크를 다른 네트워크 바꿨다가 다시 롴러로 돌아온다. 

2. 메타마스크에서 셋팅즈>reset accounts

   캐쉬에 쌓인 히스토리를 전부 지우는 것. 

   역대까지의 모든 T를 지우게 된다. 


### 6.2 매물 템플렛 작성 및 렌더링

백앤드라 칭할 수 있는 부동산 contract를 만들었다. 

UI를 담당하는 프론트를 만들어보자 



package.json 안에 있는 라이트 서버를 설치해야 한다. 

1. ```npm install``` 을 해서 노드 모듈스를 받쟈! (작업폴더에서)

우리는 스타터 템플릿을 받았기 때문에 신경써야 할 것은

``` app.js ``` 와 ```index.html``` 이다. 



- index.html 에 33 줄에 아래를 추가해준다. 

```html
<!- 템플 만들고 일단 안보이게 해둔다 -->
    <div id = "template" style = "display: none ;">
      <div class = "col-sm-6 col-md-4 col-lg-3">
        <div class="panel panel-success panel-realEstate">
          <div class = "panel-heading">
            <h3 calss = "panel-title"> 매물 </h3>
          </div>
          <div clas = "panel-body">
            <img style = "width : 100%;" src = "">
            <br/><br/>
            <strong>아이디</strong> :<span class = "id"></span><br/>
            <strong>종류</strong> : <span class = "type"></span><br/>
            <strong>면적(m^2)</strong> : <span class = "area"></span><br/>
            <strong>가격(ETH)</strong> : <span class = "price"></span><br/><br/>
            <button class = "btn btn-info btn-buy"
                    type = "button"
                    data-toggle = "modal"
                    data-target="#buyModal">
                  매입
            </button>
            <button class="btn btn-info btn-buyerInfo"
                    type="button"
                    data-toggle="modal"
                    data-target="#buyInfoModal"
                    style="display:none;">
                    매입자정보
            </button>
          </div>
        </div>
      </div>
    </div>
```



- app.js

  ```real-estate.json```에 있는 정보를 불러 올 것이고

  그 정보들을 ```index.html``` 에 각 항목에 맞춰서 보여줄것이다. 

  매물리스트 div에 우리가 만든 10개의 템플릿을 다 보여줄것임


``` js
wApp = {
  web3Provider: null,
  contracts: {},
	
  init: function() {
   
  },

  initWeb3: function() {
	
  },

  initContract: function() {
		
  },

  buyRealEstate: function() {	

  },

  loadRealEstates: function() {
	
  },
	
  listenToEvents: function() {
	
  }
};

$(function() {
  $(window).load(function() {
    App.init(); //일단 실행되면 App.init()이 실행된다 
  });
});

```



- init : function()

```js

  init: function() {
    $.getJSON('../real-estate.json', function(data) {
        //real-estate.json의 경로를 알려주고 펑션을통해 콜백으로 데이터를 받는다. 
        //list : 아이디가 리스트인 div의 정보를 담는 변수
      var list = $('#list');
        //list : 아이디가 리스트인 div의 정보를 담는 변수
      
      var template = $('template');
        //template의 정보를 담는 정수 

      for (i = 0 ; i <data.length; i++) { 
          //for루프로 데이터 길이만큼 돌리겠다. 
          //json배열을 하나하나 읽어서 template에 담음 picture필드에 있는 값을 찾도록
          //제이슨 배열 각인덱스에 있는 src필드에 있는 picture를 갖도록 하는 것
        template.find('img').attr('src', data[i].picture);
        //id필드를 찾고 그 값을 보이게 할고다!
        template.find('.id').text(data[i].id);
        template.find('.type').text(data[i].type);
        template.find('.area').text(data[i].area);
        template.find('.price').text(data[i].price);
        
          //리스트 변수에 완성된 템플릿을 추가합니다. 
          
        list.append(template.html());
          //리스트 변수에 완성된 템플릿을 추가합니다. 
          // 아이디가 list인 div에 열개인 템플릿을 받는다. 
      }
    })
   
  }
```



- ```npm run div``` 파워셸 명령어 입력해서 라이트 서버로 실행시킨다!

페이지가 뜨면서 열개의 매물이 뜨는 것을 볼 수있다. 



### 6.3 Web3 & 컨트랙 인스턴스화

web3와 컨트랙의 인스터스를 생성하는 함수를 만듭니다. 

- app3.js는 이더리움과 소통할 수 있도록 해주는 라이브러리이다. 

- 트러플 콘솔에서 web3 api 쓴것처럼 여기서도 쓸것이당

- web3를 댑에서 쓸 수 있도록 인스턴스화 하는 작업이 필요하다. 



1. 일단 template 함수 작업을 마치면 App.initWeb()

```js
 // function init() 마지막에
 
 return App.initWeb3();
```

2. initWeb3

```js
  initWeb3: function() {
    if (typeof web3 != 'undefined') { 
      App.web3.Provider = web3.currentProvider;
      web3 = new Web3(web3.currentProvider);
    } else {
      App.web3Provider = new web3.providers.HttpProvider('http://localhost : 8545');
      web3 = new Web3(App.web3Provider);
    }
    return App.initContract();	
  },

```

> - if
>
> 만약 web3가 undifined가 아니라면  = 주입된 web3 인스턴스가 존재하면!
>
> ((undifined)= 브라우저에 메타마스크 설치가 안되어있음 )
>
> 깔려있다면 web3를 미리 주입시켜둔다. 
>
> - else
>
> 만약 존재하지 않다면 우리 댑에서 쓸수있는 web3를 만듭니다. 
>
> 공급자의 정보를 가져와서 web3를 가져오장
>
> 가냐슈가 켜져있다면 가나슈가 프로바이더가 되는겁니당



3. initWeb3

   ```js
     initContract: function() {
   		$.getJSON('RealEstate.json',function(data) {
         App.contracts.RealEstate = TruffleContract(data);
         App.contracts.RealEstate.setProvider(App.web3Provider);
       })
     }
   
   ```

   > JSON 파일을 가져오고
   >
   > 아티팩 파일에 있는 데이터를 Truffle Contract 라이브러리에서 제공하는 TruffleContract()를 사용
   >
   > web3공급자의 정보를 갖고 있는 전역변수 공급자를 설정한다. 

4. 우리가 만든 스마트 컨트랙트를 인스턴스화



### 6.4 매입자 정보 모달 및 데이터 전달

* 프론트엔드에서 매물구입함수에 어떻게 데이터를 보낼 수 있는지?
* 블록체인들에 정보들을 저장시키게 하는 아주 중요한 함수입니다. 
* buyRealEstate() 함수에 데이터를 3가지를 받음 --> 어떻게 블록체인에?
* payable --> 매입버튼을 클릭했을 때 이름과 나이 제출하면 필요한 데이터를 넘긴다. 



- [부트스트랩](http://bootstrapdocs.com/v3.3.6/docs/javascript/#modals)
  - bootstrap 3 모달 예제를 가져옵니다. 
- template아래 붙여넣기 
  - ```id = "buyModal"``` 속성 추가 template에 있는  ```data-target = "#buyModal"``` 이랑 매치시켜줘야 한다!!
  - 그래야 매입 버튼을 클릭했을 때 modal이 띄워지게 된다. 
- body 에 input 추가
  - ```<button type="button" class="btn btn-primary" onclick="App.buyRealEstate(); return false;">제출</button>```
  - 제출 버튼 클릭하면 buyRealEstate()함수 실행

```html
    <!--모달-->
    <div class="modal fade" tabindex="-1" role="dialog" id = "buymodal">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
            <h4 class="modal-title">매입자 정보</h4>
          </div>
          <div class="modal-body">
            <input type = "text" class = "form-control" id = "name" placeholder = "이름"/><br/>
            <input type = "number" class = "form-control" id = "age" placeholder = "나이"/>          
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">닫기</button>
            <button type="button" class="btn btn-primary" onclick="App.buyRealEstate(); return false;">제출</button>
          </div>
        </div><!-- /.modal-content -->
      </div><!-- /.modal-dialog -->
    </div><!-- /.modal -->

```

- 이제 파워셀을 열고 잘 되는지 확인해볼까?!
- 폴더로 가서 ```npm run dev``` 



- 제출버튼 클릭시  ```buyRealEstate()``` 실행되는 기능

  제출을 누르면

  -  매물아이디
  - 가격
  - 사는사람 이름
  - 나이를 

  보내주는 함수를 합시당

  - Modal의 hidden매물의 id, price를 받는 안보이는 필드를 만들어주고 저장을 한다. 

    ```html
    <div class="modal-body">
        <input type = "hidden" id = "id">
        <input type = "hidden" id = "price">
        ..
    ```

- app.js 파일 맨 밑에 function()으로 간다

  - 이곳이 html 페이지가 다 로드 되었을 때 어떤 것을 실행하라고 정의할 수 있는 공간 

  매입버튼을 클릭해서 모달이 띄워져 있으면, 

  해당 템플릿의 매물의 id와 price를 가져와서

  hidden으로 숨겨진 필드에 저장시키기

  - 해당 템플렛의 id값을 찾고 그 id 값을 변수id로 전달

  - 이더가 타입이 스트링인데 float값으로 변환하고 웨이로 바꿔 가져옴

  - 두 히든타입 인풋 필드에 방금 변수로 받은 id와 price 값을 저장

    ```js
    $('#buyModal').on('show.bs.modal',function(e){
        var id = $(e.relatedTarget).parent().find('.id').text();
        var price = web3.toWei(parseFloat($(e.relatedTarget).parent().find('.price').text() || 0),"ether");
    
        $(e.currentTarget).find('#id').val(id);
        $(e.currentTarget).find('#price').val(price);
      })
    ```

  - 모달에 있는 id속성이 id인곳과 id속성이 price인 곳을 찾아서 각각의 인풋필드에 매물의 id와 price를 전달하도록 했다. 

- 즉, 제출버튼을 클릭하면 4개의 데이터들이  buy real Estate 함수에서 받을 수 있게 되었다. 

  ```js
  //app.js 파일 
  
    buyRealEstate: function() {	
      var id = $('#id').val();	
      var name = $('#name').val();	
      var price = $('#price').val();	
      var age = $('#age').val();
        
      console.log(id);
      console.log(price);
      console.log(name);
      console.log(age);
        
      //인풋필드 빈공간으로 만든다
      $('#name').val('');
      $('#age').val('');
        
    },
  ```

  - 모달에 있는 4개의 input 태그를 가져오고 변수에 저장시켜서 콘솔에서 확인함
  - 그리고 input field를 빈공간으로 만든다

### 6.5 컨트랙 매물구입함수 연결

- ```buyRealEstate``` 에서 받은 변수를 넘겨보도록 하겠습니다. 

```js
  buyRealEstate: function() {	
    var id = $('#id').val();	
    var name = $('#name').val();	
    var price = $('#price').val();	
    var age = $('#age').val();

    web3.eth.getAccounts(function(error,accounts) {
      if(error) {
        console.log(error);
      }
      //첫번째 계정을 뜻한다. 
      var account = accounts[0];
      App.contracts.RealEstate.deployed().then(function(instance) {
        var nameUtf8Encoded = utf8.encode(name);        
        //한글일 경우를 대비해서 UTF8로 인코딩 시킨다.
        //매물구입함수에 준비된 데이터를 넘기자
        console.log(nameUtf8Encoded);
        return instance.buyRealEstate(id,web3.teHex(nameUtf8Encoded), age, {from : account, value:price});
        //인코딩 된 이륾은 또 hex로 바꿔서 보내줘야 한다. 
        //account 는 어디서 등장? web3를 통해 노드에 연결된 계정들을 불러온다.       
      }).then(function() {
       $('#name').val('');
       $('#age').val('');
       $('#buyModal').modal('hide');
       return App.loadRealEstates();
     }).catch(function(err) {
       console.log(err.message);
     });
    });
  }
```

- 

### 6.6 매입 후 UI 업데이트 (이미지 교체, 버튼 비활성화)

##### 매물이 매입되면 매입버튼 비활성화 시켜서 매입을 못하게 막고 img도 교체하자

- 모달 창 닫는 부분 밑에 

  ```js
         return App.loadRealEstates();
  ```

  ```buyRealEstate()``` 가 끝나면 ```loadRealEstates()``` 를 불러오도록

- loadRealEstate() 함수

  ``` getAllBuyers()``` 배열을 가져올 것이다. 

  리턴된 buyers 를 for 루프로 돌린다. 

  매물을 매입할 때 매물 아이디를 buyers 배열의 인덱스 값으로 쓰고 그 인덱스의 계정 주소를 저장한다. 그 뜻은, 만약 어느 배열 인덱스의계정 주소가 존재한다면 해당 매물은 팔렸다는 뜻이다.-

```js
loadRealEstates: function() {
    //getAllBuyers 함수를 가져와서 for 루프로 돌린다. 
    App.contracts.RealEstate.deployed().then(function(instance) {
      return instance.getAllBuyers.call();
    }).then(function(buyers) {
      for (i =0 ;i<buyers.length;i++) {
        // 만약 해당 인덱스에 주소가 존재한다면
        if (buyers[i] !== '0x0000000000000000000000000000000000000000'){
        //이더리움에서는 null 쓰지 않고 주소값 그대로 넣어줌 
          var imgType = $('.panel-realEstate').eq(i).find('img').attr('src').substr(7);

          switch(imgType) {
            case 'apartment.img' : 
              $('.panel-realEstate').eq(i).find('img').attr('src','images/apartment_sold.jpg');
              break;
            case 'townhouse.img' : 
              $('.panel-realEstate').eq(i).find('img').attr('src','images/townhouse_sold.jpg');
              break;
            case 'house.img' : 
              $('.panel-realEstate').eq(i).find('img').attr('src','images/house_sold.jpg');
              break;
          }
 // 해당 템플릿에서 매입버튼을 매각으로 바꾸고 비활성화
          $('.panel-realEstate').eq(i).find('.btn-buy').text('매각').attr('disabled',true); 
        }
      }
    }).catch(function(err) {
      console.log(err.message);
    })
  }
```



- 하지만 page 새로고침하면 초기화 상태로 돌아온다.
- 처음 dapp이 실행되면 리스트 불러오는 init함수와 web3와 컨트랙트를 인스턴스화 시키고 초기화 해주는 함수가 실행된다. 
- initContract() getJSON마지막 부분에 loadRealEstates(); 



init  함수는 initWeb3 

initWeb3 inintContract()

initContract() 는 App.loadRealEstate(); 를 연결하자



### 6.7 매입 후 UI 업데이트(매입자 정보 버튼)

- 매입자의 정보를 modal에서 display 하자

- ```loadRealEstate```함수에서 매각버튼 바꾸기 아래에 다음 추가

```js
          $('.panel-realEstate').eq(i).find('.btn-buyerInfo').removeAttr('style');
          //해당 탬플렛을 찾고 그 안에 스타일 속성을 없엔다
```



- 매입자 정보 클릭하면 모달을 띄워서 볼 수 있도록

```html

    <!--모달-->
    <div class="modal fade" tabindex="-1" role="dialog" id = "buyerInfoModal">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
              <h4 class="modal-title">매입자 정보</h4>
            </div>
            <div class="modal-body">
                <strong>계정주소</strong> : <span id = "buyerAddress"></span><br/>
                <strong>이름</strong> : <span id = "buyerName"></span><br/>
                <strong>나이</strong> : <span id = "buyerAge"></span><br/>
                 
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-default" data-dismiss="modal">닫기</button>
              <button type="button" class="btn btn-primary" onclick="App.buyRealEstate(); return false;">제출</button>
            </div>
          </div><!-- /.modal-content -->
        </div><!-- /.modal-dialog -->
      </div><!-- /.modal -->
  
```

``` id = "buyerInfoModal"``` 

```<div class = "modal-body">```  부분 수정



- app. js function 마지막에 추가

```js
  //buyerModalInfo 가 열려져 있을 때 매입자 정보를 보여와서 보여주는 코드
  
  $('#buyerInfoModal').on('show.bs.modal',function(e){
    var id = $(e.relatedTarget).parent().find('.id').text();
    
    App.contracts.RealEstate.deployed().then(function(instance) {
      return instance.getBuyerInfo.call(id);
    }).then(function(buyerInfo) {
      $(e.currentTarget).find('#buyerAddress').text(buyerInfo[0]);
      $(e.currentTarget).find('#buyerName').text(web3.toUtf8(buyerInfo[1]));
      $(e.currentTarget).find('#buyerAge').text(buyerInfo[2]);
    }).catch(function(err) {
      console.log(err.message);
    })
  });
```

```getBuyerInfo 키값으로 id ,value로 계정, 이름, 나이 리턴 ```



매입자 정보 버튼활성화

클릭하면 모달이 뜨면서 매물 id를 컨트랙에 getbuyerinfo 함수에 넘기고 

정보를 불러왔습니다. 



### 6.8 이벤트를 통한 알림 메세지 

프론트엔드에서 event LogBuyRealEstate( ) 를 불러오고, 

항상 이벤트를 감지하도록 하면서  다른 사용자가 매입을 하게 되면

내 창에 메세지를 띄우도록 

매입을 하면 어떤 계정서 몇번째 매물을 매입했는지 유저들에게 알려주도록 함













