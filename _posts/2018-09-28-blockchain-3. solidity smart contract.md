---
layout: post
title:  "3. 솔리디티 스마트 계약 이론"
subtitle:   " "
categories: study
tags: blockchain
comments: true
---

## 3.1. Contract 의 구조

클래스와 비슷, 자바스킄립트와 비슷

타입을 구분할 수 있고

컨트랙트끼리 상속할 수 있다 객체지향처럼



```js
pragma solidity ^0.4.23;
//solidity 컴파일러 버전

//키워드 이름 
contract MyContract {
    uint count; //상태변수
    //컨ㅌ랙 저장소에 영구히 저장된다. 


    constructor() public {
        //생성자, 키워드 + public
    }

    //구조 자바스크립트 ++ 자바 느낌 
    function numOfStudents(address _teacher) public view returns(uint){
            //함수이름     ( 매개변수       )   함수타입        리턴타입
    }
}
```



## 3.2. 접근제어자

함수와 상태변수에 쓸 수있는 4가지 가시성이 존재한다 



| 가시성(Visibility) | 설명                                 |
| ------------------ | ------------------------------------ |
| external           | 1. 외부 컨트랙만 호출 가능/          |
|                    | 2. 상태변수는 external 사용 불가     |
| internal           | 1. 컨트랙 내부 호출 가능             |
|                    | 2. 상속받은 컨트랙트도 호출 가능     |
|                    | 3. 상태변수는 디폴트로 internal 선언 |
| public             | 1. 컨트랙 내부 호출 가능             |
|                    | 2. 상속받은 컨트랙트도 호출 가능     |
|                    | 3. 외부 컨트랙트도 호출 가능         |
| private            | 1. 컨트랙 내부만 호출 가능           |

#### external

```js
pragma solidity ^0.4.23;

contract MyContract{
    //ERROR
    uint external count;
    //external 붙일 수 없다. 
    constructor() public {

    }

    function numOfStudents(address _teacher) public view returns(uint) {
        //ERROR
        test();
        //external 이 붙은 함수를 같은 컨트랙트에서 부를 수 없음
    }

    function test() external{

    }
}

contract YourContract {
    MyContract myContract;
    //myContranct 를 선언한뒤 

    function callTest() public {
        myContract.test();
        //외부 컨트랙트에서 호출 가능
    }
}
```



#### internal

```js
pragma solidity ^0.4.23;

contract MyContract{
    uint count;
    //default 는 internal

    constructor() public {

    }

    function numOfStudents(address _teacher) public view returns(uint) {
        test();
    }

    function test() external{

    }
}

contract YourContract is MyContract{
    //YourContract 가 MyContract를 상속받음
    //~~. 의 접근제어자 없이 바로 사용가능 
    function callTest() public {
        test();
    }
}
```



#### public

```js
pragma solidity ^0.4.23;

contract MyContract{
    uint public count;
    //PUBLIC은 자동적으로 변수의 getter함수를 만들어준다. 

    constructor() public {

    }

    function numOfStudents(address _teacher) public view returns(uint) {
        test();
    }

    function test() public{

    }
}

contract YourContract is MyContract{
    //YourContract 가 MyContract를 상속받음
    //~~. 의 접근제어자 없이 바로 사용가능 
    function callTest() public {// DEFAULT : PUBLIC
        test();
    }
}

contract HisContract {
    MyContract myContract;

    function callTest() public {
        myContract.test();
    }
}

```



#### private

```  js
pragma solidity ^0.4.23;

contract MyContract{
    uint public count;
    //PUBLIC은 자동적으로 변수의 getter함수를 만들어준다. 

    constructor() public {

    }

    function numOfStudents(address _teacher) public view returns(uint) {
        test();
        //같은 컨트랙트 내에서만 부름 
    }

    function test() private{

    }
}

contract YourContract is MyContract{
    function callTest() public {// DEFAULT : PUBLIC
        //ERROR
        test();
    }
}

contract HisContract {
    MyContract myContract;

    function callTest() public {
        //ERROR
        myContract.test();
    }
}
```



## 3.3. 함수 타입 제어자

