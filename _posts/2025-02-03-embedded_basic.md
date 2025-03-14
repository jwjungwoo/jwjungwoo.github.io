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

# AVR
## AVR 기초
✅ 국내에서 80 ~ 90년대 마이크로컨트롤러 시장   
1. 8051 -> Intel   
2. PIC -> Microchip   
3. AVR -> Microchip
   
✅ 현재는 뭐가 많이 쓰일까?   
1. ARM MCU -> ST Microelectronics (ARM Cortex M3 기반)   
2. Infienon -> ARM Core X 독일회사

## 장치 관리자(AVR연결)
장치 관리자에서 몇 번 포트인지 확인한다.   
<img src="https://github.com/user-attachments/assets/23369bf3-4119-41a0-9197-8232547f2908" width="400" height="500">   

## 프로그램 설정

<https://www.microchip.com/en-us/tools-resources/archives/avr-sam-mcus> 여기서 가장 위에껄 다운 받은 뒤 아래와 같이 ATMega328p를 누른다.   
<img src="https://github.com/user-attachments/assets/ae4ccc3d-9c8f-4f77-acf2-fb79dd7c1eca" width="400" height="500">   
또한 처음엔 alt+f5가 안됨으로 다음과 같이 solution viewer로 들어가서   
<img src="https://github.com/user-attachments/assets/0e87391c-c993-4f1e-b1ea-f8a69cd16e78" width="600" height="400">   
아래와 같이 속성에 들어가 사진과 같이 설정해줘야한다.   
<img src="https://github.com/user-attachments/assets/e16b3db8-fc75-4b79-9813-ed24d392f379" width="600" height="400">   

## bit 연산

```c
#define F_CPU 8000000UL

#include <avr/io.h>
#include <util/delay.h>

#define LED_DELAY_TIME	(50) //ms

#define sbi(REG, n) (REG |=  (1<<n))
#define cbi(REG, n) (REG &= ~(1<<n))

int main(void) {
	sbi(DDRB, PINB5);
	cbi(PORTB, PINB5);
	
	while (1) {
		sbi(PORTB, PINB5);
		_delay_ms(LED_DELAY_TIME);
		cbi(PORTB, PINB5);
		_delay_ms(LED_DELAY_TIME);
	}
	return (0);
}
```   
   
가장 좋은건 아래와 같이 register를 숨기는 것이다.   
   
```c
#define F_CPU 16000000UL
#include <config.h> // avr, pic, stm을 설정한다.
#include <gpio_driver.h> // 추상화된 gpio_driver 헤더파일을 포함한다.

#define BLINK_DELAY_TIME	(20) //ms

int main(void) {
	gpio_int(PINB5, OUTPUT);// pinMode();
	gpio_write(PINB5, LOW);	 // digitalWrie();
	
	while (1) {
		gpio_write(PINB5, HIGH);
		delay_ms(BLINK_DELAY_TIME);		

		gpio_write(PINB5, LOW);
		delay_ms(BLINK_DELAY_TIME);		
	}
}
```

## 누르면 불 들어오게 하는 예재
```c
#define F_CPU 8000000UL

#include <avr/io.h>
#include <util/delay.h>

#define sbi(REG, n) (REG |=  (1<<n))
#define cbi(REG, n) (REG &= ~(1<<n))

#define LED_DIR_PORT  (DDRB)
#define LED_OUT_PORT  (PORTB)

#define LED_1_PIN (PINB5) // LED

#define KEY_DIR_PORT	(DDRB) // 버튼 방향 설정
#define KEY_IN_PORT		(PINB) // 버튼 입력 상태 읽기

#define KEY_PRESSED		(0) // Low
#define KEY_RELEASED	(1) // High

#define KEY_1_PIN		(PINB7) // 버튼이 연결된 핀


int main(void) {
	cbi(DDRB, PINB7); // 버튼을 입력 모드로 설정
	
	sbi(DDRB, PINB5); // LED는 출력 모드로 설정	
	cbi(PORTB, PINB5); // 초기 LED를 OFF(LOW) 상태로 설정
	
	uint8_t key_1_val= 0; // 키값을 받을 변수 하나 만들어 놓고
	
	while(1) {
		//key_1_val= (KEY_IN_PORT & (1<< KEY_1_PIN)) >> KEY_1_PIN; 
		key_1_val= (PINB >> PINB7);
		// 꼭 쉬프트 시켜줘야 0,1이 들어온다.
		// 그렇지 않으면 0x8?, 0x?? 값이 들어올수 있다.
		// ??는 PINB의 각 핀 상태에 따라서 달라지기 때문에 꼭 0이라고 말할수 없다.
		
		switch(key_1_val) {
			case 0: // 0 1이 아니라 0이다. Pull-up 회로이기 때문에
				sbi(PORTB, PINB5);
				break;
				
			case 1: // 1y
				cbi(PORTB, PINB5);
				break;				
		}
	}
	return (0);
}
```

