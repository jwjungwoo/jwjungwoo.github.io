---
layout: single
title:  "임베디드 sw 개발 기초"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# Arduino
## 세팅
<arduino.cc>에서 본인의 환경에 맞게 스케치를 다운해준다. 그 뒤 아래와 같이 세팅해준다.   
<img src="https://github.com/user-attachments/assets/cc718a89-d4dd-465e-a913-ae8d29a13fcc" width="600" height="400">   

## 
# Arduino Practice
## pull down
<img src="https://github.com/user-attachments/assets/60d7c53a-9aab-47cc-aff5-64810ace96e8" width="500" height="400">   
✅ floating   

## RGB LED 점멸하기
<img src="https://github.com/user-attachments/assets/e9806faa-65f5-4da6-bbb8-b34aa80944c9" width="500" height="400">   
다음과 같이 하면, 삼원색과 그 조합의 색들이 순차적으로 빛난다.   
```c
#define R_PIN 9
#define G_PIN 10
#define B_PIN 11


void setup() {
    pinMode(R_PIN, OUTPUT);
    pinMode(G_PIN, OUTPUT);
    pinMode(B_PIN, OUTPUT);

    digitalWrite(R_PIN, LOW);
    digitalWrite(G_PIN, LOW);
    digitalWrite(B_PIN, LOW);
}

void loop() {
    digitalWrite(R_PIN, HIGH); digitalWrite(G_PIN, LOW); digitalWrite(B_PIN, LOW); delay(100);
    digitalWrite(R_PIN, HIGH); digitalWrite(G_PIN, HIGH); digitalWrite(B_PIN, LOW); delay(100);
    digitalWrite(R_PIN, HIGH); digitalWrite(G_PIN, LOW); digitalWrite(B_PIN, HIGH); delay(100);
    digitalWrite(R_PIN, LOW); digitalWrite(G_PIN, HIGH); digitalWrite(B_PIN, LOW); delay(100);
    digitalWrite(R_PIN, LOW); digitalWrite(G_PIN, HIGH); digitalWrite(B_PIN, HIGH); delay(100);
    digitalWrite(R_PIN, LOW); digitalWrite(G_PIN, LOW); digitalWrite(B_PIN, HIGH); delay(100);
    digitalWrite(R_PIN, HIGH); digitalWrite(G_PIN, HIGH); digitalWrite(B_PIN, HIGH); delay(100);
}
```
```

## 아두이노에서 pc로 보내기
<img src="https://github.com/user-attachments/assets/09b683e6-f6bf-446c-9405-fd0027bd1656" width="500" height="400">   
우측 상단에 돋보기 모양의 serial 버튼을 누르고 실행하면 위와 같이 나온다.   
```c
void setup() {
    Serial.begin(9600);
}

void loop() {
    Serial.print("Hello, Arduino\n");
    // Serial.write(97);
    // Serial.write(98);
    Serial.write(0x61); //아스키 코드
    Serial.write(0x62);
    Serial.write(0x63);
    Serial.write(0x64);

    delay(1000);
}
```

## pc에서 아두이노로 보내기

# ETC
## Digital vs Analog
✅ GPIO   
GPIO는 범용 입출력(General Purpose Input/Output)의 약자로, mcu나 싱글보드 컴퓨터(예: 아두이노)에서 다양한 디지털 신호를 입출력할 수 있도록 제공되는 핀을 의미한다.   
Digital Input, Digital Output   
Analog Input, Analog Output   
   
✅ ADC   
Digital 장치는 Analog input을 받을 수 없기 때문에 Analog input을 Digital 신호로 바꿔주는 장치가 필요한데 이를 ADC(Analog to Digital Converter)라고 한다. DAC도 있는데 임베디드 mcu에선 없는 경우가 잦다. 그 이유는 
임베디드 장치는 보통 측정하는 기능을 하기 때문이다.
