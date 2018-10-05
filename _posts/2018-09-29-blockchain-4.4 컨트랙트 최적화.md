---
layout: post
title:  "4. Solidity smart contract - 컨트랙 최적화"
subtitle:   "솔리디티 스마트 계약 이론"
categories: study
tags: blockchain
comments: true
---

## 4.4 컨트랙 최적화

#### 빅오표기법

#### Gas

옵코드에 따라 가스소모가 좌지우지 된다. 

옵코드 마다 가스 소비량이 다름

알고리즘 자체의 O(n)  도 중요하지만 가스소비를 줄이는 방향의 개발도 중요하다!



어느 상황에 가스 소비를 줄여야 할까?



1. 컨트랙 배포할 때의 비용
2. 컨트랙 내의 함수를 불러올 때의비용



#### 컨트랙 배포시

- 주석
- 변수이름
- 타입이름

들은 가스를 소모하지 않는다. 



- 예시

  ```js
  function useless(uint a) public {
      if(a>10) {
          if(a+a<10){ // 실행 될 수가 없는 부분
              b=20;
          }
      }
  }
  ```



#### 함수를 불러오고 실행시킬 때

- 비싼 연산을 최대한 줄이기 

  ```
  uint total =0;
  function expensive() public{
      for (uint i =0; i <10 ;i ++)
  		total+=2;
  }
  ```

  상태변수의 디폴트 저장위치가 storage

  storage는 비용이 무지 비쌈 ㅠ

  이럴 경우에는 local 변수를 사용하는 것이 좋다. 

  ```js
  uint total =0;
  function expensive() public{
      uint temp = 0;
      for (uint i =0; i <10 ;i ++)
  		temp+=2;
      total +=temp;
  }
  ```

- 반복문과 관련된 패턴

  ```js
  function expensive() public {
      uint a = 0;
      uint b = 0;
      for (uint i=0; i<10; i++)
          a+=2;
      for (uint j=0; j<10; j++)
          b+=4;
  }
  ```

  => 이 경우 for 루프를 한 번으로 줄일 수 있다. 

  ```js
  function expensive() public {
      uint a = 0;
      uint b = 0;
      for (uint i=0; i<10; i++)
          a+=2;
          b+=4;
  }
  ```



- 고정된 크기 bytes 배열 쓰기

  string 은 동적인 크기의 타입, 

  고정된 크기를 사용하는 것이 가스 비용을 아낄 수 있다. 

  이더리움 가상머신은 32바이트 에 최적화 되어있다. 

  ```js
  string stringTest = "hello world";
  bytes32 bytes32Text = "hello world";
  // 이쪽이 더 아낄 수 있당
  function bytes32overstring() public {
      stringText = "my world";
      byte32Text = "my world";
  }
  ```

- 정리 

1. 불필요한 코드 정리
2. 비싼 연산최대한 줄이깅(상태변수)
3. 반복문 패턴 제대로 쓰기
4. string대신 bytes32 쓰기



#### 배열 사용시 주의점

- 무제한 크기의 배열 반복 피해야 함

```js
struct Student { 
	uint studentId;
	string name;
}

Student[] students;
//배열화 시킨 상태변수 studends 가 있다. 

function updateStudentById ( uint _studenntId, string _name) public {
    for (uint i = 0 ;i < students.length; i++){
        if (students[i].studentId ==_studentId) {
            students[i].name = _name;
        }
    }
}

//매개변수를 학생 아이디를 넘기고, 이와 일치하는배열인덱스에 학생 이름을 변경하는 함수 

//문제는 이 studensts 배열에 학생이 엄청 많다면 가스 비용이 한도를 넘어서게 된다. 

//매핑을 사용한다!!
```



반복해야 할 데이터가 많아서 for루프가 계쏙 돈다면 limit에 가까워지고 데이터 처리 중에 다 써버려서 shut down 되어버린다. 

가스 한도 라는 개념 때문에 shutdown문제가 된다!

길이 50 이하일 때 for 루프를 사용한다. 



##### for 루프 대신, 매핑을 사용하자 한 번에 찾을 수 있도록

```js
struct Student { 
	uint studentId;
	string name;
}

mapping(uint => Student) studentInfo;

function updateStudentById (uint _stsudentId, string _name) public { 
	Student storage student = studentInfo[_studentId];
	Student.name = _name;
}
```

모든 케이스를 매핑할 순 없다.

다량의 데이터를 iteration 할떄, 

데이터의 크기가 크기 않을때는  배열~