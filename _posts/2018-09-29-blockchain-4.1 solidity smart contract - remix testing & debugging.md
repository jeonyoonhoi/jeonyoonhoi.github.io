---
layout: post
title:  "4.솔리디티 스마트 계약 실전"
subtitle:   "리믹스 테스트 & 디버깅"
categories: study
tags: blockchain
comments: true
---

## 4.1. Remix Testing&Debugging

#### Remix

솔리디티로 컨트랙를 개발할 때 리믹스라는 아주 좋은 툴이 있다. 

컨트랙을 브라우저에서 바로 작성하고 디버깅하고 컨트랙 배포까지 지원

컨트랙 개발하면서 생각하는 로직과는 다르게 행동할때 디버깅해보며값들을 하나하나 확인할 수있다. 

사이즈가 크지 않은 컨트랙을 테스팅하고 디버깅할 때 쓰기 좋다. 

우리가 배운 예시를 리믹스로 디버깅해보자!



[리믹스](remix.ethereum.org)



- 세팅탭에서 버전 0.4.24( 현재 기준)

- Compile 탭에서 Auto compile 선택해준다 

  스크립트 자동하면 자동으로 컴파일 된다. 

- 배열은 디폴트로 storage로 저장된다. 

- 컴파일러는 명시하라고 워닝 사인을 보낸다. 



<Run 탭>

브라우저에 블록체인 인스턴스를 삽입하기 위해 리믹스에서 세 가지 옵션을 제공한다. 



- Java Script VM : 간단한건 이게 제일 변한

- injected Web3 메타마스크

- 게스나 가나슈의 엔드포인트 주소를 입력해서 사용하는 방식

  가나슈에서 호스트랑 포트 설정한거 기억하는지? 그거 연결하는거!



- JVM 설정하면 계정 자동생성, Gas limit도 맘대로 설정할 수있다.

  가장 빠른속도로 진행 가능 

- Deploy 버튼 클릭!(배포)
- 위에 선택된 계정으로 JVM 환경에 컨트랙을 배포
- 가스비용 줄어든 것 볼 수 있음 = 가스비용도 사용했다.  
- 터미널에 아래 보면 새로운 T 정보가 뜬다. 
-  오른쪽에 패널이 생성되었는데 바로 만든 함수! 
- 매개변수로 양수형 을 받는 learnDataLocation
- 매개변수 값을 입력해 보면? 



```
pragma solidity ^0.4.0;

contract MyCotract{
    uint[] ages;
    
    function learnDataLocation(uint[] newAges) public returns (uint a) {
        ages = newAges;
        uint16 myAge = 44;
        uint[] storage studentAges = ages;
        
        studentAges[0] = myAge;
        
        a = studentAges[0];
        
        return 0;
    }
}
```



```
pragma solidity ^0.4.0;
contract MyContract {
    struct Student {
        string studentName;
        string gender;
        uint age;
    }
    
    mapping(uint256 =>Student) studentInfo;
    
    function setStudentInfo(uint _studentId, string _name, string _gender, uint _age )public {
        Student storage student = studentInfo[_studentId];
        
        student.studentName = _name;
        student.gender = _gender;
        student.age = _age;
    }
    
    function getStudentInfo(uint256 _studentId) public view returns (string, string, uint) {
        return (studentInfo[_studentId].studentName, studentInfo[_studentId].gender, studentInfo[_studentId].age);
        
    }
}
```



- 솔리디티 디버깅 탭 이용하면 무지 편하다>_ㅇ

(실습 강의라 딱히 적을게 없다 ㅇㅅㅇ)











