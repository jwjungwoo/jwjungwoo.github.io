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

## Gateway

CAN 은 송신자가 수신자를 지정하지 않는다. 그래섵` gateway 는 있지만 스위치, 라우터, 허브는 없다. Gateway가 보고 전달해준다. Gateway 는 2개 이상의 port 가 있다. Gateway 를 통해 eth, can, flexlay 등이 연결될 수 
있다.

# CAN FD

## flexible datarate

✅ 가변의 속도   
2개의 속도를 가진다. 하나는 500kbps, 나머지 하나는 가변이다.   
   
✅ 64bytes   
한 메세지를 보낼 때 기존의 8배인 데이터를 보낼 수 있지만 속도는 동일하다.   
```c
CAN    CANFD    ETH
8       64      1500

CAN
108bits/msg(메시지)
64bits = data
약 60%/msg

CANFD
71bytes/msg
64bytes
약 90%/msg  -> 메시지 당 데이터의 효율을 엄청나게 높인 프로토콜이다.

ETH
1570bytes/msg
1500bytes
약 95%/msg
```   
   
✅ 한번에 보내는 이점   
```c
APP 입장에선 8개로 쪼개서 보내는게 절대 좋은게 아닌게
단순히 8배가 아니라 arbitration 에서 메세지 중간에 다른 메시지가 낄 수도있어서
데이터 전송 속도가 8배보다 훨씬 길어질 수 있다.
```   
   
✅ reserve   
reserve 는 동기화를 한번 맞추라고 있는 비트다.   
<img src="https://github.com/user-attachments/assets/0cd08099-8562-405f-b41d-876117851eb4" width="800" height="300">   
앞에서 오류가 생기면 뒤에 오류가 누적돼기 때문에 동기화를 맞추라는 것이다.   
   
✅ 48-> 49   
48바이트를 보내면 48바이트를 보내는데 49바이트부턴 64바이트를 보낸다.   
<img src="https://github.com/user-attachments/assets/60b6e352-527b-4e62-aa62-6c9e6b613fd0" width="800" height="650">   
   
✅ CRC 만 잘 들어   
Data Field 값은 빠르게 듣고, CRC 는 잘 들어야함. CRC17 or CRC21   
   
# TOOLS

## CAN 친구들

```c
CANape: ECU Calibration
CANoe, CANalyzer: 네트워크 툴

케노, 케놀라이저, 카나페

CANalyzer 와 CANoe 는 simulation 과 test 에서 차이가 난다.
CANalyzer 는 V 모델의 오른쪽 가장 끝자락에서만 사용한다. 따라서 simulation 에 제한이 된다.
반면 CANoe 는 V 모델 전반에서 사용가능하다. 또한 CANoe 는 Test 가 가능하다.
```
<img src="https://github.com/user-attachments/assets/7131cc7e-5c91-40d1-82b6-e13098b63d25" width="800" height="600">   

## 단위

메시지 단위: frame   
값 단위: signal   
네트워크 단위: bus   

## CAN network description



# 실습

## 예시

<img src="https://github.com/user-attachments/assets/a2861bb8-3b45-43a1-bd19-816ebbf88abf" width="800" height="500">   

## Analysis Window

이더넷은 A,B 중간에 계측할 때 중간선에 구리선을 연결하면 A 와 B 가 보내는 데이터가 두 개 합쳐진 값이라 계측이 힘들지만, CAN 은 그냥 가져와진다. 이 데이터는 DB 를 이용해서 알아낼 수 있다.   
   
1. Trace Window   
fixed position: ID 맨 마지막을 보여줌(ID 들어온적 있나?를 확인해줌)   
chronological: 분석   
   
✅ Msg 검사   
Analysis -> Trace -> CAN Settings 해서 하나 만들면 메세지 받는거 볼 수 있음   
<img src="https://github.com/user-attachments/assets/c898d1f4-18f4-406a-abfe-e4e6e4647146" width="800" height="500">   
   
✅ filter   
<img src="https://github.com/user-attachments/assets/6ce4d7fa-8e9e-4e2e-b9d3-f80c9013eee4" width="800" height="550">   
   
✅ Graphics window p.180   
stress no, a, b, a+b 에 따라 데이터 전송량이 는 것을 확인할 수 있다.   
<img src="https://github.com/user-attachments/assets/5f1f5645-3bdb-4753-bfaf-30a4e59a5b5a" width="800" height="550">   
