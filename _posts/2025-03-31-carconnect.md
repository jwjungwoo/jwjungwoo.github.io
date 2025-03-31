---
layout: single
title:  "차량용 통신 시스템"
categories: autoever
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 느낀점

✅ 제어의 효율을 위해 통신이 필요해졌다: 강사님의 말씀이 인상깊었다.   


# 개요

## Point to Point

✅ ECU(electronic): 연산장치와 통신장치로 이루어졌다.   
```c
연산-통신----통신-연산
-: 회로(data가 오감), ----: 물리매체(신호가 오감)
```   
통신장치는 연산장치로부터 받을 수 있는 데이터의 크기가 정해져있다. (CAN 통신은 8byte. 끌어올려서 CAN FD는 64byte. 주소와 CRC 는 천천히 주고받고, 가운데 있는 data 는 엄청 빠르게)   
   
✅ Point to Point: 통신이 필요한 두 개체를 직접 연결하는 것   
   
✅ Old Paradigm: ECU 들이 통신할 때 wire 로 point to point 연결했는데.. 그럼 진단, 확장 어떻게 할거야?   
✅ Bus Networking: 공용의 통신망을 사용하는 물리적 구조. 결국 old paradigm 말고 ECU 들이 버스를 통해 통신을 하게됐다.   
장점: 에러 진단이 가능하다. 확장이 쉬워졌다. 하네스가 가벼워졌다. 싸다. 안정적이다.   
```c
point to point: 전용
bus networking: 공용
```   
   
✅ 쌍선 통신: 노이즈에 강성이 생긴다. 두 개의 다른 신호를 보냄으로써 상대측은 이 두 개의 전압차를 확인한다. 단선 통신에선 3V보다 크냐 작냐인데, 노이즈가 낄 수도 있다. 
쌍선 통신은 3V, 1V 를 보내고 만약 노이즈가 껴도 둘 다 5, 3V 가 되어 노이즈에 강성이 생긴다. 대부분의 통신은 다 이거쓴다.   
<img src="https://github.com/user-attachments/assets/17f2eaa5-b91c-48dd-bae3-796bc184bcbb" width="800" height="640">   
```c
node(단말에 있는것): ECU, 네트워크 장치
end-node(데이터를 생산하고 소비하는 장치): ECU, (네트워크장치는 end-node 아님)
```   
✅ Node Addressing: 송수신자를 구분하는 주소체계   
✅ Broadcast addressing (라디오): 송수신자를 구분하지 않는 주소체계   

## CAN,LIN,FlexRay,OSEK NM

✅   
<img src="https://github.com/user-attachments/assets/2caea696-3a26-4d60-aaaa-e307d0c03d10" width="800" height="640">   
event-driven: 아무때나 메시지 보낼 수 있음(비동기 직렬 통신)   
time-synchronous: 시간 동기화   
master-slave: master 가 물어보면 대답함   
token passing: 토큰을 받아야 메시지 보낼 수 있음. 노드끼리 서로 token 을 주고 받을수있다. 수건돌리기 게임처럼.   
CAN 통신은 라디오처럼 송수신자에 대한 정보가 없다. sender 에게 동기화 된다는 것은 master-slave, token passing 처럼 마스터에 맞추고, token 에 맞추는 것 처럼 sender에 맞춘다는 의미이다.   
   
✅   
<img src="https://github.com/user-attachments/assets/657ed93d-a98b-471a-be95-3e671e72595e" width="800" height="640">   
master-slave: 맨앞에 통신을 위한 필요없는 데이터를 보낸다.   

✅   
<img src="https://github.com/user-attachments/assets/f3923130-9f16-49ab-bba6-49bfdf2556bf" width="800" height="640">   
   
✅   
<img src="https://github.com/user-attachments/assets/07be33dd-fae0-4b6d-bcf6-4a63a4851a96" width="800" height="640">   
CAN FD 는 데이터 전송 속도를 설정할 수 있음. 15Mbit/s 도 가능하다하셨음   

✅ Gateway   
<img src="https://github.com/user-attachments/assets/83cdc36c-d8eb-4cbd-96a1-90267f3631fb" width="800" height="640">   

# CAN

## controller, transceiver

controller: Data Link Layer   
transceiver: Physical Layer( (controller 사이에서) bit 변환, (Bus에서의) 신호 송수신만 신경쓰면 된다)   

## Tx, Rx

CAN 은 기본적으로 2.5V 의 전압이 유지된다.   
   
✅ Translation   
<img src="https://github.com/user-attachments/assets/254e6689-2dc0-4834-bcdb-8ddd486779d3" width="800" height="600">   
Tx 는 현재 전압(노이즈 크게 안 끼면 2.5V)이 어떻든 전압차만 잘 보내면 된다.   
   
✅ Receive   
차이가 난 볼티지 값만 중요하다.   