## memory mapped io 예재
불 깜빡깜빡하게 하는 코드   
```c
#define F_CPU 8000000UL

//#include <avr/io.h>
#include <util/delay.h>

#define LED_DELAY_TIME	(1000) //ms

int main(void) {

	*(volatile unsigned int *)0x24 |=  (1<<5); // DDRB = 0x05;
	*(volatile unsigned int *)0x25 &= ~(1<<5); // cbi(PORTB, 5);
	
	while (1) {
		*(volatile unsigned int *)0x25 |= (1<<5);
		_delay_ms(LED_DELAY_TIME);

		*(volatile unsigned int *)0x25 &= ~(1<<5);
		_delay_ms(LED_DELAY_TIME);

	}
	return (0);
}
```

## Tera Term 이용하기
✅ Tera Term   
Tera Term은 serial 통신을 할 수 있게 해준다. 
✅ USART는 동기(Synchronous)와 비동기(Asynchronous) 모드를 모두 지원하는 직렬 통신 방식   
✅ UART는 USART의 하위 개념이며, 비동기 방식만 지원   
✅ AVR MCU에서는 UCSR0A, UCSR0B, UCSR0C, UBRR0H/L, UDR0 등을 통해 제어   
✅ 비동기 방식(UART)이 가장 일반적이며, 동기 방식(Synchronous Mode)은 속도가 더 빠름   

아래의 코드는 내 pc에서 누른걸 AVR이 +1 해서 보낸걸 출력해준다. 코드의 작동은 AVR에서 작동한다. AVR에서 receive하면 trasmit한다.   
<img src="https://github.com/user-attachments/assets/62f118e9-0c29-4b18-a335-3499a1014055" width="500" height="400">   
```c
//#define F_CPU 16000000UL
#define F_CPU 8000000UL
#define _BV(bit) (1<<bit)
#include <stdio.h>
#include <util/delay.h>
#include <avr/io.h>

void UART_0_init(void);
void UART1_transmit(char data);
unsigned char UART1_receive(void);

void UART_0_init(void){
	UBRR0H = 0x00;		// 9,600보율로 설정
	UBRR0L = 207;
	UCSR0A = UCSR0A | (1<<1);		// 2배속 모드(= _BV(U2X1))
	UCSR0C |= 0x06;
	
	UCSR0B |= _BV(RXEN0);		// 송수신 가능
	UCSR0B |= _BV(TXEN0);
}

void UART1_transmit(char data){
	while(!(UCSR0A & (1 << UDRE0))); // 데이터 송신을 기다림
	UDR0 = data;		//데이터 전송
}

unsigned char UART1_receive(void){
	while(!(UCSR0A & (1<<RXC0))); // 데이터 수신을 기다림
	return UDR0;
}

int main(void){
	UART_0_init();		// UART0 초기화
	
	while(1){
		UART1_transmit(UART1_receive()+1);
	}
	
	return 0;
}
```