| 종류     | 설명                                     |
| -------- | ---------------------------------------- |
| view     | 1. 데이터 read-only                      |
|          | 2. 가스 비용 없음                        |
| pure     | 1. 데이터 읽지 않음                      |
|          | 2. 인자값만 활용해서 반환 값 정함        |
|          | 3. 가스 비용 없음                        |
| constant | 0.4.17 버전 이전에는 view/pure 대신 쓰임 |
| payable  | 1.함수가 eth를 받을 수 있게 함           |
|          | 2. 가스 비용 있음                        |

#### view

``` js
uint nuOfStudents;

function getNumOfStudents() public view returns (uint) {
    return numOfStudents;
    //view 가 붙으면 수정/삭제는 못한다. 
}
```



#### pure

블록체인에 저장된 데이터를 불러오는 것으로 쓸 수 없다. 

즉, 인자값만 활용해서 리턴한다. 

```js
function multiply(uint x, uint y) public pure returns(uint) {
    return x* y;
}
```



#### constant

```js
view 나 pure 가 더 권장됩니다. 
```



#### payable

```js
function buy() public pyable {
    require(1000 = msg.value);//송신자가 보넨 웨이
    transferEther(msg.sender);
}
```



## 3.4. 값 타입

#### Boolean 형

``` bool x = false``` 이렇게 사용



#### 정수형

int == int256

uint==yint256

둘 다 8 ~256bits



```int32 x -27462;```

```uint256 x = 24557867;```

 uint256 쓰면 여기에 담을 수 있는 숫자의 사이즈는 2^256 인것!

(default 256)



#### 주소형

타입 : address

값 : 20byte 값 이더리움 계정 주소

추가 설명 : 두 개의 멤버 소유 balance, transfer



0x제외하면 40개 (이더리움 주소 길이)



Ex : 

```js
address x = 0x123;

function send public {
    if(x.balance <10){
        x.transfer(10);
    }
}
```



#### 고정된 크기의 byte 배열

- 타입 : bytes
- 값 : 1~32 byte (사이즈를 지정해서 사용)
- byte ==bytes1
- EX)
- bytes32 x = "hello world!"; (문자열 저장가능 사실 hex로 젖ㅇ)



#### 동적인 크기의 byte 배열

bytes/string

- 값 : 무한
- 추가설명 : 값타입이 아니다!



#### 열거형

enum

- 값 : [value, value2]
- 추가설명 : 값을 정수형으로 리턴
- ex

``` js
enum Direction{ Right, Left}
Direction direction;

function getDirection() public returns (uint) {
    return uint(direction);
}
function setDirection(uint newDirection) public {
    direction = Direction(newDirection);
}
```



## 3.5. 참조 타입 : 데이터 위치

| 종류    | 설명                                             |
| ------- | ------------------------------------------------ |
| storage | 1. 변수를 블록체인에 영구히 저장(ex: 하드디스크) |
|         | 2. 디폴트로 상태변수는 storage                   |
| memory  | 1. 임시 저장 변수 (ex : RAM)                     |
|         | 2. 디폴트로  매개변수와 리턴 값은 memory         |

```js
contract MyContract {
    uint[] ages;// 컴파일러가 자동으로 storage 라고 인식한다. 상태변수디폴트
	
    //매개변수로 넘겼을때는 저장위치를 memory를 쓴다. 
    //함수가 종료되면 값들이 휘발된다. 
    //매개변수로 받은 newAges 값이 들어간다. 
    function learnDataLocation(uint[] newAges) public returns (uint a) {
        ages = newAges;	// 복사 ages 배열 안에 newAges 로 받은 거랑 
        				//이 경우 memory 에서 storage(블록체인에 저장)
        uint16 myAge = 44;	//uint 나 boolean 같은 경우 저장 위치가 디폴트로 메모리!!
        					//함수가 끝나면 날라갑니다. 
        uint[] studentAges = ages;	//배열이 함수 안에 선언되었을때 디폴트로 storage !!!!!
        //값 타입은 memory,  배열은 로컬변수로 쓰일 때 storage
        //studentAges는 ages를 가리키는 포인터가 된다. 

        studentAges[0] = myAge;	// studentAges 배열의 첫번쨰 인덱스를 44로 바꿈 
        						
        a = studentAges[0];

        return a; //a는 메모리이기 때문에 종료되면서 사라진다. 
    }
}
```



## 3.6. 참조 타입 :  배열

