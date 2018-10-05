---
layout: post
title:  "4. Solidity smart contract - OpCodes"
subtitle:   "솔리디티 스마트 계약 이론"
categories: study
tags: blockchain
comments: true
---

## 4.3 OpCodes

- 연산에 소모되는 비용  = operation code
  - 산술연산
  - 로직연산
  - memeory or storage 연산
  - 등



- 컴파일 탭 > Detail 누르면 확인 가능
- 컴파일 되면서 먼저 바이트 코드로 변환이 되고, 
- 다음에 옵코드로 분해되어 
- 이더리움 가상 머신에 의해 실행 된다. 



​	[옵코드 툴](https://etherscan.io/opcode-tool)

- 디테일에서 복사한 byte code 를 여기서 decode 한다. 
- 디테일에서 본 옵코드와 내용이 같음을 알 수 있음 



​	[옵코드 내용](https://ethereum.stackexchange.com/questions/119/what-opcodes-are-available-for-the-ethereum-evm)

- 옵코드 해석 및 역할



- 리믹스에서 Deploy 한 후에 Debugger 탭에 가면 옵코드로 변환된 명령어를 스택 쌓고 , 나중에 실행하면 하나씩 읽으면서 처리하는 것을 알 수 있다. 