## 1,0 -> led on,off
<img src="https://github.com/user-attachments/assets/0c5e8f2d-aecb-417b-9136-842c4d6bd7ad" width="500" height="400">   
<img src="https://github.com/user-attachments/assets/a90f77bf-817d-44db-acfe-410d9801addd" width="500" height="400">   
<img src="https://github.com/user-attachments/assets/979625e2-047f-43c3-ac7a-07aa5f7996e8" width="500" height="400">   
✅ DDR = Data Direction Register   
0이면 입력, 1이면 출력   
DDRA (A 포트 DDR)   
ex. DDRA = 0x1F (5번째 이하는 전부 출력으로 쓸거라고 말해줌)   
ex. DDRB = 0x04 (2번쩨 led 전부 끄려함   
   
```c
#define F_CPU 8000000UL  // 8MHz CPU 클럭 설정
#include <avr/io.h>
#include <util/delay.h>
#include <stdio.h>

#define sbi(REG, n) (REG |=  (1 << n))  // 특정 비트를 1로 설정 (SET BIT)
#define cbi(REG, n) (REG &= ~(1 << n))  // 특정 비트를 0으로 설정 (CLEAR BIT)

#define LED_DIR_PORT  DDRB   // LED 방향 설정
#define LED_OUT_PORT  PORTB  // LED 출력 설정
#define LED_1_PIN     PINB5  // LED 핀 (PB5)

// UART 초기화 함수 (9600bps 설정)
void UART_0_init(void) {
    UBRR0H = 0x00;  
    UBRR0L = 207;  // 9600bps (8MHz 기준)
    UCSR0A = UCSR0A | (1 << U2X0);  // 2배속 모드 설정 (U2X0 비트 활성화)
    UCSR0C |= 0x06;  // 비동기 모드, 8비트 데이터, 패리티 없음, 1 스톱 비트

    UCSR0B |= (1 << RXEN0);  // UART 수신 활성화
    UCSR0B |= (1 << TXEN0);  // UART 송신 활성화
}

// UART 송신 함수 (1바이트 전송)
void UART1_transmit(char data) {
    while (!(UCSR0A & (1 << UDRE0))); // 송신 가능할 때까지 대기
    UDR0 = data;  // 데이터 전송
}

// UART 문자열 전송 함수
void UART_send_string(const char *str) {
    while (*str) {
        UART1_transmit(*str++); // ++는 포인터 연산으로 str 포인터를 다음 문자로 이동시킨다.
    }
}

// UART 수신 함수 (1바이트 수신)
char UART1_receive(void) {
    while (!(UCSR0A & (1 << RXC0))); // 데이터 수신 대기
    return UDR0;
}

int main(void) {
    char received_char;
    
    sbi(DDRB, LED_1_PIN); // LED 핀을 출력으로 설정
    cbi(PORTB, LED_1_PIN);  // LED 초기 상태 OFFx

    UART_0_init();  // UART 초기화
    UART_send_string("shell> ");  // 초기 프롬프트 출력

    while (1) {
        received_char = UART1_receive();  // UART 데이터 수신
        UART1_transmit(received_char);  // 입력된 문자 Echo 출력

        // LED 제어 (1을 입력하면 LED ON, 0을 입력하면 LED OFF)
        if (received_char == '1') {
            sbi(PORTB, LED_1_PIN);  // LED ON
        } else if (received_char == '0') {
            cbi(PORTB, LED_1_PIN);  // LED OFF
        }

        UART_send_string("\r\nshell> ");  // 새로운 프롬프트 출력
    }
    
    return 0;
}
```

# Arduino Practice

## pull down

<img src="https://github.com/user-attachments/assets/60d7c53a-9aab-47cc-aff5-64810ace96e8" width="500" height="400">   
✅ floating   
선을 5V와 연결 안 했다 가정하자. 우린 이 값을 0이라 생각할 수 있다. 하지만 실제론 그렇지 않고 값이 0, 1로 랜덤으로 반복된다. 이 현상을 floating 이라한다. 위의 사진에서 8번 앞에 저항이 없다 가정하자. 
스위치를 안 누른 상태에선 floating 현상이 발생한다. 따라서 저항을 연결하여 8번이 floating 하는 것을 막는다. 또한 스위치를 누르면 당연히 저항으로 가지 않고 8번으로 전류가 잘 흐른다.

## RGB LED 점멸하기
<img src="https://github.com/user-attachments/assets/e9806faa-65f5-4da6-bbb8-b34aa80944c9" width="500" height="400">   
다음과 같이 코하면, 삼원색과 그 조합의 색들이 순차적으로 빛난다. (보라색이 영롱했다.)   
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
    digitalWrite(R_PIN, HIGH); digitalWrite(G_PIN, LOW); digitalWrite(B_PIN, HIGH); delay(100); // 보라!
    digitalWrite(R_PIN, LOW); digitalWrite(G_PIN, HIGH); digitalWrite(B_PIN, LOW); delay(100);
    digitalWrite(R_PIN, LOW); digitalWrite(G_PIN, HIGH); digitalWrite(B_PIN, HIGH); delay(100);
    digitalWrite(R_PIN, LOW); digitalWrite(G_PIN, LOW); digitalWrite(B_PIN, HIGH); delay(100);
    digitalWrite(R_PIN, HIGH); digitalWrite(G_PIN, HIGH); digitalWrite(B_PIN, HIGH); delay(100);
}
```

## 아두이노에서 pc로 보내기
<img src="https://github.com/user-attachments/assets/09b683e6-f6bf-446c-9405-fd0027bd1656" width="500" height="400">   
우측 상단에 돋보기 모양의 serial 버튼을 누르고 실행하면 하단에 serial 통신화면이 나온다.   
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
pwm_val은 아날로그 입력 값 (adc_val)을 PWM 출력 값으로 변환한 변수이다.   
easy shield의 파란색 조절기를 통해 가변저항(VR, Variable Resistor)의 값을 바꾸고, 아래의 코드는 이를 읽어서 LED 밝기를 조절하는 기능을 한다.   
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
필요한 라이브러리를 다운 받아 arduino 파일이 지정된 주소에 넣은 뒤 아래의 코드를 작동한다.   
<img src="https://github.com/user-attachments/assets/e4c82e64-ed67-41a5-ad1d-18e92e341444" width="500" height="400">   
```c
#include <DHT11.h>
int pin = 4;
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

