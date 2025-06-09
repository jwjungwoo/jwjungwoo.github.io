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
CAN 메시지는 데이터 패킷이고, 그 안에 들어 있는 Signal은 실제 의미 있는 값(예: 속도, RPM 등)이다.

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

## send

✅ lamp1 초마다 깜빡이게하기   
<img src="https://github.com/user-attachments/assets/e788faf1-6293-4dc6-a888-b77ec02cdf77" width="800" height="400">   
   
<img src="https://github.com/user-attachments/assets/6ec60147-67e1-4274-9a73-fad13cba0254" width="800" height="400">   


## visual sequence

<img src="https://github.com/user-attachments/assets/358c8503-4ddb-4aba-b624-a517c6bcdeb1" width="800" height="400">   

## signal count 계속 증가??

첨엔 아무것도 안 했는데 signal count 가 계속 증가해서 뭐지 싶었다. 근데 알고보니 CANister 코딩자체가 계속 보내고 있는거였다. 실제 장치도 그렇다고한다.   
<img src="https://github.com/user-attachments/assets/5c67605b-6b7b-49ce-8ef7-accc8f3fd372" width="800" height="440">   

# Mypanel 만들기 실습

## Mypanel 추가 p.245

✅ p.245 처럼 하고 메인프로젝트에서 추가하기   
<img src="https://github.com/user-attachments/assets/424cae1b-dc47-4c9f-861b-9f228ba532a8" width="800" height="440">   
   
✅ 끄기   
<img src="https://github.com/user-attachments/assets/dd0ad093-1079-46ba-9d55-24e48a501a25" width="800" height="440">   
   
✅ 켜기   
<img src="https://github.com/user-attachments/assets/c4b8dfb2-7ed7-41b1-83e4-767e39878a31" width="800" height="440">   
   
✅ 시스템변수 만들기   
<img src="https://github.com/user-attachments/assets/501f7345-8a2b-4ed0-9d6e-6ceda7f7f7f5" width="800" height="440">   
   
✅ sw2도 만듦   
<img src="https://github.com/user-attachments/assets/e4d94320-ff78-4c30-a088-a54549164d1a" width="800" height="440">   
   
✅ visual sequence   
<img src="https://github.com/user-attachments/assets/a738bd99-9333-413f-9d14-da8b1e178935" width="800" height="440">   

## 새로운거

✅ 노드 만들기(insert network node)   
   
✅ configuration 설정해서 CCU 라고 설정   
   
✅ iLconfiguration 설정. db 설정은 최대한 지양한다. 따라서 CANoe 에서 iLconfiguration 으로 설정한다.   
<img src="https://github.com/user-attachments/assets/b5e53aca-fbcc-44d5-9628-2c6abb815ffa" width="800" height="440">   
   
✅ GWC 를 설정해서 iLconfiguration 에서 뭐 하나 클릭하고 확인하면 trace 에서 msg 를 보내는 것을 확인할 수 있다.   
<img src="https://github.com/user-attachments/assets/97f7b60d-978c-4e2d-bc75-515ee76f663a" width="800" height="440">   
   
## IL

✅ signalgeneration   
<img src="https://github.com/user-attachments/assets/4a151b6f-ad73-4436-b71c-5c35d281bd69" width="800" height="440">   

✅ LED1 1,0 반복   
<img src="https://github.com/user-attachments/assets/96ae089b-4683-4864-84ce-2a747102289b" width="800" height="440">   

## CAPL

✅ p.276   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  int gCount = 0; // 전역변수는 한번이니까 이렇게 초기화해도됨.
}

on key 'h'
{
  int a;
  a = 11; // 지역변수는 이렇게 초기화해야됨.
  write("hello world %d", a);
  
  gCount = gCount + 1;
  if(gCount == 5)
  {
    stop();
  }
}

on stopMeasurement
{
  write("Good-bye!");
}
```   
<img src="https://github.com/user-attachments/assets/ec065967-0a0b-4d61-8439-2292ec3b7c11" width="800" height="460">   
   
```c
preStart -> Start ................. preStop -> StopMeasurement
              |...............................|
```

✅ preStop 써보기   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  int gCount = 0; // 전역변수는 한번이니까 이렇게 초기화해도됨.
}

on key 'h'
{
  int a;
  a = 11; // 지역변수는 이렇게 초기화해야됨.
  write("hello world %d", a);
  
  gCount = gCount + 1;
  if(gCount == 5)
  {
    stop();
  }
}

on preStop
{
  write("goodbye1");
  deferStop(1); // 1ms 뒤에 stopMeasurement 실행. 종료를 미룰 때 쓴다.
}

on stopMeasurement
{
  write("Good-bye2");
}
```   
   
✅ p.285   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  
}

//sigKey2 는 sigKey3 와 달리 두 개가 있어서 namespace 까지 적어줘야한다.
on signal frmStatus::sigKey2
{
  if($frmStatus::sigKey2 == 1)
  {
    $sigLED2 = 1;
  }
  else
  {
    $sigLED2 = 0;
  }
}

