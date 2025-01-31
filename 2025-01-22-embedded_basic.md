---
layout: single
title:  "임베디드 소프트웨어 개발 기초"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# Arduino
## 설치 및 세팅
<https://www.arduino.cc/en/software>에서 설치한다.   
<img src="https://github.com/user-attachments/assets/e91752d6-2fe7-44a3-ab45-d45589df0c2f" width="600" height="400">   
작업관리자에 들어가 연결된 아두이노 포트넘버를 확인할 수 있으며, Tools에서 Board와 Port를 위의 사진과 같이 설정하면 된다.   

## 아두이노 불 깜빡이는 예제
```c
#define LED_PIN (13)
#define LED_DELAY_TIME (500)

void setup() {
    pinMode(LED_PIN, OUTPUT);
}

void loop() {
    digitalWrite(LED_PIN, HIGH);
    delay(LED_DELAY_TIME);
    digitalWrite(LED_PIN, LOW);
    delay(LED_DELAY_TIME);
}
```

# 임베디드 기초
## 전압과 전류
전압: V (볼타 발견)   
전류: A (앙페르 발견)   

✅ 어떤 어댑터를 골라야 할까?   
전압은 무조건 같게 하고, 전류는 원하는 거 이상으로 해주면 됨. 다만 암페어는 높을수록 비싸므로 최대한 작게 맞춰주자. 
장치가 원하는게 220V, 2A면 무조건 220V로 맞춰주고 2A 이상이면 된다.   
   
✅ LED   
꼬마전구는 방향이 있고, LED는 방향이 없기 때문에 꼬마전구는 +, -를 어디에 꽂든 상관 없지만, LED는 상관이 있다.   ㅁ--
layout: single
title:  "임베디드 소프트웨어 개발 기초"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# Arduino
## 설치 및 세팅
<https://www.arduino.cc/en/software>에서 설치한다.   
<img src="https://github.com/user-attachments/assets/e91752d6-2fe7-44a3-ab45-d45589df0c2f" width="600" height="400">   
작업관리자에 들어가 연결된 아두이노 포트넘버를 확인할 수 있으며, Tools에서 Board와 Port를 위의 사진과 같이 설정하면 된다.   

## 아두이노 불 깜빡이는 예제
```c
#define LED_PIN (13)
#define LED_DELAY_TIME (500)

void setup() {
    pinMode(LED_PIN, OUTPUT);
}

void loop() {
    digitalWrite(LED_PIN, HIGH);
    delay(LED_DELAY_TIME);
    digitalWrite(LED_PIN, LOW);
    delay(LED_DELAY_TIME);
}
```

# 임베디드 기초
## 전압과 전류
전압: V (볼타 발견)   
전류: A (앙페르 발견)   

✅ 어떤 어댑터를 골라야 할까?   
전압은 무조건 같게 하고, 전류는 원하는 거 이상으로 해주면 됨. 다만 암페어는 높을수록 비싸므로 최대한 작게 맞춰주자. 
장치가 원하는게 220V, 2A면 무조건 220V로 맞춰주고 2A 이상이면 된다.   
   
✅ LED   
꼬마전구는 방향이 있고, LED는 방향이 없기 때문에 꼬마전구는 +, -를 어디에 꽂든 상관 없지만, LED는 상관이 있다.   ㅁ

## 임베디드 시스템이란?
✅ 컴퓨터 시스템 vs 임베디드 시스템   
둘을 가르는 기준은 '범용성'이다. 임베디드 시스템은 만들 때부터 특정 기능을 하도록 설계된다.   
   
✅ MCU   
<img src="https://github.com/user-attachments/assets/01ebae03-c93a-4ce7-aa33-8e54bf0f437f" width="800" height="300">   

## Native vs Cross
✅ Native 개발환경   
개발환경과 작동환경이 동일.   
✅ Cross 개발환경   
개발환경과 작동환경이 동일하지 않음.   

## 임베디드 개발 환경
✅ 3가지
Target Board, Programmer, PC로 구성된다. 오른쪽에서 왼쪽으로 코드가 흐른다. 여기서 Programmer에는 두 종류가 있는데 다운로드만 되는 것, debug가 되는 것으로 나뉜다.   