# Timer
## 초 재기
<img src="https://github.com/user-attachments/assets/2b7d4e9b-472b-4d1b-b080-45c5a1a05917" width="500" height="400">   
위의 사진과 같이 새로 file을 만들고 timer관련 파일을 같이 놓는다.   
```c
#include "SimpleTimer.h"

SimpleTimer timer;

#define LED_1_PIN    (13)
#define TIMER_INTERVAL   1000

void my_func() {
    static int val= 1;
    Serial.print("Uptime(s): ");
    Serial.println(millis() / 1000);
    digitalWrite(LED_1_PIN, val);
    val ^= 0x01;
}

void setup() {
    Serial.begin(9600);
    pinMode(LED_1_PIN, OUTPUT);
    timer.setInterval(TIMER_INTERVAL, my_func); // 1초마다 reapeatMe 함수를 호출..
}
void loop() {
    //timer.update();
    timer.run();
}
```   
<img src="https://github.com/user-attachments/assets/00e64c07-6d91-4756-81c1-22ca326cc21c" width="500" height="400">   
실행하면 다음과같이 시간이 측정된다.   

## 시간에 따른 함수 호출
<img src="https://github.com/user-attachments/assets/360b7aec-a863-45df-b1ce-437a6f2a64dd" width="600" height="400">   
```c
#include "SimpleTimer.h"
SimpleTimer timer;
#define LED_1_PIN    (13)

#define TASK_1 2000
#define TASK_2 3000
#define TASK_3 5000
int tick = 0;
int tmp_cnt = 0;
void my_func1() {
    Serial.print("2 ");
}
void my_func2() {
    Serial.print("3 ");
}
void my_func3() {
    Serial.print("5 ");
}

void my_func() {
    Serial.print("["+String(++tmp_cnt)+"]");
    tick++;
    if (tick % (TASK_1/ 1000) == 0) {
        my_func1();
    }
    if (tick % (TASK_2/ 1000) == 0) {
        my_func2();
    }
    if (tick % (TASK_3/ 1000) == 0) {
        my_func3();
    }
    Serial.println();
}
void setup() {
    Serial.begin(9600);
    pinMode(LED_1_PIN, OUTPUT);
    timer.setInterval(1000, my_func);
}
void loop() {
    //timer.update();
    timer.run();
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

## motor
<img src="https://github.com/user-attachments/assets/172721e3-ce89-484f-b455-d7528d0abf02" width="500" height="600">   
모터 돌리는 코드   
```c
// define Motor Driver Pins
#define MOTOR_DIR_PIN (12)
#define MOTOR_PWM_PIN (3)
#define MOTOR_BRK_PIN (9)

// define Motor Direction
#define CW LOW
#define CCW HIGH

// define Brake On/Off
#define BRAKE_ON HIGH
#define BRAKE_OFF LOW

void setup() {
    Serial.begin(9600);
    pinMode(MOTOR_DIR_PIN, OUTPUT); // 모터 제어 핀 설정
    pinMode(MOTOR_PWM_PIN, OUTPUT);
    pinMode(MOTOR_BRK_PIN, OUTPUT);
    
    digitalWrite(MOTOR_DIR_PIN, HIGH); // 초기에는 모터 정지
    digitalWrite(MOTOR_PWM_PIN, 0);
    digitalWrite(MOTOR_BRK_PIN, HIGH);
    delay(1000);
}

void loop() {
    digitalWrite(MOTOR_DIR_PIN, CW); // 시계 방향 회전
    digitalWrite(MOTOR_BRK_PIN, BRAKE_OFF); //release breaks
    analogWrite(MOTOR_PWM_PIN, 255); //set work duty for the motor
    Serial.println("motor cw rotate");
    delay(2000);
    
    digitalWrite(MOTOR_BRK_PIN, HIGH);//activate breaks
    Serial.println("motor brake on");
    delay(500);

    digitalWrite(MOTOR_DIR_PIN, CCW); // 반시계 방향 회전
    digitalWrite(MOTOR_BRK_PIN, BRAKE_OFF); //release breaks
    analogWrite(MOTOR_PWM_PIN, 255); //set work duty for the motor to 0 (off)
    Serial.println("motor ccw rotate");
    delay(2000);

    digitalWrite(MOTOR_BRK_PIN, BRAKE_OFF);//activate breaks
    Serial.println("motor brake on");
    delay(500);    
}
```

## 통신
✅ 동기/비동기   
Synchronous / Asynchronous   
동기ex. 광통신을 할 때 데이터만 보내면 이게 11111000인지 11110000인지 구분하기 힘들다. 이때 data와 함께 clk을 보낸다.   
비동기ex. 동기와 다르게 clk이 없다. 따라서 data 1bit을 보내는 시간을 약속한다. (내가 1초마다 1bit를 보낼게)   