on signal sigKey3 // 변경됐을때
{ // 0 -> 1, 1 -> 0
  if(getSignal(sigKey3) == 1) // $sigKey3 == 1 만 써도 됨. RxSig 값을 갖고옴
  {
    $sigLED3 = 1; //setSignal(sigLED3, 1); TxSig -> IL Msg. TxSig 값을 바꿈
  }
  else
  {
    $sigLED3 = 0;
  }
}
```   
<img src="https://github.com/user-attachments/assets/9f748a7b-fee8-40fc-83a5-2cfbdcf0bd2b" width="800" height="470">   
   
✅ T3 눌렀을 때 LED3 계속 켜짐   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  int cnt2 = 0;
}

//sigKey2 는 sigKey3 와 달리 두 개가 있어서 namespace 까지 적어줘야한다.
on signal frmStatus::sigKey2
{
  if($frmStatus::sigKey2 == 1)
  {
    $sigLED2 = 1;
  }
  else
  {
    $sigLED2 = 0;
  }
}

on signal sigKey3 // 변경됐을때
{ // 0 -> 1, 1 -> 0
  if(getSignal(sigKey3) == 1) // $sigKey3 == 1 만 써도 됨. RxSig 값을 갖고옴
  {
    $sigLED3 = !$sigLED3;
  }
}
```
   
✅ Key4 누르면 LED4,5,6 켜지기   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  int cnt2 = 0;
}

//sigKey2 는 sigKey3 와 달리 두 개가 있어서 namespace 까지 적어줘야한다.
on signal frmStatus::sigKey2
{
  if($frmStatus::sigKey2 == 1)
  {
    $sigLED2 = 1;
  }
  else
  {
    $sigLED2 = 0;
  }
}

on signal sigKey3 // 변경됐을때
{ // 0 -> 1, 1 -> 0
  if(getSignal(sigKey3) == 1) // $sigKey3 == 1 만 써도 됨. RxSig 값을 갖고옴
  {
    $sigLED3 = !$sigLED3;
  }
}

on signal sigKey4
{
  if($sigKey4 == 1)
  {
    Toggle(sigLED4);
    Toggle(sigLED5);
    Toggle(sigLED6);
  }
}

void Toggle(signal * symbol)
{
  if($symbol == 1)
  {
    $symbol = 0;
  }
  else
  {
    $symbol = 1;
  }
}
```
   
✅ clock 사용   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  msTimer tmrCycle;
  int gCounter;
}

on key 'a'
{
  setTimerCyclic(tmrCycle, 1000);
}

on timer tmrCycle
{
  gCounter++;
  write("%d. execution", gCounter);
}
```   
   
✅ p.292 첫번째 문제   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  msTimer loop;
}

on start
{
  setTimerCyclic(loop,200);
}

on timer loop
{
  $sigSensorValue = (($sigSensorA1 /10 + $sigSensorA2 /10 + $sigSensorB1 /10 + $sigSensorB2 /10) / 4);
}
```   
![sig1234평균](https://github.com/user-attachments/assets/2d641a49-e019-470e-b082-8d650b8f0a18)   
   
위에꺼에서 IL 을 설정하여 메세지 주기를 낮춰서 좀 더 완만한 그래프를 그린다.   
![100ms](https://github.com/user-attachments/assets/fb05aa0d-c5b2-4dcf-8455-82688fa77d03)   
   
✅ p.300 문제   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  
}

on message frmStatus
{
  if(this.sigKey8.phys == 1)
  {
    write("both key pressed");
    stop();
  }
  output(this); // 이거 안 쓰면 Trace 에 아무것도 안 들어감.
}
```   
![300](https://github.com/user-attachments/assets/c9699b6f-b42c-450f-bb4e-d8b99565f635)   
   
근데 현재는 filter 로 인해 하나만 들어온다. 10개 모두 들어오게 하고싶으면   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  
}

on message frmStatus
{
  if(this.sigKey8.phys == 1)
  {
    write("both key pressed");
    stop();
  }
  output(this);
}

on message * {
  output(this);
}
```   
   
✅ button + counter 로 sigCounter 늘리기   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  msTimer loop;
  int gCount = 0;
}

on start
{
  setTimerCyclic(loop, 200);
}

on timer loop
{
  sendMsg();
}

void sendMsg ()
{
  message frmStatus msg;
  msg.sigCounter.phys = gCount;
  gCount++;
  if(gCount > 255)
  {
    gCount = 0;
  }
  output(msg);
}

on sysvar sysvar::My::btn
{
  if(@sysvar::My::btn == 1)
  {
    sendMsg();
//    message 0x1A1 msg;
//    msg.dlc = 4;
//    msg.byte(0) = 0x13;
//    msg.can = 1; // channel은 1
//    output(msg);
  }
}
```   
![button+clock 으로 sigCount 늘리기](https://github.com/user-attachments/assets/a73a37c6-ca05-413c-b9ec-5aee71f2c7fd)   
   
✅ can1 에서 받은걸 can2 에서도 출력하기   
```c
/*@!Encoding:65001*/
includes
{
  
}

variables
{
  msTimer loop;
}

on start
{
  setTimerCyclic(loop,200);
}

on timer loop
{
  $sigSensorValue = (($sigSensorA1 /10 + $sigSensorA2 /10 + $sigSensorB1 /10 + $sigSensorB2 /10) / 4);
}

on message can1.* //can 1(recv) -> can 2(recv) .. can1의 모든 수신 메세지
{
  message * gw;
  gw = this; // 현재 여기선 gw.can = 1 이다 (channel 1)
  gw.can = 2;
  output(gw);
}
```
can1   
![can1](https://github.com/user-attachments/assets/1fbd9f39-bfca-4706-8fa1-c8d2e8a08346)   
can2   
![can2](https://github.com/user-attachments/assets/157c490d-6fca-4a64-9454-83794b48f2ad)   
