---
layout: single
title:  "어린이 차 남김 사고 예방 시스템"
categories: autoever
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# DFplayer mini

## 128GB sd카드 포멧

우리는 128GB sd 카드를 제공받아서 DF 가 제공하는 서비스를 이용하기위해 FAT32 로 부팅하기 위해선 partition 을 따로 나눠야했다.   
<img src="https://github.com/user-attachments/assets/dca2bc96-8fb5-4c1b-8ae0-40b7f8cdc6f5" width="600" height="480">   

## 노래 다운

youtube 에서 원하는 노래를 검색하고 youtube.com 같은 주소에 youtubepp.com 이란 주소로 youtube 뒤에 pp 를 붙이면 된다. 

## 프로젝트생성

새로운 프로젝트를 생성해야하기때문에 아래와 같이 세팅해야한다.   
   
✅ uart1번 키기.   
2번은 컴퓨터와 연결하기위한 가상 com 포트라 사용 안 한다. 따라서 1번을 키고 braudrate 를 dfmini player 에 맞게 9600 으로 킨다.   
![uart1](https://github.com/user-attachments/assets/d41f4b5b-d96b-4979-be3c-ab1b080adb36)   
   
✅ UVRC 세팅   
DFplayer mini 가 stm 보드로 노래를 틀라고 인터럽트를 걸 수 있게 uvrc 를 세팅해둔다.   
![uvrc](https://github.com/user-attachments/assets/74a52ff3-923a-4a50-800d-f37c52d3a2ee)   

✅ D2, D8   
보드의 D2, D8을 Tx, Rx 로 연결해야한다고 하셨다.   

## compiler 오류

코드를 얼추 짜고 빌드를 실행했는데 기존 컴파일러를 사용할 수 없다고 떠서 compiler 를 바꿨다. 사진에 있는 요술봉같은걸 누르고 다음과 같이 컴파일러를 바꾼다.   
![compiler](https://github.com/user-attachments/assets/f878193e-da58-4c68-8599-91e30f32fb14)   



