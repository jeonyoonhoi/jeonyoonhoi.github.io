---
layout: post
title:  "1. 이더리움 DApp - 부동산 스마트 컨트랙트 개발 - intro"
subtitle:   "1. intro"
categories: study
tags: blockchain
comments: true
---

## 1. Intro

[inflearn](https://www.inflearn.com/course/blockchain-%EC%9D%B4%EB%8D%94%EB%A6%AC%EC%9B%80-dapp/)

유료 인터넷 강의를 돈주고 듣는건 고등학교때 이후로 처음이다. 

필요한 부분은 5강 이후라서 고민했다.

하지만 복숩 및 강화할겸 처음부터 차근 차근 보며 정리 해보기로 했다. 

그동안 공부하면서 기록에 너무 소홀했던것 같아 반성하며 이번 인강 듣기 + 자료정리를 추석동안 끝내보려고 한다. 





### 강좌 교육과정

| 1. 첫 발걸음                        |                                                              |          |
| ----------------------------------- | ------------------------------------------------------------ | -------- |
|                                     | [인트로](https://www.inflearn.com/unit/%ec%9d%b8%ed%8a%b8%eb%a1%9c-2/) | 미리보기 |
| 2. 환경설정                         |                                                              |          |
|                                     | [이더리움 DAPP 개발환경 셋업 I (Geth, 가나슈, 노드.js, 트러플)](https://www.inflearn.com/unit/%ec%9d%b4%eb%8d%94%eb%a6%ac%ec%9b%80-dapp-%ea%b0%9c%eb%b0%9c%ed%99%98%ea%b2%bd-%ec%85%8b%ec%97%85-i-geth-%ea%b0%80%eb%82%98%ec%8a%88-%eb%85%b8%eb%93%9c-js-%ed%8a%b8%eb%9f%ac%ed%94%8c/) | 미리보기 |
|                                     | 이더리움 DAPP 개발환경 셋업 II (비쥬얼 스튜디오 코드, 메타마스크) |          |
|                                     | [Geth로 프라이빗 노드 구축 I (제네시스 블록, 계정 생성)](https://www.inflearn.com/unit/geth%eb%a1%9c-%ed%94%84%eb%9d%bc%ec%9d%b4%eb%b9%97-%eb%85%b8%eb%93%9c-%ea%b5%ac%ec%b6%95-i-%ec%a0%9c%eb%84%a4%ec%8b%9c%ec%8a%a4-%eb%b8%94%eb%a1%9d-%ea%b3%84%ec%a0%95-%ec%83%9d%ec%84%b1/) | 미리보기 |
|                                     | Geth로 프라이빗 노드 구축 II (노드 첫 실행, DAG 파일 생성)   |          |
|                                     | Geth로 프라이빗 노드 구축 III (Geth 콘솔)                    |          |
| 3. 솔리디티 스마트 계약 이론        |                                                              |          |
|                                     | [컨트랙의 구조](https://www.inflearn.com/unit/%ec%bb%a8%ed%8a%b8%eb%9e%99%ec%9d%98-%ea%b5%ac%ec%a1%b0/) | 미리보기 |
|                                     | [접근 제어자](https://www.inflearn.com/unit/%ec%a0%91%ea%b7%bc-%ec%a0%9c%ec%96%b4%ec%9e%90/) | 미리보기 |
|                                     | 함수 타입 제어자                                             |          |
|                                     | 값 타입                                                      |          |
|                                     | 참조 타입: 데이터 위치                                       |          |
|                                     | 참조 타입: 배열                                              |          |
|                                     | 참조타입: 구조체                                             |          |
|                                     | 참조타입: 매핑                                               |          |
| 4. 솔리디티 스마트 계약 실전        |                                                              |          |
|                                     | Remix 테스팅 & 디버깅 I                                      |          |
|                                     | Remix 테스팅 & 디버깅 II                                     |          |
|                                     | 가스란?                                                      |          |
|                                     | 옵코드 (OpCodes)                                             |          |
|                                     | 컨트랙 최적화 I                                              |          |
|                                     | 컨트랙 최적화 II                                             |          |
|                                     | [트러플 & 컨트랙 배포 I (구조 설명, 배포)](https://www.inflearn.com/unit/%ed%8a%b8%eb%9f%ac%ed%94%8c-%ec%bb%a8%ed%8a%b8%eb%9e%99-%eb%b0%b0%ed%8f%ac-i-%ea%b5%ac%ec%a1%b0-%ec%84%a4%eb%aa%85-%eb%b0%b0%ed%8f%ac/) | 미리보기 |
|                                     | 트러플 & 컨트랙 배포 II (트러플 콘솔 사용)                   |          |
|                                     | 트러플 & 컨트랙 배포 III (가나슈 사용)                       |          |
| 5. 이더리움 부동산 스마트 계약 개발 |                                                              |          |
|                                     | [부동산 DAPP 미리보기 및 주의점](https://www.inflearn.com/unit/%eb%b6%80%eb%8f%99%ec%82%b0-dapp-%eb%af%b8%eb%a6%ac%eb%b3%b4%ea%b8%b0-%eb%b0%8f-%ec%a3%bc%ec%9d%98%ec%a0%90/) | 미리보기 |
|                                     | 스타터 템플렛 받기                                           |          |
|                                     | 컨트랙 소유자 설정                                           |          |
|                                     | 첫 테스팅                                                    |          |
|                                     | 매물구입 함수                                                |          |
|                                     | 이벤트 (Event)                                               |          |
|                                     | 읽기전용 함수들                                              |          |
|                                     | 마무리 테스팅                                                |          |
| 6. 이더리움 부동산 프론트앤드 개발  |                                                              |          |
|                                     | RPC Error 해결법 미리알기                                    |          |
|                                     | 매물 템플렛 작성 및 렌더링                                   |          |
|                                     | Web3 & 컨트랙 인스턴스화                                     |          |
|                                     | 매입자 정보 모달 및 데이터 전달                              |          |
|                                     | 컨트랙 매물구입함수 연결                                     |          |
|                                     | 매입 후 UI 업데이트 I (이미지 교체, 버튼 비활성화)           |          |
|                                     | 매입 후 UI 업데이트 II (매입자 정보 버튼)                    |          |
|                                     | 이벤트를 통한 알림 메세지                                    |          |
| 7. 마무리                           |                                                              |          |
|                                     | 공개 네트워크 컨트랙 배포 미리알기                           |          |
|                                     | 메타마스크 계정 리셋 및 에더 얻기                            |          |
|                                     | Ropsten 테스트넷 컨트랙 배포 및 테스팅 I (리믹스 사용)       |          |
|                                     | Ropsten 테스트넷 컨트랙 배포 및 테스팅 II (인퓨라 사용)      |          |
|                                     | [요약](https://www.inflearn.com/unit/%ec%9a%94%ec%95%bd/)    |          |