## LED 5개 차례로 깜빡이는 예제
```c
#define LED_1_PIN (13)
#define LED_2_PIN (12)
#define LED_3_PIN (11)
#define LED_4_PIN (10)
#define LED_5_PIN (9)

#define LED_DELAY_TIME (500)

void setup() {
    pinMode(LED_1_PIN, OUTPUT);
    pinMode(LED_2_PIN, OUTPUT);
    pinMode(LED_3_PIN, OUTPUT);
    pinMode(LED_4_PIN, OUTPUT);
    pinMode(LED_5_PIN, OUTPUT);
}

void loop() {
    digitalWrite(LED_1_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_1_PIN, LOW); delay(LED_DELAY_TIME);
    digitalWrite(LED_2_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_2_PIN, LOW); delay(LED_DELAY_TIME);
    digitalWrite(LED_3_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_3_PIN, LOW); delay(LED_DELAY_TIME);
    digitalWrite(LED_4_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_4_PIN, LOW); delay(LED_DELAY_TIME);
    digitalWrite(LED_5_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_5_PIN, LOW); delay(LED_DELAY_TIME);
}
```

## 스위치 눌리면 깜빡이는 예제 (pull down으로)
✅ floating 현상   
스위치와 가장 가까운 갈색선이 저항으로 GND와 연결되있지 않았으면 floating 현상으로 인해 1,0 값이 랜덤으로 바뀐다. 이를 방지하고자 저항을 연결했다.   
<img src="https://github.com/user-attachments/assets/f56e948a-947f-48f0-8a07-01d520c18c7d" width="600" height="600">   
```c
#define LED_1_PIN (4)
#define LED_2_PIN (3)
#define LED_3_PIN (2)
#define LED_4_PIN (1)
#define LED_5_PIN (0)
#define KEY_1_PIN (8)
#define LED_DELAY_TIME (500)

void setup() {
    pinMode(LED_1_PIN, OUTPUT);
    pinMode(LED_2_PIN, OUTPUT);
    pinMode(LED_3_PIN, OUTPUT);
    pinMode(LED_4_PIN, OUTPUT);
    pinMode(LED_5_PIN, OUTPUT);

    pinMode(KEY_1_PIN, INPUT);
}

void loop() {

    char key_1 = HIGH;
    char key_2 = 1;

    key_1 = digitalRead(KEY_1_PIN);
    if (key_1 == HIGH) {
      digitalWrite(LED_1_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_1_PIN, LOW); delay(LED_DELAY_TIME);
      digitalWrite(LED_2_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_2_PIN, LOW); delay(LED_DELAY_TIME);
      digitalWrite(LED_3_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_3_PIN, LOW); delay(LED_DELAY_TIME);
      digitalWrite(LED_4_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_4_PIN, LOW); delay(LED_DELAY_TIME);
      digitalWrite(LED_5_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_5_PIN, LOW); delay(LED_DELAY_TIME);
    }
}
```

## 쉴드로 누르면 불 들어오게하는 예제
```

# 임베디드 기초
## 전압과 전류
전압: V (볼타 발견)   
전류: A (앙페르 발견)   

✅ 어떤 어댑터를 골라야 할까?   
전압은 무조건 같게 하고, 전류는 원하는 거 이상으로 해주면 됨. 다만 암페어는 높을수록 비싸므로 최대한 작게 맞춰주자. 
장치가 원하는게 220V, 2A면 무조건 220V로 맞춰주고 2A 이상이면 된다.   
   
✅ LED   
꼬마전구는 방향이 있고, LED는 방향이 없기 때문에 꼬마전구는 +, -를 어디에 꽂든 상관 없지만, LED는 상관이 있다.   ㅁ

## 임베디드 시스템이란?
✅ 컴퓨터 시스템 vs 임베디드 시스템   
둘을 가르는 기준은 '범용성'이다. 임베디드 시스템은 만들 때부터 특정 기능을 하도록 설계된다.   
   
✅ MCU   
<img src="https://github.com/user-attachments/assets/01ebae03-c93a-4ce7-aa33-8e54bf0f437f" width="800" height="300">   

## Native vs Cross
✅ Native 개발환경   
개발환경과 작동환경이 동일.   
✅ Cross 개발환경   
개발환경과 작동환경이 동일하지 않음.   

## 임베디드 개발 환경
✅ 3가지
Target Board, Programmer, PC로 구성된다. 오른쪽에서 왼쪽으로 코드가 흐른다. 여기서 Programmer에는 두 종류가 있는데 다운로드만 되는 것, debug가 되는 것으로 나뉜다.   

## LED 5개 차례로 깜빡이는 예제
```c
#define LED_1_PIN (13)
#define LED_2_PIN (12)
#define LED_3_PIN (11)
#define LED_4_PIN (10)
#define LED_5_PIN (9)

