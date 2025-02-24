---
layout: single
title:  "임베디드 C 프로그래밍 기초"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# 배울점
# 지식
## MCU란?
✅ CPU / GPU /  MPU / MCU   
거시 세계   
CPU: Central Processing Unit   
GPU: Graphic Processing Unit (그래픽을 사용할 때 좌표점들을 색깔별로 다 구분해줌. 행렬 연산으로 +,-을 빠르게 해줌)   
   
미시 세계   
MPU: Micro Processing Unit (CPU랑 하는 일이 똑같다. 연산처리, 중앙처리)   
MCU: Micro Control Unit   

## ST
✅ ST사   
수많은 MCU를 만듦. 우리가 쓸 보드는 NUCLEO 보드다. 임베디드는 현재는 8bit, 32bit MCU 보드가 있다. 64bit 부턴 M이 아니다. 요구사항에 따라 high performance냐 ultra-low power냐 선택해야한다. 성능 좋으면 전력소모가 크다. 각 회사는 자신만의 core를 개발한다.   
   
STM32L0 모델   
<img src="https://github.com/user-attachments/assets/2f1f312c-63a1-4f55-97ad-7d293711ade9" width="600" height="350">   
내가 예를들어 코드 길이가 1M면 ROM이 버티지 못하니 메모리 용량이 1M 보다 높은 모델을 사야한다.   
   
✅ STM32 NUCLEO-L073RZ 보드   
우리가 쓸 보드는 STM32 Nucleo 보드이다. 3종류로 구별되는데 32핀, 64핀, 144핀으로 나뉜다.   
MCU: STM32L073   
STM32 Flash memory size(ROM 사이즈): Z for 192Kbytes. (Z말고도 B for 64bits, C for 256 Kbytes 등이 있다.)   
ST link가 usb volate 등을 알아서 맞춰줌.   
   
✅ 요구사항 명세   
요구사항 명세에 따른 알맞은 제품을 선택해야한다.   
예를들어 우리가 쓸 하드웨어 스펙으론 영하 60도에서 작동하는 시스템은 개발할 수 없다.   
<img src="https://github.com/user-attachments/assets/48222bfe-caf3-4ef6-9566-5978013f433d" width="600" height="550">   
   
✅ L073RZ Bus system Arch   
GPIO ports(A, ..., H)와 Cortex M0+를 연결하는 IOPORT라는 버스가 따로 있다. MIF(Memory Interface), SRAM 등은 버스로 연결된다.   
<img src="https://github.com/user-attachments/assets/bedcaa75-52ee-4d11-9969-dc1900c98e93" width="600" height="450">   
   
✅ ARM Cortex   
ARM 코어텍스는 4가지 시리즈로 나뉜다. A, R, M, X 시리즈이다. A 시리즈는 아이폰에서 쓰는 것이다. 규모가 장난 아닌 것이다. 숫자가 커질수록 기능이 커진다.   
   
✅ Memory Map   
<img src="https://github.com/user-attachments/assets/b4a7a51d-4361-4b96-baa9-d3cfcd7a1124" width="600" height="600">   
(0x0000 0000)에서 많이 코딩할 것이다. 왜냐면 우리가 sd 카드를 꽂을건 아니니까.

# 개발 환경 이해 및 실습
## Compile / Linking 이해
✅ 프로그램이란?   
순서 등을 미리 짜놓은 것   
   
✅ Compile / Linking   
<img src="https://github.com/user-attachments/assets/67730cac-6098-4555-b66d-82f3587de2d7" width="600" height="470">   
Object: CPU가 알아볼 수 있는 기계어   
Linker: 호출한 애를 찾으려고 함   
Compile 에러는 문법 에러고, linking 에러는 참조 에러다.   
   
어셈블리어 (.S), C 소스 (.c), C++ 소스 (.cpp)에 따라 각각 어셈블러, C컴파일러, C++컴파일러가 따로따로 있다.   
(armasm, gas), (armcc, gcc), (armcpp, g++)   
   
✅ Cross Compile이란?   
compile과 linking은 pc에서 하고 그 결과물인 executable을 embedded 시스템에서 동작할 수 있게 해주는 것이 Cross Compile이다.   
   
✅ ARM 부팅 시스템(Cortex-M0 시작 순서)   
reset 버튼(주소를 0번지로 간다.) -> Boot Loader -> Reset handler(하드웨어 초기화 작업) -> C start up code -> Application   
   
reset 버튼을 누르면 맨처음에 Main Stack Point가 들어온다. 그 다음은 Reset vector가 들어간다.   
Reset hanlder: SystemInit을 하고, 끝나면 main을 실행한다.

## 실습
✅ 
hal driver는 지금 안 쓸 것이다. 
startup_stm32l073xx.s, main.c, system_stm32l0xx.c 만 냅두면 됨.