| 종류      | 설명            | ex                        |
| --------- | --------------- | ------------------------- |
| 정적 배열 | 사이즈가 고정   | ```uint[s] fixedArray```  |
| 동적 배열 | 사이즈가 무한대 | ```uint[] dynamicArray``` |

```js
contact MyContract {
    uint[] myArray;
    function learnArrays() public {
        uint256[] memory a = new uint256[](5);
        bytes32[] memory b = new bytes32[](10);
        
        a[0] = 1; //  a 배열 첫 번째 인덱스 숫자 1 입력
        a[1] = 2; //  a 배열 두 번째 인덱스 숫자 2 입력
        
        uint8[3] memory c= [1,2,3];
        // 함수 안에서 리터럴통해 배열 초기화 할 때 저장위치 memory
        
        //ERROR
        uint8[3] d = [1,2,3];
        //리터럴 통해 초기화 하는데 저장위치 memory 안써줘서 에러 
        
        myArray.push(5);
        uint myArrayLength = myArray.length;
    }
}
```



## 3.7. 참조 타입 : 구조체

struct : 필요한 자료형들을 가지고 새롭게 정의하는 사용자 정의 타입

``` js
 contract MyContract {
     struct studentName{
         string studentName;
         string gender;
         uint age;
     }

     Student[] students;

     funciton addStudent(string _ name, string _gender, uint _age) public {
         students.push(Student(_name, _gender, _age));
         // student 상태변수 배열에 새로운 Student 입력   
         
         Student storage updateStudent = students[0];
         // storage 에 저장하는 새로운 Student 선언
         // 상태변수 students 배열의 첫 번째 인덱스 값을 대입
         // "storage" 로 선언했기 때문에 상태변수를 가르키는 포인터 역할

         updateStudent.age = 55;
         //updataStudent age 필드를 55로 변경
         // 결과적으로는 상태변수 students 배열의 첫 번째 이ㅣㄴ덱스의 age 필드를 55 로 변경한다. 

         Student memory updateStudent2 = students[0];
         // memory 에 저장하는 새로운 Student 선언
         // 상태변수 students 배열의 첫 번째 인덱스 값을 대입
         // memory 로 선언됐기 때문에 복사한다. 

         updateStudent2.age = 20;
         //updateStudent2의 age 필드를 20으로 변경
         // 이 경우는 복사된 updateStudent2의 age 의 값이 변경된다. 

         students[0] =updateStudent2;
         //memory 배열으 ㅣ값을 상태변수에 직접적으로 대입해주면 
         // students 값 영구히 변경 

     }
 }
```



## 3.8. 참조 타입 : 매핑

``` mapping(_KeyType => _ValueType)```

- Key & Value 를 쌍으로 저장하는 자바의 Map과 비슷
- KeyType : 동적 배열, 열거체, 구조체, 매핑타입 제외 다른 타입들 다 가능
- ValueType :  매핑 포함 다른 타입 다 가능

```
contract MyContract {
    mapping(address =>uint256) balance;
    //매핑키워드로 주소형을 양수형으로 변수이름은 balance
    //어떤 이더리움 계정의 양수값이 블록체인 안에 존재한다. 
    function learnMapping() public {
        balance[msg.sender] = 100;
        balance[msg.sender] += 100;
        // balance 상태변수
        // msg.sender : 현제 이 함수를 불러오는 계정 주소
        // 
        
        uint256 currentBalance = balance[msg.sender];
    }
}
```



``` js
contract MyContract {
    struct Student {
        string studentName;
        string gender;
        uint age;
    }

    mapping(uinnt256 => Student) studetInfo ;

    function setStuddentInfo(uint _studenntId,string _name, string _gender, uint _age) public {
        Student storage student = studentInfo[_studenId]];
        // 키 값으로 매개변수로 받은 _studentId(예 : 1234) 입력
        // 1234 키 값만의 특정 Student 구조체 정보를 불러온다. 

        student.studentName = _name;
        student.gender = _gender;
        studnet.age = _age;
        // 각각 필드에 매개변수로 받은 자료형을 대입 
    }

    function getStudentInfo(uint256 _studentId) view public returns (string, string, uint) {
        //설명 매개변수로 받은 studnetId 를 키값으로 활용하여 1234에 매핑된 value 값인 Student 를 불러온다. 
        return (studentInfo[_studentId].studentName, studnetInfo[_studentId].gender, studentInfo[_studnetId].age);
    }
}
```