#define LED_DELAY_TIME (500)

void setup() {
    pinMode(LED_1_PIN, OUTPUT);
    pinMode(LED_2_PIN, OUTPUT);
    pinMode(LED_3_PIN, OUTPUT);
    pinMode(LED_4_PIN, OUTPUT);
    pinMode(LED_5_PIN, OUTPUT);
}

void loop() {
    digitalWrite(LED_1_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_1_PIN, LOW); delay(LED_DELAY_TIME);
    digitalWrite(LED_2_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_2_PIN, LOW); delay(LED_DELAY_TIME);
    digitalWrite(LED_3_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_3_PIN, LOW); delay(LED_DELAY_TIME);
    digitalWrite(LED_4_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_4_PIN, LOW); delay(LED_DELAY_TIME);
    digitalWrite(LED_5_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_5_PIN, LOW); delay(LED_DELAY_TIME);
}
```

## 스위치 눌리면 깜빡이는 예제 (pull down으로)
✅ floating 현상   
스위치와 가장 가까운 갈색선이 저항으로 GND와 연결되있지 않았으면 floating 현상으로 인해 1,0 값이 랜덤으로 바뀐다. 이를 방지하고자 저항을 연결했다.   
<img src="https://github.com/user-attachments/assets/f56e948a-947f-48f0-8a07-01d520c18c7d" width="600" height="600">   
```c
#define LED_1_PIN (4)
#define LED_2_PIN (3)
#define LED_3_PIN (2)
#define LED_4_PIN (1)
#define LED_5_PIN (0)
#define KEY_1_PIN (8)
#define LED_DELAY_TIME (500)

void setup() {
    pinMode(LED_1_PIN, OUTPUT);
    pinMode(LED_2_PIN, OUTPUT);
    pinMode(LED_3_PIN, OUTPUT);
    pinMode(LED_4_PIN, OUTPUT);
    pinMode(LED_5_PIN, OUTPUT);

    pinMode(KEY_1_PIN, INPUT);
}

void loop() {

    char key_1 = HIGH;
    char key_2 = 1;

    key_1 = digitalRead(KEY_1_PIN);
    if (key_1 == HIGH) {
      digitalWrite(LED_1_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_1_PIN, LOW); delay(LED_DELAY_TIME);
      digitalWrite(LED_2_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_2_PIN, LOW); delay(LED_DELAY_TIME);
      digitalWrite(LED_3_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_3_PIN, LOW); delay(LED_DELAY_TIME);
      digitalWrite(LED_4_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_4_PIN, LOW); delay(LED_DELAY_TIME);
      digitalWrite(LED_5_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_5_PIN, LOW); delay(LED_DELAY_TIME);
    }
}
```

## 쉴드로 누르면 불 들어오게하는 예제
<img src="https://github.com/user-attachments/assets/b4216871-743d-4f7e-ae43-0f1d90adc0de" width="600" height="600">   
✅ 빨강, 파랑 led   
```c
#define LED_1_PIN 13
#define LED_2_PIN 12
#define LED_DELAY_TIME 50

#define KEY_1_PIN 2
#define KEY_2_PIN 3

void setup() {
    pinMode(LED_1_PIN, OUTPUT);
    pinMode(LED_2_PIN, OUTPUT);
    
    digitalWrite(LED_1_PIN, LOW);
    digitalWrite(LED_2_PIN, LOW);

    pinMode(KEY_1_PIN, INPUT);
    pinMode(KEY_2_PIN, INPUT);
}

void loop() {

    uint8_t key_1 = 0;
    uint8_t key_2 = 0;

    key_1 = digitalRead(KEY_1_PIN);
    key_2 = digitalRead(KEY_2_PIN);

    if (key_1 == LOW) {
      digitalWrite(LED_1_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_1_PIN, LOW); 
    }
    if (key_2 == LOW) {
      digitalWrite(LED_2_PIN, HIGH); delay(LED_DELAY_TIME); digitalWrite(LED_2_PIN, LOW);
    }
}
```

✅ 삼원색   
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
