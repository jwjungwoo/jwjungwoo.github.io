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
<https://www.arduino.cc/>에서 본인의 환경에 맞게 스케치를 다운해준다. 그 뒤 아래와 같이 세팅해준다.   
<img src="https://github.com/user-attachments/assets/cc718a89-d4dd-465e-a913-ae8d29a13fcc" width="600" height="400">   

# Arduino Practice
## pull down
<img src="https://github.com/user-attachments/assets/60d7c53a-9aab-47cc-aff5-64810ace96e8" width="500" height="400">   
✅ floating   
선을 연결 안 했다 가정하자. 우린 이 값을 0이라 생각할 수 있다. 하지만 실제론 그렇지 않고 값이 0, 1로 랜덤으로 반복된다. 위의 사진에서 8번 앞에 저항이 없다 가정하자. 스위치를 안 누른 상태에선 floating 현상이 발생한다. 따라서 
저항을 연결하여 8번이 floating 하는 것을 막는다. 또한 스위치를 누르면 당연히 저항으로 가지 않고 8번으로 전류가 잘 흐른다.

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

# pc에서 아두이노로 보내기
## 123 -> RGB
아두이노에 Easy Module Shield를 장착하고 pc에서 1,2,3을 누르면 r,g,b 불이 들어오게 한다.   
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
    Serial.begin(9600);
}

void loop() {
    char c= 0;
    if(Serial.available()) {
        c= Serial.read();
    }
    if(c=='1') {
        digitalWrite(R_PIN, HIGH); digitalWrite(G_PIN, LOW); digitalWrite(B_PIN, LOW); delay(100);
    }
    if(c=='2') {
        digitalWrite(R_PIN, LOW); digitalWrite(G_PIN, HIGH); digitalWrite(B_PIN, LOW); delay(100);
    }
    if (c == '3') {
        digitalWrite(R_PIN, LOW); digitalWrite(G_PIN, LOW); digitalWrite(B_PIN, HIGH); delay(100);
    }
    if(c=='4') {
        digitalWrite(R_PIN, LOW); digitalWrite(G_PIN, LOW); digitalWrite(B_PIN, LOW); delay(100);
    }
}
```

## Rotation A0로 밝기 조절
✅ pwm_val   

```c
#define VR_PIN A0
#define LED_PIN 9

int tmp_cnt = 0;
int adc_val = 0;

void setup() {
    Serial.begin(9600);
}

void loop() {
      adc_val= analogRead(VR_PIN);
    Serial.println("["+String(++tmp_cnt)+"]"+String(adc_val));

    int pwm_val = 0;
    pwm_val = map(adc_val, 0, 1023, 0, 255);
    analogWrite(LED_PIN, pwm_val);
    delay(100);
}
```   
<img src="https://github.com/user-attachments/assets/3eb8145e-0cad-4d5a-8305-dce9f73e71a4" width="500" height="400">   
우측 상단 serial 옆의 그래프를 누르고 아래 코드만 바꾸면 위의 사진과 같이 그래프로도 볼 수 있다.   
```c
Serial.println("["+String(++tmp_cnt)+"]"+String(adc_val));
Serial.println(String(adc_val));
```

## 조도 센서
조도 센서에 들어가는 빛의 양을 바꾸면 LED 값이 바뀐다.   
<img src="https://github.com/user-attachments/assets/1303fb12-87ff-4669-a07c-ecc718c9a293" width="500" height="400">   
```c

#define cds_pin (15)
#define g_pin (10)

void setup() {
    Serial.begin(9600);
    pinMode(g_pin, OUTPUT);
}

void loop() {
    int adc1_val= 0;
    int lux= 0;

    while(1) {
        adc1_val= analogRead(cds_pin);
        lux= map(adc1_val, 0, 1023, 255, 0);
        Serial.println(lux);
        analogWrite(g_pin, lux);
        delay(10);
    }
}
```

## 온도 습도
필요한 라이브러리를 다운 받아 arduino가 지정한 곳에 넣는다.   
<img src="https://github.com/user-attachments/assets/e4c82e64-ed67-41a5-ad1d-18e92e341444" width="500" height="400">   
```c
#include <DHT11.h>
int pin=4;
DHT11 dht11(pin); 
void setup()
{
   Serial.begin(9600);
  while (!Serial) {
      ; // wait for serial port to connect. Needed for Leonardo only
    }
}

void loop()
{
  int err;
  float temp, humi;
  if((err=dht11.read(humi, temp))==0)
  {
    Serial.print("temperature:");
    Serial.print(temp);
    Serial.print(" humidity:");
    Serial.print(humi);
    Serial.println();
  }
  else
  {
    Serial.println();
    Serial.print("Error No :");
    Serial.print(err);
    Serial.println();    
  }
  delay(DHT11_RETRY_DELAY); //delay for reread
}
```

# ETC
## Digital vs Analog
✅ GPIO   
GPIO는 범용 입출력(General Purpose Input/Output)의 약자로, mcu나 싱글보드 컴퓨터(예: 아두이노)에서 다양한 디지털 신호를 입출력할 수 있도록 제공되는 핀을 의미한다.   
Digital Input, Digital Output   
Analog Input, Analog Output   
   
✅ ADC   
Digital 장치는 Analog input을 받을 수 없기 때문에 Analog input을 Digital 신호로 바꿔주는 장치가 필요한데 이를 ADC(Analog to Digital Converter)라고 한다. DAC도 있는데 임베디드 mcu에선 없는 경우가 잦다. 그 이유는 
임베디드 장치는 보통 측정하는 기능을 하기 때문이다.
