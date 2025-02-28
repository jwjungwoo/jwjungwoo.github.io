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
GPU: Graphic Processing Unit (그래픽을 사용할 때 좌표점들을 색깔별로 다 구분해줌. 행렬 연산으로 +,- 빠르게 해줌)   
   
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
<img src="https://github.com/user-attachments/assets/a8ce6305-f854-4e54-bc5a-26b3bac3d9a6" width="600" height="600">   

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
<img src="https://github.com/user-attachments/assets/b6f55246-f974-4ba0-88d0-ed65965f2f2a" width="600" height="450">   
reset 버튼(주소를 0번지로 간다.) -> Boot Loader -> Reset handler(하드웨어 초기화 작업) -> C start up code -> Application   
   
✅ reset   
<img src="https://github.com/user-attachments/assets/0405445c-bd24-4525-8ffd-8d1882c02e56" width="600" height="450">   
reset 버튼을 누르면 맨처음에 Main Stack Point가 들어온다. 그 다음은 Reset vector가 들어간다.   
Reset hanlder: SystemInit을 하고, 끝나면 main을 실행한다.

## 실습
✅ 기본 정보   
hal driver는 지금 안 쓸 것이다.   
startup_stm32l073xx.s, main.c, system_stm32l0xx.c 만 냅두면 됨.   
RW-data: Read Write data, ZI-data: compiler가 써주는 data.   
HD_01.hex: nucleo보드 실행 파일.   
HD_01.sct: Rom, RAM 정보 적힘.   
```c
// volatile을 붙이면 컴파일러가 무시하지 않고 무조건 동작하게 한다.
volatile int counter = 0;
```
<img src="https://github.com/user-attachments/assets/aefd1f35-a59f-456b-a1d9-f0b46433274a" width="600" height="460">   

# Digital Output
## MCU Bring-up
✅ MCU Bring-up   
firmware 를 통해 MCU 가 기본적으로 동작하는 것을 의미햔다.(개발자들은 "눈뜬다"라고 표현)   
   
✅ Firmware   
하드웨어와 밀접하게 관련되어 있는 sw   
sw 위치: ROM/ RAM 등에 위치   
기능이 바뀌면 통으로 바꿔야한다. 기본 hw를 동작시켜 MCU를 기본 동작이 가능하게 만드는 sw   
   
✅ pull up, pull down   
pull up을 많이 쓰긴 한다.   

✅ 출력   
<img src="https://github.com/user-attachments/assets/28b7e732-8474-472c-9e74-d5eb340fafda" width="600" height="470">   
예를들어 LED 같은 경우는 00으로 하면 된다.   
   
✅ 0X5000 0000: GPIOA의 시작 주소   
<img src="https://github.com/user-attachments/assets/a72924e3-a4ea-48b4-8399-f403fca9095f" width="600" height="330">   

## GPIO
✅ GPIO 가능 모드   
GPIO 모드는 input, ouput, alternative 모드로 총 3 종류다.   

# 실습
## 불 키기
<img src="https://github.com/user-attachments/assets/1556f8e2-d99b-4903-9be9-f9564b848ed6" width="600" height="550">   
```c
int main (void)
{
	// RCC_GPIOA Clock En; RCC register begins from 0x4002 1000 
	*((unsigned int*)0x4002102C) = (unsigned int)0x01; // 40021000 + 2C 

	// GPIOA Register           
	// moder. A-port begins from 0x5000 0000. 주소의 시작번지가 mode임.
	*((unsigned int*)0x50000000U) = 0xEBFFF4FF; // GPIO Output Mode setting register was initialed 0xEBFF FCFF; and it should be 0xEBFFF4FF
	// OTYPER:  output type register
	*((unsigned int*)(0x50000000U + 0x00000004U)) = 0;
	// OSPEEDR: output speed register   00: low, 01: medium, 10: high, 11: very high
	*((unsigned int*)(0x50000000U + 0x00000008U)) = (unsigned int)0x0C000C00;
	// pull up, pull down
	*((unsigned int*)(0x50000000U + 0x0000000CU)) = (unsigned int)0x24000000;
	
	// LED2(PA5) on
	// ODR
	*((unsigned int*)(0x50000000U + 0x00000014U)) = (unsigned char) 0x20;
	while(1) 
	{
		;
	}
}
```

## 불 깜빡이게 하기
```c
int main (void)
{
	// RCC_GPIOA Clock En; RCC register begins from 0x4002 1000 
	*((unsigned int*)0x4002102C) = (unsigned int)0x01; // 40021000 + 2C 

	// GPIOA Register           
	// moder. A-port begins from 0x5000 0000 
	*((unsigned int*)0x50000000U) = 0xEBFFF4FF; // GPIO Output Mode setting register was initialed 0xEBFF FCFF; and it should be 0xEBFF F4FF
	// OTYPER
	*((unsigned int*)(0x50000000U + 0x00000004U)) = 0;
	// OSPEEDR
	*((unsigned int*)(0x50000000U + 0x00000008U)) = (unsigned int)0x0C000400; // speed
	// pull up, pull down
	*((unsigned int*)(0x50000000U + 0x0000000CU)) = (unsigned int)0x24000000;
	volatile int counter = 0; // volatile을 붙여야한다.
	while(1) 
	{
			// on ODR
	  *((unsigned int*)(0x50000000U + 0x00000014U)) = (unsigned int) 0x20;
		counter = 0;
		while (counter < 0x20000) {
			counter++;
		}
			// off ODR
	  *((unsigned int*)(0x50000000U + 0x00000014U)) = (unsigned int) 0x0;
		counter = 0;
		while (counter < 0x20000) {
			counter++;
		}			
	}
}
```

## 불 깜빡이게 하기(define사용)
```c
//Define
//#define RCC_GPIOA (*((unsigned char*)0x4002102C))
#define PERIPH_BASE       (0x40000000UL) /*!< Peripheral base address in the alias region */
#define AHBPERIPH_BASE    (PERIPH_BASE + 0x00020000UL)
#define RCC_BASE          (AHBPERIPH_BASE + 0x00001000UL)
#define RCC_GPIOA 				*((volatile unsigned int*)(RCC_BASE + 0x0000002CUL))

#define GPIO_PA5PIN_BASE (unsigned int)0x50000000U
#define GPIO_PA5PIN_MODE 	(*((volatile unsigned int*)GPIO_PA5PIN_BASE))
#define GPIO_PA5PIN_OTYPE (*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x00000004UL)))
#define GPIO_PA5PIN_OSPEEDR (*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x00000008UL)))
#define GPIO_PA5PIN_OPUPDR (*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x0000000CU)))
#define GPIO_PA5PIN_IDR    (*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x00000010U)))
#define GPIO_PA5PIN_ODR 	(*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x00000014UL)))

int main(void)
{
	volatile int counter = 0;

	//RCC-GPIOA
	RCC_GPIOA = (unsigned int)0x01;
	
	//GPIOA-Reg
	// Mode
	GPIO_PA5PIN_MODE = (unsigned int)0xEBFFF4FF;

	//OTYPER
	GPIO_PA5PIN_OTYPE = 0;

	//OSPEEDR
	GPIO_PA5PIN_OSPEEDR = (unsigned int)0x0C000C00;
	
	//PULL UP/DOWN
	GPIO_PA5PIN_OPUPDR = (unsigned int)0x24000000;

	while(1)
	{	
		//ODR 
	 	GPIO_PA5PIN_ODR = (unsigned int)0x20;
		
		counter = 0;
		while(counter < 0x20000) //delay loop
		{
			counter++;
		}
		
		//ODR 
	  	GPIO_PA5PIN_ODR = (unsigned int)0x0;	
		
		counter = 0;
		while(counter < 0x20000U) //delay loop
		{
			counter++;
		}
	}
}
```

## 불 깜빡이게 하기(PA5 레지만 변경)
```c
//Define
//#define RCC_GPIOA (*((unsigned char*)0x4002102C))
#define PERIPH_BASE       (0x40000000UL) /*!< Peripheral base address in the alias region */
#define AHBPERIPH_BASE    (PERIPH_BASE + 0x00020000UL)
#define RCC_BASE          (AHBPERIPH_BASE + 0x00001000UL)
#define RCC_GPIOA 				*((volatile unsigned int*)(RCC_BASE + 0x0000002CUL))

#define GPIO_PA5PIN_BASE (unsigned int)0x50000000U
#define GPIO_PA5PIN_MODE 	(*((volatile unsigned int*)GPIO_PA5PIN_BASE))
#define GPIO_PA5PIN_OTYPE (*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x00000004UL)))
#define GPIO_PA5PIN_OSPEEDR (*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x00000008UL)))
#define GPIO_PA5PIN_OPUPDR (*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x0000000CU)))
#define GPIO_PA5PIN_IDR    (*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x00000010U)))
#define GPIO_PA5PIN_ODR 	(*((volatile unsigned int*)(GPIO_PA5PIN_BASE+0x00000014UL)))

#define GPIO_2BIT_POS_MASK    ((unsigned int)0x00000003U) // 2 bit를 움직일 거면 11을 shift 하고 00으로 반전 시킬거임
#define GPIO_1BIT_POS_MASK		((unsigned int)0x00000001U) // U는 unsigned long 이다.

#define GPIO_PIN_5_POS		5

// delay function
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

int main(void)
{
	unsigned int position = GPIO_PIN_5_POS; //Pin Position: PA5
	unsigned int temp = 0x0U;
	unsigned int mode = 0x0U;
	unsigned int reg = 0x0U;
	
	//RCC-GPIOA
	reg = RCC_GPIOA;
	RCC_GPIOA = (unsigned int)(reg | 0x01U);

  // mode, speed 는 2 bit이다.

	// PA5 Mode
	// GPIO_PA5PIN_MODE = (unsigned int)0xEBFFF4FF;
	reg = GPIO_PA5PIN_MODE;
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF; 맨 처음에 11을 5개의 register만큼(5*2) shift했다. 그리고 반전 시키고..
	reg &= temp; // 반전 시키고 & 연산 했다.
	// GPO Mode: 01
	mode = 0x01U << (position*2U) ;	// GPIO OutPut Mode - 2bit 0x00000400
	reg |= mode;
	GPIO_PA5PIN_MODE = reg;

	//PA5 OTYPER
	//GPIO_PA5PIN_OTYPE = 0;
	reg = GPIO_PA5PIN_OTYPE;
	temp = ~(GPIO_1BIT_POS_MASK << position);	//~0x20 ==> 0xFFFFFFDF
	reg &= temp;
	mode = 0x0U << position ;	//Output type - 1bit
	reg |= mode;
	GPIO_PA5PIN_OTYPE = reg;

	//PA5 OSPEEDR
	//GPIO_PA5PIN_OSPEEDR = (unsigned int)0x0C000C00;
	reg = GPIO_PA5PIN_OSPEEDR;
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = 0x03U << (position*2U) ;	//Very High Mode - 2bit 0x00000C00
	reg |= mode;
	GPIO_PA5PIN_OSPEEDR = reg;
	
	//PULL UP/DOWN
	//GPIO_PA5PIN_OPUPDR = (unsigned int)0x24000000;
	reg = GPIO_PA5PIN_OPUPDR;
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = 0x0U << (position*2U) ;	//No Pull Up - Pull Down - 2bit 0x24000000
	reg |= mode;
	GPIO_PA5PIN_OPUPDR = reg;
	
	//ODR - 1bit
	//GPIO_PA5PIN_ODR = (unsigned char)0x20;
	reg = GPIO_PA5PIN_ODR;
	temp = ~(GPIO_1BIT_POS_MASK << position);	//~0x20 ==> 0xFFFFFFDF
	reg &= temp;

	//LED On value
	mode = 0x01 << position ;

	GPIO_PA5PIN_ODR = reg | mode;

		
	while(1)
	{
		//ODR 
		//LED Off value
	 	mode = ~mode&(GPIO_1BIT_POS_MASK << position);
		GPIO_PA5PIN_ODR = reg | mode;

		delay(0x20000);

	}
}
```

## 불 깜빡이게 하기(구조체)
```c
//Structure & Array   구조체 선언할 때 변수들의 순서는 절대 바꾸면 안 된다. 순서대로 메모리 주소에 매핑되기 때문이다.
typedef struct  
{
  volatile unsigned int MODER;        /*!< GPIO port mode register,                     Address offset: 0x00 */
  volatile unsigned int OTYPER;       /*!< GPIO port output type register,              Address offset: 0x04 */
  volatile unsigned int OSPEEDR;      /*!< GPIO port output speed register,             Address offset: 0x08 */
  volatile unsigned int PUPDR;        /*!< GPIO port pull-up/pull-down register,        Address offset: 0x0C */
  volatile unsigned int IDR;          /*!< GPIO port input data register,               Address offset: 0x10 */
  volatile unsigned int ODR;          /*!< GPIO port output data register,              Address offset: 0x14 */
  volatile unsigned int BSRR;         /*!< GPIO port bit set/reset registerBSRR,        Address offset: 0x18 */
  volatile unsigned int LCKR;         /*!< GPIO port configuration lock register,       Address offset: 0x1C */
  volatile unsigned int AFR[2];       /*!< GPIO alternate function register,            Address offset: 0x20-0x24 */
  volatile unsigned int BRR;          /*!< GPIO bit reset register,                     Address offset: 0x28 */
}GPIO_TypeDef;

#define PERIPH_BASE       (0x40000000UL) /*!< Peripheral base address in the alias region */
#define AHBPERIPH_BASE    (PERIPH_BASE + 0x00020000UL)
#define RCC_BASE          (AHBPERIPH_BASE + 0x00001000UL)
#define RCC_IOPENR		*((volatile unsigned int*)(RCC_BASE + 0x0000002CUL))
#define RCC_GPIOA_EN			((unsigned int)0x00000001U)

//#define GPIO_PA5PIN_BASE    (unsigned int)0x50000000UL
#define GPIOA_BASE		     ((unsigned int)0x50000000UL)
#define GPIOA			     (GPIO_TypeDef *) GPIOA_BASE         // start address
#define GPIO_PIN_5_POS				5

#define GPIO_2BIT_POS_MASK    ((unsigned int)0x00000003U)
#define GPIO_1BIT_POS_MASK    ((unsigned int)0x00000001U)

#define GPIO_MODE_OUTPUT      			((unsigned int)0x00000001U)   /*!< GPIO OutPut Mode                 */
#define GPIO_OUTPUT_TYPE_0      		((unsigned int)0x00000000U)		/* PushPull */
#define GPIO_NOPUPD				((unsigned int)0x00000000U)		/* No Pull up / down */
#define GPIO_SPEED_FREQ_VERY_HIGH   	((unsigned int)0x00000003U)  /*!< range  10 MHz to 35 MHz, please refer to the product datasheet */

#define LED2_ON					((unsigned int)0x00000001U)
#define LED2_OFF					((unsigned int)0x00000000U)	

void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

int main()
{
	unsigned int position = GPIO_PIN_5_POS; //Pin Position: PA5
	unsigned int temp = 0x0U;
	unsigned int mode = 0x0U;
	unsigned int reg = 0x0U;
	GPIO_TypeDef *GPIOA_reg = GPIOA;
	
	//RCC-GPIOA
	RCC_IOPENR |= RCC_GPIOA_EN;
	
	//PA5 Mode
	reg = GPIOA_reg->MODER;	//GPIO_PA5PIN_MODE
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = GPIO_MODE_OUTPUT << (position*2U) ;	// GPO Mode - 2bit 0x00000400
	reg |= mode;
	GPIOA_reg->MODER = reg;

	//OTYPER
	reg = GPIOA_reg->OTYPER;	//GPIO_PA5PIN_OTYPE
	temp = ~(GPIO_1BIT_POS_MASK << position);	//~0x20 ==> 0xFFFFFFDF
	reg &= temp;
	mode = GPIO_OUTPUT_TYPE_0 << position ;	//PP Output type - 1bit
	reg |= mode;
	GPIOA_reg->OTYPER = reg;

	//OSPEEDR
	reg = GPIOA_reg->OSPEEDR;	//GPIO_PA5PIN_OSPEEDR
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = GPIO_SPEED_FREQ_VERY_HIGH << (position*2U) ;	//Very High Mode - 2bit 0x00000C00
	reg |= mode;
	GPIOA_reg->OSPEEDR = reg;
	
	//OPUPDR
	reg = GPIOA_reg->PUPDR;
	temp = ~(0x03U << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = 0x0U << (position*2U) ;	//No Pull Up - Pull Down - 2bit 0x24000000
	reg |= mode;
	GPIOA_reg->PUPDR = reg;

	//ODR
	reg = GPIOA_reg->ODR;	//GPIO_PA5PIN_ODR
	temp = ~(GPIO_1BIT_POS_MASK << position);	//~0x20 ==> 0xFFFFFFDF
	reg &= temp;

	mode = LED2_ON << position ;	
	GPIOA_reg->ODR = reg | mode;

	while(1)
	{
		delay(0x20000);
		mode = ~mode&(GPIO_1BIT_POS_MASK << position);
		GPIOA_reg->ODR = reg | mode;
	}
}
```

## D13(초록LED), D7
<img src="https://github.com/user-attachments/assets/47f6822e-ec1a-4d66-b64e-db3bb2281d01" width="300" height="520">   
<img src="https://github.com/user-attachments/assets/eef64d05-d1d8-461d-af59-e4ec6c439434" width="300" height="520">   
```c
typedef struct
{
  volatile unsigned int MODER;        /*!< GPIO pGPIO_Type mode register,                     Address offset: 0x00 */
  volatile unsigned int OTYPER;       /*!< GPIO pGPIO_Type output type register,              Address offset: 0x04 */
  volatile unsigned int OSPEEDR;      /*!< GPIO pGPIO_Type output speed register,             Address offset: 0x08 */
  volatile unsigned int PUPDR;        /*!< GPIO pGPIO_Type pull-up/pull-down register,        Address offset: 0x0C */
  volatile unsigned int IDR;          /*!< GPIO pGPIO_Type input data register,               Address offset: 0x10 */
  volatile unsigned int ODR;          /*!< GPIO pGPIO_Type output data register,              Address offset: 0x14 */
  volatile unsigned int BSRR;         /*!< GPIO pGPIO_Type bit set/reset registerBSRR,        Address offset: 0x18 */
  volatile unsigned int LCKR;         /*!< GPIO pGPIO_Type configuration lock register,       Address offset: 0x1C */
  volatile unsigned int AFR[2];       /*!< GPIO alternate function register,            Address offset: 0x20-0x24 */
  volatile unsigned int BRR;          /*!< GPIO bit reset register,                     Address offset: 0x28 */
}GPIO_TypeDef;

typedef enum
{
  GPIO_PIN_RESET = 0U,
  GPIO_PIN_SET
} GPIO_PinState;

#define PERIPH_BASE       	(0x40000000UL) /*!< Peripheral base address in the alias region */
#define AHBPERIPH_BASE    (PERIPH_BASE + 0x00020000UL)
#define RCC_BASE          	(AHBPERIPH_BASE + 0x00001000UL)
#define RCC_IOPENR		*((volatile unsigned int*)(RCC_BASE + 0x0000002CUL))
#define RCC_GPIOA_EN	((unsigned int)0x00000001U)

//#define GPIO_PA5PIN_BASE    (unsigned int)0x50000000UL
#define GPIOA_BASE		     ((unsigned int)0x50000000UL)
#define GPIOA			     (GPIO_TypeDef *) GPIOA_BASE

#define GPIO_2BIT_POS_MASK    ((unsigned int)0x00000003U)
#define GPIO_1BIT_POS_MASK    ((unsigned int)0x00000001U)

#define GPIO_MODE_OUTPUT      			((unsigned int)0x00000001U)   /*!< GPIO OutPut Mode                 */
#define GPIO_OUTPUT_TYPE_0      		((unsigned int)0x00000000U)		/* PushPull */
#define GPIO_NOPUPD				((unsigned int)0x00000000U)		/* No Pull up / down */
#define GPIO_SPEED_FREQ_VERY_HIGH   	((unsigned int)0x00000003U)  /*!< range  10 MHz to 35 MHz, please refer to the product datasheet */

#define LED2_ON					((unsigned int)0x00000001U)
#define LED2_OFF					((unsigned int)0x00000000U)	

#define GPIO_PIN_5_POS				5
#define GPIO_PIN_8_POS				8

void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void GPIO_Init(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_TypeDef *initVal)
{
	unsigned int position = pin; //Pin Position: PA5
	unsigned int temp = 0x0U;
	unsigned int mode = 0x0U;
	unsigned int reg = 0x0U;
	
		//PA5 Mode
	reg = pGPIO_Type->MODER;	//GPIO_PA5PIN_MODE
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = initVal->MODER << (position*2U) ;	// GPO Mode - 2bit 0x00000400
	reg |= mode;
	pGPIO_Type->MODER = reg;

	//OTYPER
	reg = pGPIO_Type->OTYPER;	//GPIO_PA5PIN_OTYPE
	temp = ~(GPIO_1BIT_POS_MASK << position);	//~0x20 ==> 0xFFFFFFDF
	reg &= temp;
	mode = initVal->OTYPER << position ;	//PP Output type - 1bit
	reg |= mode;
	pGPIO_Type->OTYPER = reg;

	//OSPEEDR
	reg = pGPIO_Type->OSPEEDR;	//GPIO_PA5PIN_OSPEEDR
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = initVal->OSPEEDR << (position*2U) ;	//Very High Mode - 2bit 0x00000C00
	reg |= mode;
	pGPIO_Type->OSPEEDR = reg;
	
	//OPUPDR
	reg = pGPIO_Type->PUPDR;
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = initVal->PUPDR << (position*2U) ;	//No Pull Up - Pull Down - 2bit 0x24000000
	reg |= mode;
	pGPIO_Type->PUPDR = reg;
}

void GPIO_Write_Pin(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_PinState state)
{
	unsigned int temp = 0x0U;
	unsigned int mode = 0x0U;
	unsigned int reg = 0x0U;
	
		//ODR
	reg = pGPIO_Type->ODR;	//GPIO_PA5PIN_ODR
	temp = ~(GPIO_1BIT_POS_MASK << pin);	//~0x20 ==> 0xFFFFFFDF
	reg &= temp;
	mode = state << pin ;	
	pGPIO_Type->ODR = reg | mode;
}
	
int main()
{
	GPIO_TypeDef initVal;
	GPIO_PinState pin_state = GPIO_PIN_RESET;
	
	//RCC-GPIOA
	RCC_IOPENR |= RCC_GPIOA_EN;
	
	initVal.MODER = GPIO_MODE_OUTPUT;
	initVal.OTYPER = GPIO_OUTPUT_TYPE_0;
	initVal.OSPEEDR = GPIO_SPEED_FREQ_VERY_HIGH;
	initVal.PUPDR = GPIO_NOPUPD;
	
//	GPIO_Init(GPIOA, GPIO_PIN_5_POS, &initVal);
	
	//ODR
//	GPIO_Write_Pin(GPIOA, GPIO_PIN_5_POS, pin_state);

	// LED2
	GPIO_Init(GPIOA, GPIO_PIN_5_POS, &initVal);
	GPIO_Write_Pin(GPIOA, GPIO_PIN_5_POS, pin_state);
	
	//UserLED1
	GPIO_Init(GPIOA, GPIO_PIN_8_POS, &initVal);
	GPIO_Write_Pin(GPIOA, GPIO_PIN_8_POS, (GPIO_PinState)!pin_state);

	while(1)
	{
		delay(0x20000);
		pin_state = (GPIO_PinState)(!pin_state);
		GPIO_Write_Pin(GPIOA, GPIO_PIN_5_POS, pin_state);
		GPIO_Write_Pin(GPIOA, GPIO_PIN_8_POS, (GPIO_PinState)!pin_state);
	}
}
```

## 위에꺼에서 BRR, BSRR 사용하고 토클
✅ BSRR, BRR이란?
BSRR (Bit Set/Reset Register)는 GPIO 핀을 설정(SET) 또는 리셋(RESET) 하는 레지스터이다. 
BSRR의 낮은 16비트 (0~15비트) → 해당 비트를 1로 설정하면 핀이 HIGH(1) 상태가 됨   
BSRR의 높은 16비트 (16~31비트) → 해당 비트를 1로 설정하면 핀이 LOW(0) 상태가 됨   
BRR (Bit Reset Register)는 특정 핀을 0으로 만드는 역할을 한다.   
BSRR의 상위 16비트와 동일한 기능을 하지만 따로 분리된 레지스터로 존재함   
Active-Low 방식으로 LED를 연결했기 때문에 bsrr이 0이면 불이 켜진다.   
   
```c
typedef struct
{
  volatile unsigned int MODER;        /*!< GPIO port mode register,                     Address offset: 0x00 */
  volatile unsigned int OTYPER;       /*!< GPIO port output type register,              Address offset: 0x04 */
  volatile unsigned int OSPEEDR;      /*!< GPIO port output speed register,             Address offset: 0x08 */
  volatile unsigned int PUPDR;        /*!< GPIO port pull-up/pull-down register,        Address offset: 0x0C */
  volatile unsigned int IDR;          /*!< GPIO port input data register,               Address offset: 0x10 */
  volatile unsigned int ODR;          /*!< GPIO port output data register,              Address offset: 0x14 */
  volatile unsigned int BSRR;         /*!< GPIO port bit set/reset registerBSRR,        Address offset: 0x18 */
  volatile unsigned int LCKR;         /*!< GPIO port configuration lock register,       Address offset: 0x1C */
  volatile unsigned int AFR[2];       /*!< GPIO alternate function register,            Address offset: 0x20-0x24 */
  volatile unsigned int BRR;          /*!< GPIO bit reset register,                     Address offset: 0x28 */
}GPIO_TypeDef;

typedef enum
{
  GPIO_PIN_RESET = 0U,
  GPIO_PIN_SET
} GPIO_PinState;

typedef enum 
{
  LED2 = 0,
  USER_LED1,
} Led_TypeDef;

#define PERIPH_BASE       	(0x40000000UL) /*!< Peripheral base address in the alias region */
#define AHBPERIPH_BASE    (PERIPH_BASE + 0x00020000UL)
#define RCC_BASE          	(AHBPERIPH_BASE + 0x00001000UL)
#define RCC_IOPENR		*((volatile unsigned int*)(RCC_BASE + 0x0000002CUL))
#define RCC_GPIOA_EN	((unsigned int)0x00000001U)

//#define GPIO_PA5PIN_BASE    (unsigned int)0x50000000UL
#define GPIOA_BASE		     ((unsigned int)0x50000000UL)
#define GPIOA			     (GPIO_TypeDef *) GPIOA_BASE

#define GPIO_NUMBER           (16U)
#define GPIO_2BIT_POS_MASK    ((unsigned int)0x00000003U)
#define GPIO_1BIT_POS_MASK    ((unsigned int)0x00000001U)

#define GPIO_MODE_OUTPUT      			((unsigned int)0x00000001U)   /*!< GPIO OutPut Mode                 */
#define GPIO_OUTPUT_TYPE_0      		((unsigned int)0x00000000U)		/* PushPull */
#define GPIO_NOPUPD				((unsigned int)0x00000000U)		/* No Pull up / down */
#define GPIO_SPEED_FREQ_VERY_HIGH   	((unsigned int)0x00000003U)  /*!< range  10 MHz to 35 MHz, please refer to the product datasheet */

#define LED2_ON					((unsigned int)0x00000001U)
#define LED2_OFF					((unsigned int)0x00000000U)	
	
#define LEDn									2
#define GPIO_PIN_5_POS				5
#define GPIO_PIN_8_POS				8

GPIO_TypeDef* LED_PORT[LEDn] = {GPIOA, GPIOA};
unsigned short LED_PIN[LEDn] = {GPIO_PIN_5_POS, GPIO_PIN_8_POS};

void delay(unsigned int delay_cnt);
void GPIO_Init(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_TypeDef *initVal);
void GPIO_Write_Pin(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_PinState state);
void GPIO_Toggle_Pin(GPIO_TypeDef* pGPIO_Type, unsigned short GPIO_Pin);    // 토글은 현재 상태 읽을 필요 없어서 입력값 하나 줄어듦

int main()
{
	GPIO_TypeDef initVal;
	GPIO_PinState pin_state = GPIO_PIN_RESET;
	
	//RCC-GPIOA
	RCC_IOPENR |= RCC_GPIOA_EN;
	
	initVal.MODER = GPIO_MODE_OUTPUT;
	initVal.OTYPER = GPIO_OUTPUT_TYPE_0;
	initVal.OSPEEDR = GPIO_SPEED_FREQ_VERY_HIGH;
	initVal.PUPDR = GPIO_NOPUPD;
	
	// LED2
	GPIO_Init(LED_PORT[LED2], LED_PIN[LED2], &initVal);
	GPIO_Write_Pin(LED_PORT[LED2], LED_PIN[LED2], pin_state);
	
	//UserLED1
	GPIO_Init(LED_PORT[USER_LED1], LED_PIN[USER_LED1], &initVal);
	GPIO_Write_Pin(LED_PORT[USER_LED1], LED_PIN[USER_LED1], (GPIO_PinState)!pin_state);

	while(1)
	{
		delay(0x20000);
		GPIO_Toggle_Pin(LED_PORT[LED2], LED_PIN[LED2]);
		GPIO_Toggle_Pin(LED_PORT[USER_LED1], LED_PIN[USER_LED1]);
	}

}

void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void GPIO_Init(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_TypeDef *initVal)
{
	unsigned int position = pin; //Pin Position: PA5
	unsigned int temp = 0x0U;
	unsigned int mode = 0x0U;
	unsigned int reg = 0x0U;
	
	// GPIO Mode
	reg = pGPIO_Type->MODER;	//GPIO_PA5PIN_MODE
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = initVal->MODER << (position*2U) ;	// GPO Mode - 2bit 0x00000400
	reg |= mode;
	pGPIO_Type->MODER = reg;

	//OTYPER
	reg = pGPIO_Type->OTYPER;	//GPIO_PA5PIN_OTYPE
	temp = ~(GPIO_1BIT_POS_MASK << position);	//~0x20 ==> 0xFFFFFFDF
	reg &= temp;
	mode = initVal->OTYPER << position ;	//PP Output type - 1bit
	reg |= mode;
	pGPIO_Type->OTYPER = reg;

	//OSPEEDR
	reg = pGPIO_Type->OSPEEDR;	//GPIO_PA5PIN_OSPEEDR
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = initVal->OSPEEDR << (position*2U) ;	//Very High Mode - 2bit 0x00000C00
	reg |= mode;
	pGPIO_Type->OSPEEDR = reg;
	
	//OPUPDR
	reg = pGPIO_Type->PUPDR;
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = initVal->PUPDR << (position*2U) ;	//No Pull Up - Pull Down - 2bit 0x24000000
	reg |= mode;
	pGPIO_Type->PUPDR = reg;
}

#if 0
void GPIO_Write_Pin(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_PinState state)
{
	unsigned int temp = 0x0U;
	unsigned int mode = 0x0U;
	unsigned int reg = 0x0U;
	
	//ODR
	reg = pGPIO_Type->ODR;
	temp = ~(GPIO_1BIT_POS_MASK << pin);
	reg &= temp;
	mode = state << pin ;	
	port->ODR = reg | mode;
}
#else
void GPIO_Write_Pin(GPIO_TypeDef *pGPIO_Type, unsigned short GPIO_Pin, GPIO_PinState PinState)
{
	unsigned int position = 0;
	position = GPIO_1BIT_POS_MASK << (GPIO_Pin);
  if (PinState != GPIO_PIN_RESET)
  {
    pGPIO_Type->BSRR = position;
  }
  else // PinState == GPIO_PIN_RESET
  {
    pGPIO_Type->BRR = position ;
  }
}
#endif
void GPIO_Toggle_Pin(GPIO_TypeDef* pGPIO_Type, unsigned short GPIO_Pin)
{
	unsigned int position = 0;
	position = GPIO_1BIT_POS_MASK << (GPIO_Pin);
	unsigned int odr; // we should read odr value
  /* get current Output Data Register value */
  odr = pGPIO_Type->ODR;

  /* Set selected pins that were at low level, and reset ones that were high */
  pGPIO_Type->BSRR = ((odr & position) << GPIO_NUMBER) | (~odr & position);
}
```

## 혼자 가지고 놀기
<img src="https://github.com/user-attachments/assets/0fa5d924-9a7c-4099-a1d5-62a4b1a366fb" width="350" height="300">   
<img src="https://github.com/user-attachments/assets/94c00ec2-4210-48ac-99a4-e2fe828c8d73" width="350" height="300">   
stm32l0xx_nucleo.h 에 적혀있음   
```c
typedef struct
{
  volatile unsigned int MODER;        /*!< GPIO port mode register,                     Address offset: 0x00 */
  volatile unsigned int OTYPER;       /*!< GPIO port output type register,              Address offset: 0x04 */
  volatile unsigned int OSPEEDR;      /*!< GPIO port output speed register,             Address offset: 0x08 */
  volatile unsigned int PUPDR;        /*!< GPIO port pull-up/pull-down register,        Address offset: 0x0C */
  volatile unsigned int IDR;          /*!< GPIO port input data register,               Address offset: 0x10 */
  volatile unsigned int ODR;          /*!< GPIO port output data register,              Address offset: 0x14 */
  volatile unsigned int BSRR;         /*!< GPIO port bit set/reset registerBSRR,        Address offset: 0x18 */
  volatile unsigned int LCKR;         /*!< GPIO port configuration lock register,       Address offset: 0x1C */
  volatile unsigned int AFR[2];       /*!< GPIO alternate function register,            Address offset: 0x20-0x24 */
  volatile unsigned int BRR;          /*!< GPIO bit reset register,                     Address offset: 0x28 */
}GPIO_TypeDef;

typedef enum
{
  GPIO_PIN_RESET = 0U,
  GPIO_PIN_SET
} GPIO_PinState;

typedef enum 
{
  LED2 = 0,
	LED3 = 1,
	LED4 = 2,
  USER_LED1,
} Led_TypeDef;

#define PERIPH_BASE       	(0x40000000UL) /*!< Peripheral base address in the alias region */
#define AHBPERIPH_BASE    (PERIPH_BASE + 0x00020000UL)
#define RCC_BASE          	(AHBPERIPH_BASE + 0x00001000UL)
#define RCC_IOPENR		*((volatile unsigned int*)(RCC_BASE + 0x0000002CUL))
#define RCC_GPIOA_EN	((unsigned int)0x00000001U)

//#define GPIO_PA5PIN_BASE    (unsigned int)0x50000000UL
#define GPIOA_BASE		     ((unsigned int)0x50000000UL)
#define GPIOA			     (GPIO_TypeDef *) GPIOA_BASE

#define GPIO_NUMBER           (16U)
#define GPIO_2BIT_POS_MASK    ((unsigned int)0x00000003U)
#define GPIO_1BIT_POS_MASK    ((unsigned int)0x00000001U)

#define GPIO_MODE_OUTPUT      			((unsigned int)0x00000001U)   /*!< GPIO OutPut Mode                 */
#define GPIO_OUTPUT_TYPE_0      		((unsigned int)0x00000000U)		/* PushPull */
#define GPIO_NOPUPD				((unsigned int)0x00000000U)		/* No Pull up / down */
#define GPIO_SPEED_FREQ_VERY_HIGH   	((unsigned int)0x00000003U)  /*!< range  10 MHz to 35 MHz, please refer to the product datasheet */

#define LED2_ON					((unsigned int)0x00000001U)
#define LED2_OFF					((unsigned int)0x00000000U)	
	
#define LEDn									4
#define GPIO_PIN_5_POS				5
#define GPIO_PIN_6_POS				6
#define GPIO_PIN_7_POS				7
#define GPIO_PIN_8_POS				8

GPIO_TypeDef* LED_PORT[LEDn] = {GPIOA, GPIOA, GPIOA, GPIOA};
unsigned short LED_PIN[LEDn] = {GPIO_PIN_5_POS, GPIO_PIN_6_POS, GPIO_PIN_7_POS, GPIO_PIN_8_POS};

void delay(unsigned int delay_cnt);
void GPIO_Init(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_TypeDef *initVal);
void GPIO_Write_Pin(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_PinState state);
void GPIO_Toggle_Pin(GPIO_TypeDef* pGPIO_Type, unsigned short GPIO_Pin);

int main()
{
	GPIO_TypeDef initVal;
	GPIO_PinState pin_state = GPIO_PIN_RESET;
	
	//RCC-GPIOA
	RCC_IOPENR |= RCC_GPIOA_EN;
	
	initVal.MODER = GPIO_MODE_OUTPUT;
	initVal.OTYPER = GPIO_OUTPUT_TYPE_0;
	initVal.OSPEEDR = GPIO_SPEED_FREQ_VERY_HIGH;
	initVal.PUPDR = GPIO_NOPUPD;
	
	// LED2
	GPIO_Init(LED_PORT[LED2], LED_PIN[LED2], &initVal);
	GPIO_Write_Pin(LED_PORT[LED2], LED_PIN[LED2], pin_state);
	
	//UserLED1
	GPIO_Init(LED_PORT[USER_LED1], LED_PIN[USER_LED1], &initVal);
	GPIO_Write_Pin(LED_PORT[USER_LED1], LED_PIN[USER_LED1], (GPIO_PinState)!pin_state);
	
	// LED3
	GPIO_Init(LED_PORT[LED3], LED_PIN[LED3], &initVal);
	GPIO_Write_Pin(LED_PORT[LED3], LED_PIN[LED3], pin_state);
	
	// LED4
	GPIO_Init(LED_PORT[LED4], LED_PIN[LED4], &initVal);
	GPIO_Write_Pin(LED_PORT[LED4], LED_PIN[LED4], pin_state);
	while(1)
	{
		delay(0x20000);
		GPIO_Toggle_Pin(LED_PORT[LED2], LED_PIN[LED2]);
		GPIO_Toggle_Pin(LED_PORT[USER_LED1], LED_PIN[USER_LED1]);
		GPIO_Toggle_Pin(LED_PORT[LED3], LED_PIN[LED3]);
		GPIO_Toggle_Pin(LED_PORT[LED4], LED_PIN[LED4]);		
	}

}

void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void GPIO_Init(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_TypeDef *initVal)
{
	unsigned int position = pin; //Pin Position: PA5
	unsigned int temp = 0x0U;
	unsigned int mode = 0x0U;
	unsigned int reg = 0x0U;
	
	// GPIO Mode
	reg = pGPIO_Type->MODER;	//GPIO_PA5PIN_MODE
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = initVal->MODER << (position*2U) ;	// GPO Mode - 2bit 0x00000400
	reg |= mode;
	pGPIO_Type->MODER = reg;

	//OTYPER
	reg = pGPIO_Type->OTYPER;	//GPIO_PA5PIN_OTYPE
	temp = ~(GPIO_1BIT_POS_MASK << position);	//~0x20 ==> 0xFFFFFFDF
	reg &= temp;
	mode = initVal->OTYPER << position ;	//PP Output type - 1bit
	reg |= mode;
	pGPIO_Type->OTYPER = reg;

	//OSPEEDR
	reg = pGPIO_Type->OSPEEDR;	//GPIO_PA5PIN_OSPEEDR
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = initVal->OSPEEDR << (position*2U) ;	//Very High Mode - 2bit 0x00000C00
	reg |= mode;
	pGPIO_Type->OSPEEDR = reg;
	
	//OPUPDR
	reg = pGPIO_Type->PUPDR;
	temp = ~(GPIO_2BIT_POS_MASK << (position*2U)); //0xFFFFF3FF
	reg &= temp;
	mode = initVal->PUPDR << (position*2U) ;	//No Pull Up - Pull Down - 2bit 0x24000000
	reg |= mode;
	pGPIO_Type->PUPDR = reg;
}

#if 0
void GPIO_Write_Pin(GPIO_TypeDef *pGPIO_Type, unsigned short pin, GPIO_PinState state)
{
	unsigned int temp = 0x0U;
	unsigned int mode = 0x0U;
	unsigned int reg = 0x0U;
	
	//ODR
	reg = pGPIO_Type->ODR;
	temp = ~(GPIO_1BIT_POS_MASK << pin);
	reg &= temp;
	mode = state << pin ;	
	port->ODR = reg | mode;
}
#else
void GPIO_Write_Pin(GPIO_TypeDef *pGPIO_Type, unsigned short GPIO_Pin, GPIO_PinState PinState)
{
	unsigned int position = 0;
	position = GPIO_1BIT_POS_MASK << (GPIO_Pin);
  if (PinState != GPIO_PIN_RESET)
  {
    pGPIO_Type->BSRR = position;
  }
  else // PinState == GPIO_PIN_RESET
  {
    pGPIO_Type->BRR = position ;
  }
}
#endif
void GPIO_Toggle_Pin(GPIO_TypeDef* pGPIO_Type, unsigned short GPIO_Pin)
{
	unsigned int position = 0;
	position = GPIO_1BIT_POS_MASK << (GPIO_Pin);
	unsigned int odr; // we should read odr value
  /* get current Output Data Register value */
  odr = pGPIO_Type->ODR;

  /* Set selected pins that were at low level, and reset ones that were high */
  pGPIO_Type->BSRR = ((odr & position) << GPIO_NUMBER) | (~odr & position);
}
```

# HAL
## STM32 HAL이란?
✅ HAL   
Hardware Abstraction Layer: 레지스터를 직접 다루지 않고도 쉽게 하드웨어를 제어할 수 있도록 도와준다. 
직접 HAL을 사용하면서 시행착오(Try & Error)를 겪다 보면 HAL이 가진 제한점이나 한계를 자연스럽게 알게 된다.(우리가 건들 일 없다. 일반화가 잘 돼있기에 건들 일 없다.)   
<img src="https://github.com/user-attachments/assets/b9feaa1e-0361-400d-9d4f-bd9a1e079c98" width="600" height="450">   
hal.h에 hal init 함수가 있음.   
<img src="https://github.com/user-attachments/assets/19a0c8df-cf19-4c9c-80bb-9e956071a71f" width="600" height="450">   
맨 아래 남색 3개가 hal driver 다. LL은 가장 낮은 레벨로 레지스터를 직접 만진다.   
   
✅ CMSIS   
ARM Cortex Microcontroller Software Interface Standard: ARM Cortex-M 기반 마이크로컨트롤러(예: STM32, NXP, TI 등)에서 코드를 표준화하고, 효율적으로 하드웨어를 제어할 수 있도록 만든 소프트웨어 프레임워크(우리가 건들 일 없다.)   
   
✅ Module   
<img src="https://github.com/user-attachments/assets/ae2f4581-9682-4e7a-b960-4d5449c56178" width="300" height="350">   
   
## HAL interrupt handler and callback functions – GPIO Interrupt Sequence
<img src="https://github.com/user-attachments/assets/b497eda1-63c6-44ae-b5c5-25d75a843797" width="600" height="200">   

# 실습 (다른 프로젝트 생성)
## toggle
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */

/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */
static GPIO_InitTypeDef  GPIO_InitStruct;
/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void delay(unsigned int delay_cnt)
{
      volatile int counter = 0;

      while(counter < delay_cnt) //delay loop
      {
         counter++;
      }
}
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  /* USER CODE BEGIN 2 */
   HAL_Init();
   SystemClock_Config();

  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */
      delay(0x20000);
      HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5);
    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_MSI;
  RCC_OscInitStruct.MSIState = RCC_MSI_ON;
  RCC_OscInitStruct.MSICalibrationValue = 0;
  RCC_OscInitStruct.MSIClockRange = RCC_MSIRANGE_5;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_MSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
/* USER CODE BEGIN MX_GPIO_Init_1 */
   GPIO_InitTypeDef initGPIO;
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();

/* USER CODE BEGIN MX_GPIO_Init_2 */
   initGPIO.Pin = GPIO_PIN_5;
   initGPIO.Mode = GPIO_MODE_OUTPUT_PP;
   initGPIO.Speed = GPIO_SPEED_FREQ_HIGH;
   initGPIO.Pull = GPIO_NOPULL;
   HAL_GPIO_Init(GPIOA, &initGPIO);
   
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```

# 실습 (핀 설정 바꿈)
## 5번 8번 toggle
<img src="https://github.com/user-attachments/assets/7fec1c6d-ffff-443d-8598-eea403ab19ef" width="600" height="500">   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
   
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */
   
  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  /* USER CODE BEGIN 2 */
   
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
   HAL_GPIO_WritePin(USER_LED_01_GPIO_Port, USER_LED_01_Pin, GPIO_PIN_SET);
   HAL_GPIO_WritePin(LED2_GPIO_Port, LED2_Pin, GPIO_PIN_SET);  
   
  while (1)
  {
    /* USER CODE END WHILE */
		delay(0x20000);
		HAL_GPIO_TogglePin(LED2_GPIO_Port, LED2_Pin);
		HAL_GPIO_TogglePin(USER_LED_01_GPIO_Port, USER_LED_01_Pin);		
    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_MSI;
  RCC_OscInitStruct.MSIState = RCC_MSI_ON;
  RCC_OscInitStruct.MSICalibrationValue = 0;
  RCC_OscInitStruct.MSIClockRange = RCC_MSIRANGE_5;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_MSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(GPIOA, LED2_Pin|USER_LED_01_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pins : LED2_Pin USER_LED_01_Pin */
  GPIO_InitStruct.Pin = LED2_Pin|USER_LED_01_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

/* USER CODE BEGIN MX_GPIO_Init_2 */
   
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```

## L0 Nucleo BSP에 User LED 추가 실습
BSP로 구현할 땐 main.c 뿐만 아니라 stm3210xx_nucleo.h, stm3210xx_nucleo.c 도 수정해줘야한다.
LED3를 USER_LED로 이름을 바꾸면 좋을 듯 하다.   
✅ main.c   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
#include "stm32l0xx_nucleo.h"
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
   
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */
   
  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  /* USER CODE BEGIN 2 */
   
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
   
  while (1)
  {
    /* USER CODE END WHILE */
		delay(0x20000);
		BSP_LED_Toggle(LED2);
		BSP_LED_Toggle(LED3);
    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_MSI;
  RCC_OscInitStruct.MSIState = RCC_MSI_ON;
  RCC_OscInitStruct.MSICalibrationValue = 0;
  RCC_OscInitStruct.MSIClockRange = RCC_MSIRANGE_5;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_MSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
	BSP_LED_Init(LED3);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

```   
   
✅ stm3210xx_nucleo.h, stm3210xx_nucleo.c   
얘네는 알아서 하자. 쉽다.   

# polling vs interrupt
## polling vs interrupt
✅ polling은 loop에서 계속 도는거고 interrupt는 trigger해준다.   
# 실습
## 누르면 LED2, LED3 불 키기(BSP, polling)
<img src="https://github.com/user-attachments/assets/1556f8e2-d99b-4903-9be9-f9564b848ed6" width="600" height="550">   
파란색 버튼은 PA13번이다. PA13번을 외부 LED에 연결하면 버튼이 안 눌릴 때 불이 들어오고 누르면 꺼진다. 즉, Pull-Up 구조인 것이다.   
<img src="https://github.com/user-attachments/assets/5c016ddd-3489-4afd-8156-209dc0420ebd" width="300" height="400">   
<img src="https://github.com/user-attachments/assets/573787d8-33ac-4910-a439-1f88d070d5fb" width="300" height="400">   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
#include "stm32l0xx_nucleo.h"
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
   
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */
   
  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  /* USER CODE BEGIN 2 */

	BSP_PB_Init(BUTTON_USER, BUTTON_MODE_GPIO);
	
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
   
  while (1)
  {
    /* USER CODE END WHILE */
		
		if (BSP_PB_GetState(BUTTON_USER) == 0) {
			BSP_LED_On(LED2);
			BSP_LED_On(LED3);
		}
		else {
			BSP_LED_Off(LED2);
			BSP_LED_Off(LED3);			
		}
    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_MSI;
  RCC_OscInitStruct.MSIState = RCC_MSI_ON;
  RCC_OscInitStruct.MSICalibrationValue = 0;
  RCC_OscInitStruct.MSIClockRange = RCC_MSIRANGE_5;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_MSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
	BSP_LED_Init(LED3);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```

## 누르면 LED2, LED3 불 키기(Interrupt)
✅ 작동 순서   
<img src="https://github.com/user-attachments/assets/9f9c78ba-a839-486e-9533-61dde1494cb0" width="600" height="380">   
   
✅ stm32l0xx_it.c 일부분   
```c
/* USER CODE BEGIN 1 */
void EXTI4_15_IRQHandler(void) 
{
	// USER_BUTTON_PIN
	HAL_GPIO_EXTI_IRQHandler(GPIO_PIN_13);
}
/* USER CODE END 1 */
```   
   
✅ main.c   
main.c 에선 BSP_PB_Init(BUTTON_USER, BUTTON_MODE_EXTI);로 모드만 바꿔주고 while문은 아무것도 안 적으면 된다.   
stm32l0xx_hal_gpio.c 에서 웬만하면 만지지 말라고 주석으로 경고가 써져있다. 따라서 아래코드는 main에서 작성하면 된다. 다만 여기선 직접 toggle 하지만 flag 값만 바꾸고 interrupt는 빠지는 것이 좋다.
```c
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
  /* Prevent unused argument(s) compilation warning */
	switch(GPIO_Pin)
	{
		case GPIO_PIN_13:
			HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5);
			HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_8);
			break;
		default:
			break;
	}
}
```

## 진동센서
✅ 세팅   
<img src="https://github.com/user-attachments/assets/95259bc6-9bc8-419b-9a3b-a15b06817a25" width="600" height="380">   
추가적으로 led 이름도 바꿨다.(뭐였는지 까먹음)   
   
✅ main.c   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
#include "stm32l0xx_nucleo.h"
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */
volatile int cnt = 0;
volatile int flag = 0;
/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
  switch(GPIO_Pin) {
		case USER_VIB_Pin:
			cnt++;
			break;
		case USER_BLUE_BUTTON_Pin:
			flag = 0;
			cnt = 0;
			break;
		default: 
			break;
	}
}
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
   
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */
   
  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  /* USER CODE BEGIN 2 */
	BSP_PB_Init(BUTTON_USER, BUTTON_MODE_EXTI);
	
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
   
  while (1)
  {
    /* USER CODE END WHILE */
		if (cnt>= 3) flag = 1;
		
		if(flag) 
		{
			HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5);
			HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_8);
		  delay(0x20000);
		}
		else 
		{
			BSP_LED_Off(LED2);
			BSP_LED_Off(LED3);			
		}
    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_MSI;
  RCC_OscInitStruct.MSIState = RCC_MSI_ON;
  RCC_OscInitStruct.MSICalibrationValue = 0;
  RCC_OscInitStruct.MSIClockRange = RCC_MSIRANGE_5;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_MSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin : USER_BLUE_BUTTON_Pin */
  GPIO_InitStruct.Pin = USER_BLUE_BUTTON_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_IT_RISING;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  HAL_GPIO_Init(USER_BLUE_BUTTON_GPIO_Port, &GPIO_InitStruct);

  /*Configure GPIO pin : USER_VIB_Pin */
  GPIO_InitStruct.Pin = USER_VIB_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_IT_RISING;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  HAL_GPIO_Init(USER_VIB_GPIO_Port, &GPIO_InitStruct);

  /* EXTI interrupt init*/
  HAL_NVIC_SetPriority(EXTI4_15_IRQn, 0, 0);
  HAL_NVIC_EnableIRQ(EXTI4_15_IRQn);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
	BSP_LED_Init(LED3);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {

  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```

# Clock  이해 및 Timer 활용
## Clock
✅ Clocks   
HSI16 (High Speed Internal) oscillator clock   
HSE16 (High Speed External) oscillator clock (micro second 까지 측정하려면 외부에서 좋은 oscillator clock 을 사오면 된다.)   

## Timer 주요 정보
✅ AHB   
AHB: 중앙에서 총괄하는 버스. AHB 가 Clock 정보를 전달한다.   

✅ Timer-Event Period 계산   
<img src="https://github.com/user-attachments/assets/acbd67f5-90cf-4279-944e-cbc974e99034" width="600" height="440">   
   
✅ RM0367 Timer   
ITR0, ITR1, ITR2, ITR3가 있는데 여기서 signal이 오면 그때부터 시간을 잰다.   
   
✅ Timer 주요 레지스터 정보   
PSC: Prescaler   
CNT: Counter   
ARR: Auto Reload Register   
CCR: Capture/Compare Register   
   
PSC, CNT, ARR이 중요하다. PSC 와 ARR 값은 같이 바꾼다보면된다.   
Reload Value를 만나면 Counter overflow를 띄우고, event를 띄운다. 또한 Interrupt Flag를 띄운다.   
<img src="https://github.com/user-attachments/assets/3305a94a-db42-4057-ba70-d60a300b84fd" width="600" height="440">   

## PWM
✅ PWM이란?   
Pulse Width Modulation: 진동 폭 변조   

# 실습
## 1초마다 interrupt 걸어서 led 깜빡
✅ RCC Clock Choice   
<img src="https://github.com/user-attachments/assets/e6ca4859-9645-4f29-b312-c309f5fd607c" width="600" height="440">   
   
✅ Clock Configuration   
<img src="https://github.com/user-attachments/assets/4393c647-19fa-4910-ae79-e54f1bb1d700" width="600" height="360">   
TMI3, TMI7는 기본 타이머다 이번 실습에선 TMI7을 사용했다.   
   
✅ main.c 일부분   
특히 이 부분에서 if 로 htim7 을 둔 이유는 이 함수는 다른 timer들도 다 사용하는 기본 함수라서 case 를 분류하기 위해서 그런 것이다.   
```c
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == htim7.Instance)
	{
		it_1sec = 1;
	}
}
```
   
✅ main.c   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
#include "stm32l0xx_nucleo.h"
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim3;
TIM_HandleTypeDef htim7;

/* USER CODE BEGIN PV */
volatile int cnt = 0;
volatile int flag = 0;
volatile int it_1sec = 0;
/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM7_Init(void);
static void MX_TIM3_Init(void);
/* USER CODE BEGIN PFP */
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == htim7.Instance)
	{
		it_1sec = 1;
	}
}
/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
  switch(GPIO_Pin) {
		case USER_VIB_Pin:
			cnt++;
			break;
		case USER_BLUE_BUTTON_Pin:
			flag = 0;
			cnt = 0;
			break;
		default: 
			break;
	}
}

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
   
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */
   
  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM7_Init();
  MX_TIM3_Init();
  /* USER CODE BEGIN 2 */
	BSP_PB_Init(BUTTON_USER, BUTTON_MODE_EXTI);
	
	HAL_TIM_Base_Start_IT(&htim7);
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
   
  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_BYPASS;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_OC_InitTypeDef sConfigOC = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 8-1;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 1000-1;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_PWM_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigOC.OCMode = TIM_OCMODE_PWM1;
  sConfigOC.Pulse = 0;
  sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
  sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
  if (HAL_TIM_PWM_ConfigChannel(&htim3, &sConfigOC, TIM_CHANNEL_3) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */

  /* USER CODE END TIM3_Init 2 */
  HAL_TIM_MspPostInit(&htim3);

}

/**
  * @brief TIM7 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM7_Init(void)
{

  /* USER CODE BEGIN TIM7_Init 0 */

  /* USER CODE END TIM7_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM7_Init 1 */

  /* USER CODE END TIM7_Init 1 */
  htim7.Instance = TIM7;
  htim7.Init.Prescaler = 800-1;
  htim7.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim7.Init.Period = 10000-1;
  htim7.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim7) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim7, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM7_Init 2 */

  /* USER CODE END TIM7_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();

  /*Configure GPIO pin : USER_BLUE_BUTTON_Pin */
  GPIO_InitStruct.Pin = USER_BLUE_BUTTON_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_IT_RISING;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  HAL_GPIO_Init(USER_BLUE_BUTTON_GPIO_Port, &GPIO_InitStruct);

  /*Configure GPIO pin : USER_VIB_Pin */
  GPIO_InitStruct.Pin = USER_VIB_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_IT_RISING;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  HAL_GPIO_Init(USER_VIB_GPIO_Port, &GPIO_InitStruct);

  /* EXTI interrupt init*/
  HAL_NVIC_SetPriority(EXTI4_15_IRQn, 0, 0);
  HAL_NVIC_EnableIRQ(EXTI4_15_IRQn);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
	BSP_LED_Init(LED3);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {

  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```   
   
## TIM3를 써서 PWM mode 1 을 사용해서 LED 밝기 바꾸기
✅ 결과   
<img src="https://github.com/user-attachments/assets/013b20ef-2303-4361-9585-90b27333bed2" width="300" height="340">   
<img src="https://github.com/user-attachments/assets/981d37cc-2e1e-4150-a435-5fe76d114bf1" width="300" height="340">   
<img src="https://github.com/user-attachments/assets/3299db35-7d5a-4f7d-98ff-96b6e0e6f353" width="300" height="340">   
<img src="https://github.com/user-attachments/assets/3d2b2504-2829-4f0c-a79e-7b8bccc931c9" width="300" height="340">   
<img src="https://github.com/user-attachments/assets/92b61fb2-d50f-4f86-acf1-f7435786abe4" width="300" height="340">   
   
✅ 세팅   
<img src="https://github.com/user-attachments/assets/d8ecaccd-d563-4488-b418-9641b17aacac" width="600" height="1100">   
<img src="https://github.com/user-attachments/assets/7d38e03e-c4c5-41d7-89be-c41464899818" width="600" height="310">   
Period 를 1kHz 로 함으로써 1초에 한 번씩 전원공급을 해준다. TIM3의 채널3을 PWM Generation CH3으로 하면 오른쪽 그림에서 PB3가 불이 켜진다. PB3는 A3에서 출력된다. 따라서 A3에서 선을 빼와서 LED의 +에 연결하면 불이 단계별로 켜진다.   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
#include "stm32l0xx_nucleo.h"
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim3;
TIM_HandleTypeDef htim7;

/* USER CODE BEGIN PV */
volatile int cnt = 0;
volatile int flag = 0;
volatile int it_1sec = 0;
volatile int Pulse = 0;
/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM7_Init(void);
static void MX_TIM3_Init(void);
/* USER CODE BEGIN PFP */
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == htim7.Instance)
	{
		it_1sec = 1;
	}
}
/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
  switch(GPIO_Pin) {
		case USER_VIB_Pin:
			cnt++;
			break;
		case USER_BLUE_BUTTON_Pin:
			flag = 0;
			cnt = 0;
			break;
		default: 
			break;
	}
}

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
   
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */
   
  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM7_Init();
  MX_TIM3_Init();
  /* USER CODE BEGIN 2 */
	BSP_PB_Init(BUTTON_USER, BUTTON_MODE_EXTI);
	
	HAL_TIM_Base_Start_IT(&htim7);

	HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_3); // 3번은 키면 할게 없음.
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  
	volatile int sum = htim3.Init.Period/4;
	
  while (1)
  {
    /* USER CODE END WHILE */
		if(it_1sec == 1) {
			it_1sec = 0;
			Pulse += sum;
			if(Pulse > htim3.Init.Period) {
				Pulse = 0;
			}
		}
		__HAL_TIM_SetCompare(&htim3, TIM_CHANNEL_3, Pulse); // timer function. PWM모드를 설정하면 사용 가능한 함수.
    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_BYPASS;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_OC_InitTypeDef sConfigOC = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 8-1;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 1000-1;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_PWM_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigOC.OCMode = TIM_OCMODE_PWM1;
  sConfigOC.Pulse = 0;
  sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
  sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
  if (HAL_TIM_PWM_ConfigChannel(&htim3, &sConfigOC, TIM_CHANNEL_3) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */

  /* USER CODE END TIM3_Init 2 */
  HAL_TIM_MspPostInit(&htim3);

}

/**
  * @brief TIM7 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM7_Init(void)
{

  /* USER CODE BEGIN TIM7_Init 0 */

  /* USER CODE END TIM7_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM7_Init 1 */

  /* USER CODE END TIM7_Init 1 */
  htim7.Instance = TIM7;
  htim7.Init.Prescaler = 800-1;
  htim7.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim7.Init.Period = 10000-1;
  htim7.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim7) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim7, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM7_Init 2 */

  /* USER CODE END TIM7_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();

  /*Configure GPIO pin : USER_BLUE_BUTTON_Pin */
  GPIO_InitStruct.Pin = USER_BLUE_BUTTON_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_IT_RISING;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  HAL_GPIO_Init(USER_BLUE_BUTTON_GPIO_Port, &GPIO_InitStruct);

  /*Configure GPIO pin : USER_VIB_Pin */
  GPIO_InitStruct.Pin = USER_VIB_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_IT_RISING;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  HAL_GPIO_Init(USER_VIB_GPIO_Port, &GPIO_InitStruct);

  /* EXTI interrupt init*/
  HAL_NVIC_SetPriority(EXTI4_15_IRQn, 0, 0);
  HAL_NVIC_EnableIRQ(EXTI4_15_IRQn);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
	BSP_LED_Init(LED3);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {

  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```   

# 초음파 센서 HC-SR04
## 초음파 센서
✅ timer6 추가했더니 SYS warning 이 떴다.   
이유인즉슨 P13 과 충돌이 일어난다는데 아무런 선과 연결하고 있지않아서 뭔가싶었다. 근데 보니까 blue button이 P13인데 아직 blue button을 사용하고 있기 때문이었다.

# 실습
## 개요
tim6: 1 마이크로 세컨드.   
tim3: 기다리는 것을 세는 것은 결국 tick을 계산하는 것이다. 그냥 timer6처럼 1us로 했다. tick이 30번 걸리면 30us, 300번 걸리면 300us 이다. 처음에 Rising edge를 검출해야하므로 세팅에 rising edge로 하고, Capture하면 interrupt해줘야 하기에 
interrupt세팅도 걸어줬다. GPIO setting에 very high   
하다가 distance가 한번만 들어오는 것 같았다. 코드상에서 distance로 찾아가 보니 timer7의 interrupt가 제대로 안 들어오는 것 같았다. 그래서 ir_1sec 변수에 앞에 volatile을 붙여줬다.   

## main.c
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stm32l0xx_nucleo.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim3;
TIM_HandleTypeDef htim6;
TIM_HandleTypeDef htim7;

/* USER CODE BEGIN PV */
volatile uint32_t nVibCnt = 0;
volatile uint8_t bVib = 0;
volatile uint8_t ir_1sec = 0;

//for Ultra Sonic Sensor
volatile uint8_t bCalculatedDistance = 0;
volatile uint32_t IC_Val1 = 0;
volatile uint32_t IC_Val2 = 0;
volatile uint32_t Difference = 0;
volatile uint8_t Is_First_Captured = 0;  // is the first value captured ?
volatile float Distance  = 0;

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM7_Init(void);
static void MX_TIM3_Init(void);
static void MX_TIM6_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void delay_us (uint16_t us)
{
  __HAL_TIM_SET_COUNTER(&htim6, 0);
  while (__HAL_TIM_GET_COUNTER (&htim6) < us);
}

void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)     // it works when rising edge triggered
{
	if(htim->Instance != htim3.Instance)
		return;
	
	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_1)  // if the interrupt source is channel1
	{
		if (Is_First_Captured==0) // if the first value is not captured
		{
			IC_Val1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1); // read the first value
			Is_First_Captured = 1;  // set the first captured as true
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_FALLING); 	// Now change the polarity to falling edge
		}
		else if (Is_First_Captured==1)   // if the first is already captured
		{
			IC_Val2 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1);  // read second value
			__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

			if (IC_Val2 > IC_Val1)
			{
				Difference = IC_Val2-IC_Val1;
			}

			else if (IC_Val1 > IC_Val2)
			{
				Difference = (0xffff - IC_Val1) + IC_Val2;
			}

			Distance = Difference * 0.034/2;
			Is_First_Captured = 0; // set it back to false			
			bCalculatedDistance = 1;
			__HAL_TIM_SET_COUNTER(htim, 0); // reset the counter
			// set polarity to rising edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
			HAL_TIM_IC_Stop_IT(&htim3,TIM_CHANNEL_1);
		}
	}
}

void HCSR04_Read (void)
{
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_SET);  // pull the TRIG pin HIGH
	delay_us(10);  // wait for 10 us
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);  // pull the TRIG pin low

	HAL_TIM_IC_Start_IT(&htim3, TIM_CHANNEL_1);
}


void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == TIM7)
  {   
		ir_1sec = 1;
  }
}
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
//	uint16_t oc_pulse = 0;
//	uint16_t oc_level = 0;
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM7_Init();
  MX_TIM3_Init();
  MX_TIM6_Init();
  /* USER CODE BEGIN 2 */
	BSP_LED_Off(LED2);
	
	BSP_PB_Init(BUTTON_USER, BUTTON_MODE_EXTI);
	
	HAL_TIM_Base_Start_IT(&htim7);
	HAL_TIM_Base_Start(&htim6);
	
//	// LED Light Level
//	oc_level = (htim3.Init.Period / 4);
//	HAL_TIM_PWM_Start(&htim3,TIM_CHANNEL_3);
	
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
		if(ir_1sec)
		{
			ir_1sec = 0;
			if(bCalculatedDistance && Distance > 30)
			{
				bCalculatedDistance = 0;	
				BSP_LED_Toggle(LED2);
			}
			HCSR04_Read();
		}
		
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_BYPASS;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_IC_InitTypeDef sConfigIC = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 8-1;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 65535;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_IC_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigIC.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;
  sConfigIC.ICSelection = TIM_ICSELECTION_DIRECTTI;
  sConfigIC.ICPrescaler = TIM_ICPSC_DIV1;
  sConfigIC.ICFilter = 0;
  if (HAL_TIM_IC_ConfigChannel(&htim3, &sConfigIC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIMEx_RemapConfig(&htim3, TIM3_TI1_GPIO) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */

  /* USER CODE END TIM3_Init 2 */

}

/**
  * @brief TIM6 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM6_Init(void)
{

  /* USER CODE BEGIN TIM6_Init 0 */

  /* USER CODE END TIM6_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM6_Init 1 */

  /* USER CODE END TIM6_Init 1 */
  htim6.Instance = TIM6;
  htim6.Init.Prescaler = 8 - 1;
  htim6.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim6.Init.Period = 65535;
  htim6.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim6) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim6, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM6_Init 2 */

  /* USER CODE END TIM6_Init 2 */

}

/**
  * @brief TIM7 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM7_Init(void)
{

  /* USER CODE BEGIN TIM7_Init 0 */

  /* USER CODE END TIM7_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM7_Init 1 */

  /* USER CODE END TIM7_Init 1 */
  htim7.Instance = TIM7;
  htim7.Init.Prescaler = 8000-1;
  htim7.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim7.Init.Period = 1000-1;
  htim7.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim7) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim7, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM7_Init 2 */

  /* USER CODE END TIM7_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : TRIG_Pin */
  GPIO_InitStruct.Pin = TRIG_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH;
  HAL_GPIO_Init(TRIG_GPIO_Port, &GPIO_InitStruct);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```

# 임베디드 시스템의 인터페이스 이해
## MCU 인터페이스
✅ 아날로그 센서와 디지털 센서   
연속 신호와 이산 신호   
   
✅ 인터페이스 유형   
1. Analog 핀   
2. Digital 핀
3. UART 핀 (여기서부터 통신)   
4. I2C 핀   
5. SPI 핀   
   
✅ ADC   
0, 4, 5 channel 들이 빠르다.   
<<<<<<<<<<<<<<<<<<<<<<<
   
✅ 직렬, 병렬   
<<<<<<<<<<<<<<<<<<<<<<<   
다리가 짧을수록, 많을수록 성능이 높아진다. RAM 같은 경우도 CPU옆에 딱 붙여서 다리 짧게 만드는 것이 기술력이다.   
   
## 통신
✅ 통신 속도   
데이터율 (Data Rate): 데이터(비트, 심볼, 패킷 등)가 전송되는 속도(비트율, 심볼률 등)을 총칭.   
비트율 bps, 심볼율 sps (하나의 심볼은 n개의 비트를 하나로 모아 놓은 단위)   
Baud Rate: 단위 시간 당 변조된 신호 상태의 변화율 (= 변조 속도 = 심볼률)   
   
✅ U(S)ART   
Universal (Synchronous) Asynchronous Receiver/Transmitter   
<<<<<<<<<<<<<<<<<<<<<
한 라인당 시리얼통신을 한다. 무전기처럼 한 명만 말할 수 있다. 위의 사진엔 라인이 두 개다. 즉 2명에서 대화할 수 있는 것이다.   
Start, Data, Parity, Stop: 값이 가변적이다. 이들을 다 합친게 심볼이다.   

## STM32L073RZ-USART
✅ 총 4개를 지원해준다. X 는 지원해준다는 것이다.   
>>>>>>>>>>>>>>>>>>>>>>
USART1, USART2 가 4번 5번보다 기능을 많이 제공함.   
   
✅ I2C   
>>>>>>>>>>>>>>>>>>>>
master(MCU) 하나에 온도계, 습도계, 마이크를 달아서 I2C 통신을 할 수 있다. (주소값을 통해서 구분)   
   
우리 보드엔 세 개의 I2C 기능이 있다.   
>>>>>>>>>>>>>>>>>>>

✅ SPI   
>>>>>>>>>>>>>>>>>>
디지털핀 SS1, SS2를 set 하면 각각의 라인으로 slave 들을 통신할 수 있다. (빠른 통신 지원가능)   

## UART를 이용한 Serial 통신 이해 및 활용
✅ UART 는 serial 통신을 한다.   

# UART 실습
## 세팅
>>>>>>>>>>>>>>>>>> 강의록 ppt 파일들로 정리하기   
아래의 사진처럼 serial 이라고 먼저 눌러놓고 왼쪽의 serial 창을 설정하면 된다. Putty의 검은색 창이 뜨는데 이건 보드에서 뜨는거라서 여기다가는 타자를 못 친다. 또한 코드를 로드하면 reset버튼을 누르면 된다.   
<img src="https://github.com/user-attachments/assets/e09817bb-0398-4039-912e-d378c5fca6a6" width="300" height="300">   
## 
## main.c
```c
바뀐부분
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stm32l0xx_nucleo.h"
#include "stdio.h"
#include "string.h"

#define BUFSIZE 128

static uint8_t buff[BUFSIZE]="Hello Nucleo-L073RZ\r\n"; // array should not be used with volatile keyword

int main ()에서
	HAL_UART_Transmit(&huart2, buff, strlen((const char *)buff), 100);	
```

```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stm32l0xx_nucleo.h"
#include "stdio.h"
#include "string.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
#define BUFSIZE 128
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim3;
TIM_HandleTypeDef htim6;
TIM_HandleTypeDef htim7;

UART_HandleTypeDef huart2;

/* USER CODE BEGIN PV */
volatile uint32_t nVibCnt = 0;
volatile uint8_t bVib = 0;
volatile uint8_t ir_1sec = 0;

//for Ultra Sonic Sensor
volatile uint8_t bCalculatedDistance = 0;
volatile uint32_t IC_Val1 = 0;
volatile uint32_t IC_Val2 = 0;
volatile uint32_t Difference = 0;
volatile uint8_t Is_First_Captured = 0;  // is the first value captured ?
volatile float Distance  = 0;

// for uart
static uint8_t buff[BUFSIZE]="Hello Nucleo-L073RZ\r\n"; // array should not be used with volatile keyword

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM7_Init(void);
static void MX_TIM3_Init(void);
static void MX_TIM6_Init(void);
static void MX_USART2_UART_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void delay_us (uint16_t us)
{
  __HAL_TIM_SET_COUNTER(&htim6, 0);
  while (__HAL_TIM_GET_COUNTER (&htim6) < us);
}

void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)     // it works when rising edge triggered
{
	if(htim->Instance != htim3.Instance)
		return;
	
	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_1)  // if the interrupt source is channel1
	{
		if (Is_First_Captured==0) // if the first value is not captured
		{
			IC_Val1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1); // read the first value
			Is_First_Captured = 1;  // set the first captured as true
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_FALLING); 	// Now change the polarity to falling edge
		}
		else if (Is_First_Captured==1)   // if the first is already captured
		{
			IC_Val2 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1);  // read second value
			__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

			if (IC_Val2 > IC_Val1)
			{
				Difference = IC_Val2-IC_Val1;
			}

			else if (IC_Val1 > IC_Val2)
			{
				Difference = (0xffff - IC_Val1) + IC_Val2;
			}

			Distance = Difference * 0.034/2;
			Is_First_Captured = 0; // set it back to false			
			bCalculatedDistance = 1;
			__HAL_TIM_SET_COUNTER(htim, 0); // reset the counter
			// set polarity to rising edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
			HAL_TIM_IC_Stop_IT(&htim3,TIM_CHANNEL_1);
		}
	}
}

void HCSR04_Read (void)
{
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_SET);  // pull the TRIG pin HIGH
	delay_us(10);  // wait for 10 us
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);  // pull the TRIG pin low

	HAL_TIM_IC_Start_IT(&htim3, TIM_CHANNEL_1);
}


void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == TIM7)
  {   
		ir_1sec = 1;
  }
}
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
//	uint16_t oc_pulse = 0;
//	uint16_t oc_level = 0;
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM7_Init();
  MX_TIM3_Init();
  MX_TIM6_Init();
  MX_USART2_UART_Init();
  /* USER CODE BEGIN 2 */
	BSP_LED_Off(LED2);
	HAL_UART_Transmit(&huart2, buff, strlen((const char *)buff), 100);	
	BSP_PB_Init(BUTTON_USER, BUTTON_MODE_EXTI);
	
	HAL_TIM_Base_Start_IT(&htim7);
	HAL_TIM_Base_Start(&htim6);
	
//	// LED Light Level
//	oc_level = (htim3.Init.Period / 4);
//	HAL_TIM_PWM_Start(&htim3,TIM_CHANNEL_3);
	
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
		if(ir_1sec)
		{
			ir_1sec = 0;
			if(bCalculatedDistance && Distance > 30)
			{
				bCalculatedDistance = 0;	
				BSP_LED_Toggle(LED2);
			}
			HCSR04_Read();
		}
		
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_BYPASS;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART2;
  PeriphClkInit.Usart2ClockSelection = RCC_USART2CLKSOURCE_PCLK1;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_IC_InitTypeDef sConfigIC = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 8-1;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 65535;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_IC_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigIC.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;
  sConfigIC.ICSelection = TIM_ICSELECTION_DIRECTTI;
  sConfigIC.ICPrescaler = TIM_ICPSC_DIV1;
  sConfigIC.ICFilter = 0;
  if (HAL_TIM_IC_ConfigChannel(&htim3, &sConfigIC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIMEx_RemapConfig(&htim3, TIM3_TI1_GPIO) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */

  /* USER CODE END TIM3_Init 2 */

}

/**
  * @brief TIM6 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM6_Init(void)
{

  /* USER CODE BEGIN TIM6_Init 0 */

  /* USER CODE END TIM6_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM6_Init 1 */

  /* USER CODE END TIM6_Init 1 */
  htim6.Instance = TIM6;
  htim6.Init.Prescaler = 8 - 1;
  htim6.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim6.Init.Period = 65535;
  htim6.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim6) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim6, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM6_Init 2 */

  /* USER CODE END TIM6_Init 2 */

}

/**
  * @brief TIM7 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM7_Init(void)
{

  /* USER CODE BEGIN TIM7_Init 0 */

  /* USER CODE END TIM7_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM7_Init 1 */

  /* USER CODE END TIM7_Init 1 */
  htim7.Instance = TIM7;
  htim7.Init.Prescaler = 8000-1;
  htim7.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim7.Init.Period = 1000-1;
  htim7.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim7) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim7, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM7_Init 2 */

  /* USER CODE END TIM7_Init 2 */

}

/**
  * @brief USART2 Initialization Function
  * @param None
  * @retval None
  */
static void MX_USART2_UART_Init(void)
{

  /* USER CODE BEGIN USART2_Init 0 */

  /* USER CODE END USART2_Init 0 */

  /* USER CODE BEGIN USART2_Init 1 */

  /* USER CODE END USART2_Init 1 */
  huart2.Instance = USART2;
  huart2.Init.BaudRate = 9600;
  huart2.Init.WordLength = UART_WORDLENGTH_8B;
  huart2.Init.StopBits = UART_STOPBITS_1;
  huart2.Init.Parity = UART_PARITY_NONE;
  huart2.Init.Mode = UART_MODE_TX_RX;
  huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart2.Init.OverSampling = UART_OVERSAMPLING_16;
  huart2.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart2.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart2) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN USART2_Init 2 */

  /* USER CODE END USART2_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : TRIG_Pin */
  GPIO_InitStruct.Pin = TRIG_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH;
  HAL_GPIO_Init(TRIG_GPIO_Port, &GPIO_InitStruct);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```

## interrupt
✅ 결과   
타자를 치면 PuTTY 창에서 뜬다.   
<img src="https://github.com/user-attachments/assets/66bb475d-e7d2-463e-b0e3-597740390b2e" width="600" height="350">   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stm32l0xx_nucleo.h"
#include "stdio.h"
#include "string.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
#define BUFSIZE 128
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim3;
TIM_HandleTypeDef htim6;
TIM_HandleTypeDef htim7;

UART_HandleTypeDef huart2;

/* USER CODE BEGIN PV */
volatile uint32_t nVibCnt = 0;
volatile uint8_t bVib = 0;
volatile uint8_t ir_1sec = 0;

//for Ultra Sonic Sensor
volatile uint8_t bCalculatedDistance = 0;
volatile uint32_t IC_Val1 = 0;
volatile uint32_t IC_Val2 = 0;
volatile uint32_t Difference = 0;
volatile uint8_t Is_First_Captured = 0;  // is the first value captured ?
volatile float Distance  = 0;
uint8_t uart_rcvbuf;
volatile uint8_t bUART_RX = 0;
// for uart
static uint8_t buff[BUFSIZE]="Hello Nucleo-L073RZ\r\n"; // array should not be used with volatile keyword

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM7_Init(void);
static void MX_TIM3_Init(void);
static void MX_TIM6_Init(void);
static void MX_USART2_UART_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void delay_us (uint16_t us)
{
  __HAL_TIM_SET_COUNTER(&htim6, 0);
  while (__HAL_TIM_GET_COUNTER (&htim6) < us);
}

void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)     // it works when rising edge triggered
{
	if(htim->Instance != htim3.Instance)
		return;
	
	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_1)  // if the interrupt source is channel1
	{
		if (Is_First_Captured==0) // if the first value is not captured
		{
			IC_Val1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1); // read the first value
			Is_First_Captured = 1;  // set the first captured as true
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_FALLING); 	// Now change the polarity to falling edge
		}
		else if (Is_First_Captured==1)   // if the first is already captured
		{
			IC_Val2 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1);  // read second value
			__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

			if (IC_Val2 > IC_Val1)
			{
				Difference = IC_Val2-IC_Val1;
			}

			else if (IC_Val1 > IC_Val2)
			{
				Difference = (0xffff - IC_Val1) + IC_Val2;
			}

			Distance = Difference * 0.034/2;
			Is_First_Captured = 0; // set it back to false			
			bCalculatedDistance = 1;
			__HAL_TIM_SET_COUNTER(htim, 0); // reset the counter
			// set polarity to rising edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
			HAL_TIM_IC_Stop_IT(&htim3,TIM_CHANNEL_1);
		}
	}
}

void HCSR04_Read (void)
{
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_SET);  // pull the TRIG pin HIGH
	delay_us(10);  // wait for 10 us
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);  // pull the TRIG pin low

	HAL_TIM_IC_Start_IT(&htim3, TIM_CHANNEL_1);
}


void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == TIM7)
  {   
		ir_1sec = 1;
  }
}

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) // when receive, board sets bUART_RX = 1
{
  bUART_RX =1;
}
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
//	uint16_t oc_pulse = 0;
//	uint16_t oc_level = 0;
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM7_Init();
  MX_TIM3_Init();
  MX_TIM6_Init();
  MX_USART2_UART_Init();
  /* USER CODE BEGIN 2 */

	HAL_UART_Transmit(&huart2, buff, strlen((const char *)buff), 100);	
	HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1);
	
//	// LED Light Level
//	oc_level = (htim3.Init.Period / 4);
//	HAL_TIM_PWM_Start(&htim3,TIM_CHANNEL_3);
	
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
			if(ir_1sec)
			{
				ir_1sec = 0;
				if(bCalculatedDistance && Distance > 30)
				{
					bCalculatedDistance = 0;	
					BSP_LED_Toggle(LED2);
				}
				HCSR04_Read();
			}
		
    /* USER CODE END WHILE */
		if (bUART_RX) 
		{
			bUART_RX = 0;
			HAL_UART_Transmit(&huart2, &uart_rcvbuf, 1, 300);
			HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1); // 1 is size. when board receive 1bit it interrupts
		}
    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_BYPASS;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART2;
  PeriphClkInit.Usart2ClockSelection = RCC_USART2CLKSOURCE_PCLK1;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_IC_InitTypeDef sConfigIC = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 8-1;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 65535;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_IC_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigIC.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;
  sConfigIC.ICSelection = TIM_ICSELECTION_DIRECTTI;
  sConfigIC.ICPrescaler = TIM_ICPSC_DIV1;
  sConfigIC.ICFilter = 0;
  if (HAL_TIM_IC_ConfigChannel(&htim3, &sConfigIC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIMEx_RemapConfig(&htim3, TIM3_TI1_GPIO) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */

  /* USER CODE END TIM3_Init 2 */

}

/**
  * @brief TIM6 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM6_Init(void)
{

  /* USER CODE BEGIN TIM6_Init 0 */

  /* USER CODE END TIM6_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM6_Init 1 */

  /* USER CODE END TIM6_Init 1 */
  htim6.Instance = TIM6;
  htim6.Init.Prescaler = 8 - 1;
  htim6.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim6.Init.Period = 65535;
  htim6.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim6) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim6, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM6_Init 2 */

  /* USER CODE END TIM6_Init 2 */

}

/**
  * @brief TIM7 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM7_Init(void)
{

  /* USER CODE BEGIN TIM7_Init 0 */

  /* USER CODE END TIM7_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM7_Init 1 */

  /* USER CODE END TIM7_Init 1 */
  htim7.Instance = TIM7;
  htim7.Init.Prescaler = 8000-1;
  htim7.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim7.Init.Period = 1000-1;
  htim7.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim7) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim7, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM7_Init 2 */

  /* USER CODE END TIM7_Init 2 */

}

/**
  * @brief USART2 Initialization Function
  * @param None
  * @retval None
  */
static void MX_USART2_UART_Init(void)
{

  /* USER CODE BEGIN USART2_Init 0 */

  /* USER CODE END USART2_Init 0 */

  /* USER CODE BEGIN USART2_Init 1 */

  /* USER CODE END USART2_Init 1 */
  huart2.Instance = USART2;
  huart2.Init.BaudRate = 9600;
  huart2.Init.WordLength = UART_WORDLENGTH_8B;
  huart2.Init.StopBits = UART_STOPBITS_1;
  huart2.Init.Parity = UART_PARITY_NONE;
  huart2.Init.Mode = UART_MODE_TX_RX;
  huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart2.Init.OverSampling = UART_OVERSAMPLING_16;
  huart2.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart2.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart2) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN USART2_Init 2 */

  /* USER CODE END USART2_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : TRIG_Pin */
  GPIO_InitStruct.Pin = TRIG_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH;
  HAL_GPIO_Init(TRIG_GPIO_Port, &GPIO_InitStruct);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```
## printf관련함수들이 구현된 hw_vcom.c
✅ hw_vcom.c, hw_vcom.h 추가   
<img src="https://github.com/user-attachments/assets/485fdd63-36c9-4cf2-802b-42b9885e049e" width="600" height="350">   
<img src="https://github.com/user-attachments/assets/5c14435e-a0bd-4afe-a755-ade0fa4d9fa2" width="600" height="350">   

<https://github.com/sjmun96/stm32_lib/tree/main> 에서 추가

   
✅ 결과   
<img src="https://github.com/user-attachments/assets/83a0eb10-75b8-4a0b-af8c-1f2c11d02663" width="600" height="350">   
   
✅ main.c   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stm32l0xx_nucleo.h"
#include "stdio.h"
#include "string.h"
#include "hw_vcom.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
#define BUFSIZE 128
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim3;
TIM_HandleTypeDef htim6;
TIM_HandleTypeDef htim7;

UART_HandleTypeDef huart2;

/* USER CODE BEGIN PV */
volatile uint32_t nVibCnt = 0;
volatile uint8_t bVib = 0;
volatile uint8_t ir_1sec = 0;

//for Ultra Sonic Sensor
volatile uint8_t bCalculatedDistance = 0;
volatile uint32_t IC_Val1 = 0;
volatile uint32_t IC_Val2 = 0;
volatile uint32_t Difference = 0;
volatile uint8_t Is_First_Captured = 0;  // is the first value captured ?
volatile float Distance  = 0;
uint8_t uart_rcvbuf;
volatile uint8_t bUART_RX = 0;
// for uart
static uint8_t buff[BUFSIZE]="Hello Nucleo-L073RZ\r\n"; // array should not be used with volatile keyword

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM7_Init(void);
static void MX_TIM3_Init(void);
static void MX_TIM6_Init(void);
static void MX_USART2_UART_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void delay_us (uint16_t us)
{
  __HAL_TIM_SET_COUNTER(&htim6, 0);
  while (__HAL_TIM_GET_COUNTER (&htim6) < us);
}

void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)     // it works when rising edge triggered
{
	if(htim->Instance != htim3.Instance)
		return;
	
	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_1)  // if the interrupt source is channel1
	{
		if (Is_First_Captured==0) // if the first value is not captured
		{
			IC_Val1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1); // read the first value
			Is_First_Captured = 1;  // set the first captured as true
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_FALLING); 	// Now change the polarity to falling edge
		}
		else if (Is_First_Captured==1)   // if the first is already captured
		{
			IC_Val2 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1);  // read second value
			__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

			if (IC_Val2 > IC_Val1)
			{
				Difference = IC_Val2-IC_Val1;
			}

			else if (IC_Val1 > IC_Val2)
			{
				Difference = (0xffff - IC_Val1) + IC_Val2;
			}

			Distance = Difference * 0.034/2;
			Is_First_Captured = 0; // set it back to false			
			bCalculatedDistance = 1;
			__HAL_TIM_SET_COUNTER(htim, 0); // reset the counter
			// set polarity to rising edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
			HAL_TIM_IC_Stop_IT(&htim3,TIM_CHANNEL_1);
		}
	}
}

void HCSR04_Read (void)
{
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_SET);  // pull the TRIG pin HIGH
	delay_us(10);  // wait for 10 us
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);  // pull the TRIG pin low

	HAL_TIM_IC_Start_IT(&htim3, TIM_CHANNEL_1);
}


void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == TIM7)
  {   
		ir_1sec = 1;
  }
}

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) // when receive, board sets bUART_RX = 1
{
 // bUART_RX =1;
	if(huart->Instance == huart2.Instance)
	{
		HW_VCOM_RxCpltCallback(huart);
	}
}
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
//	uint16_t oc_pulse = 0;
//	uint16_t oc_level = 0;
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM7_Init();
  MX_TIM3_Init();
  MX_TIM6_Init();
  MX_USART2_UART_Init();
  /* USER CODE BEGIN 2 */

//	HAL_UART_Transmit(&huart2, buff, strlen((const char *)buff), 100);	
//	HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1);
	
	Printf("Hello Nucleo-l017rz\r\n"); //
//	// LED Light Level
//	oc_level = (htim3.Init.Period / 4);
//	HAL_TIM_PWM_Start(&htim3,TIM_CHANNEL_3);
	
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */
		/*
		if (bUART_RX) 
		{
			bUART_RX = 0;
			HAL_UART_Transmit(&huart2, &uart_rcvbuf, 1, 300);
			HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1); // 1 is size. when board receive 1bit it interrupts
		}
		*/
		
    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_BYPASS;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART2;
  PeriphClkInit.Usart2ClockSelection = RCC_USART2CLKSOURCE_PCLK1;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */

static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_IC_InitTypeDef sConfigIC = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 8-1;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 65535;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_IC_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigIC.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;
  sConfigIC.ICSelection = TIM_ICSELECTION_DIRECTTI;
  sConfigIC.ICPrescaler = TIM_ICPSC_DIV1;
  sConfigIC.ICFilter = 0;
  if (HAL_TIM_IC_ConfigChannel(&htim3, &sConfigIC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIMEx_RemapConfig(&htim3, TIM3_TI1_GPIO) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */

  /* USER CODE END TIM3_Init 2 */

}

/**
  * @brief TIM6 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM6_Init(void)
{

  /* USER CODE BEGIN TIM6_Init 0 */

  /* USER CODE END TIM6_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM6_Init 1 */

  /* USER CODE END TIM6_Init 1 */
  htim6.Instance = TIM6;
  htim6.Init.Prescaler = 8 - 1;
  htim6.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim6.Init.Period = 65535;
  htim6.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim6) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim6, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM6_Init 2 */

  /* USER CODE END TIM6_Init 2 */

}

/**
  * @brief TIM7 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM7_Init(void)
{

  /* USER CODE BEGIN TIM7_Init 0 */

  /* USER CODE END TIM7_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM7_Init 1 */

  /* USER CODE END TIM7_Init 1 */
  htim7.Instance = TIM7;
  htim7.Init.Prescaler = 8000-1;
  htim7.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim7.Init.Period = 1000-1;
  htim7.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim7) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim7, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM7_Init 2 */

  /* USER CODE END TIM7_Init 2 */

}

/**
  * @brief USART2 Initialization Function
  * @param None
  * @retval None
  */
static void MX_USART2_UART_Init(void)
{

  /* USER CODE BEGIN USART2_Init 0 */

  /* USER CODE END USART2_Init 0 */

  /* USER CODE BEGIN USART2_Init 1 */
	HW_VCOM_Init(&huart2);
#if 0
  /* USER CODE END USART2_Init 1 */
  huart2.Instance = USART2;
  huart2.Init.BaudRate = 9600;
  huart2.Init.WordLength = UART_WORDLENGTH_8B;
  huart2.Init.StopBits = UART_STOPBITS_1;
  huart2.Init.Parity = UART_PARITY_NONE;
  huart2.Init.Mode = UART_MODE_TX_RX;
  huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart2.Init.OverSampling = UART_OVERSAMPLING_16;
  huart2.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart2.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart2) != HAL_OK)
  {
    Error_Handler();
  }
#endif
  /* USER CODE BEGIN USART2_Init 2 */

  /* USER CODE END USART2_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : TRIG_Pin */
  GPIO_InitStruct.Pin = TRIG_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH;
  HAL_GPIO_Init(TRIG_GPIO_Port, &GPIO_InitStruct);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```
## 초음파거리 자동출력
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stm32l0xx_nucleo.h"
#include "stdio.h"
#include "string.h"
#include "hw_vcom.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
#define BUFSIZE 128
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim3;
TIM_HandleTypeDef htim6;
TIM_HandleTypeDef htim7;

UART_HandleTypeDef huart2;

/* USER CODE BEGIN PV */
volatile uint32_t nVibCnt = 0;
volatile uint8_t bVib = 0;
volatile uint8_t ir_1sec = 0;

//for Ultra Sonic Sensor
volatile uint8_t bCalculatedDistance = 0;
volatile uint32_t IC_Val1 = 0;
volatile uint32_t IC_Val2 = 0;
volatile uint32_t Difference = 0;
volatile uint8_t Is_First_Captured = 0;  // is the first value captured ?
volatile float Distance  = 0;
uint8_t uart_rcvbuf;
volatile uint8_t bUART_RX = 0;
// for uart
static uint8_t buff[BUFSIZE]="Hello Nucleo-L073RZ\r\n"; // array should not be used with volatile keyword

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM7_Init(void);
static void MX_TIM3_Init(void);
static void MX_TIM6_Init(void);
static void MX_USART2_UART_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void delay_us (uint16_t us)
{
  __HAL_TIM_SET_COUNTER(&htim6, 0);
  while (__HAL_TIM_GET_COUNTER (&htim6) < us);
}

void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)     // it works when rising edge triggered
{
	if(htim->Instance != htim3.Instance)
		return;
	
	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_1)  // if the interrupt source is channel1
	{
		if (Is_First_Captured==0) // if the first value is not captured
		{
			IC_Val1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1); // read the first value
			Is_First_Captured = 1;  // set the first captured as true
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_FALLING); 	// Now change the polarity to falling edge
		}
		else if (Is_First_Captured==1)   // if the first is already captured
		{
			IC_Val2 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1);  // read second value
			__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

			if (IC_Val2 > IC_Val1)
			{
				Difference = IC_Val2-IC_Val1;
			}

			else if (IC_Val1 > IC_Val2)
			{
				Difference = (0xffff - IC_Val1) + IC_Val2;
			}

			Distance = Difference * 0.034/2;
			Is_First_Captured = 0; // set it back to false			
			bCalculatedDistance = 1;
			__HAL_TIM_SET_COUNTER(htim, 0); // reset the counter
			// set polarity to rising edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
			HAL_TIM_IC_Stop_IT(&htim3,TIM_CHANNEL_1);
		}
	}
}

void HCSR04_Read (void)
{
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_SET);  // pull the TRIG pin HIGH
	delay_us(10);  // wait for 10 us
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);  // pull the TRIG pin low

	HAL_TIM_IC_Start_IT(&htim3, TIM_CHANNEL_1);
}


void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == TIM7)
  {   
		ir_1sec = 1;
  }
}

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) // when receive, board sets bUART_RX = 1
{
 // bUART_RX =1;
	if(huart->Instance == huart2.Instance)
	{
		HW_VCOM_RxCpltCallback(huart);
	}
}
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
//	uint16_t oc_pulse = 0;
//	uint16_t oc_level = 0;
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM7_Init();
  MX_TIM3_Init();
  MX_TIM6_Init();
  MX_USART2_UART_Init();
  /* USER CODE BEGIN 2 */

//	HAL_UART_Transmit(&huart2, buff, strlen((const char *)buff), 100);	
//	HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1);
	HAL_TIM_Base_Start_IT(&htim7);
	HAL_TIM_Base_Start(&htim6);	


//	// LED Light Level
//	oc_level = (htim3.Init.Period / 4);
//	HAL_TIM_PWM_Start(&htim3,TIM_CHANNEL_3);
	
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */
		/*
		if (bUART_RX) 
		{
			bUART_RX = 0;
			HAL_UART_Transmit(&huart2, &uart_rcvbuf, 1, 300);
			HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1); // 1 is size. when board receive 1bit it interrupts
		}
		*/
		if(ir_1sec)
		{
			ir_1sec = 0;
			if(bCalculatedDistance && Distance > 30)
			{
				bCalculatedDistance = 0;	
				Printf("%.3f\r\n",Distance);
			}
			HCSR04_Read();
		}

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_BYPASS;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART2;
  PeriphClkInit.Usart2ClockSelection = RCC_USART2CLKSOURCE_PCLK1;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */

static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_IC_InitTypeDef sConfigIC = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 8-1;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 65535;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_IC_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigIC.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;
  sConfigIC.ICSelection = TIM_ICSELECTION_DIRECTTI;
  sConfigIC.ICPrescaler = TIM_ICPSC_DIV1;
  sConfigIC.ICFilter = 0;
  if (HAL_TIM_IC_ConfigChannel(&htim3, &sConfigIC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIMEx_RemapConfig(&htim3, TIM3_TI1_GPIO) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */

  /* USER CODE END TIM3_Init 2 */

}

/**
  * @brief TIM6 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM6_Init(void)
{

  /* USER CODE BEGIN TIM6_Init 0 */

  /* USER CODE END TIM6_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM6_Init 1 */

  /* USER CODE END TIM6_Init 1 */
  htim6.Instance = TIM6;
  htim6.Init.Prescaler = 8 - 1;
  htim6.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim6.Init.Period = 65535;
  htim6.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim6) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim6, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM6_Init 2 */

  /* USER CODE END TIM6_Init 2 */

}

/**
  * @brief TIM7 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM7_Init(void)
{

  /* USER CODE BEGIN TIM7_Init 0 */

  /* USER CODE END TIM7_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM7_Init 1 */

  /* USER CODE END TIM7_Init 1 */
  htim7.Instance = TIM7;
  htim7.Init.Prescaler = 8000-1;
  htim7.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim7.Init.Period = 1000-1;
  htim7.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim7) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim7, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM7_Init 2 */

  /* USER CODE END TIM7_Init 2 */

}

/**
  * @brief USART2 Initialization Function
  * @param None
  * @retval None
  */
static void MX_USART2_UART_Init(void)
{

  /* USER CODE BEGIN USART2_Init 0 */

  /* USER CODE END USART2_Init 0 */

  /* USER CODE BEGIN USART2_Init 1 */
	HW_VCOM_Init(&huart2);
#if 0
  /* USER CODE END USART2_Init 1 */
  huart2.Instance = USART2;
  huart2.Init.BaudRate = 9600;
  huart2.Init.WordLength = UART_WORDLENGTH_8B;
  huart2.Init.StopBits = UART_STOPBITS_1;
  huart2.Init.Parity = UART_PARITY_NONE;
  huart2.Init.Mode = UART_MODE_TX_RX;
  huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart2.Init.OverSampling = UART_OVERSAMPLING_16;
  huart2.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart2.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart2) != HAL_OK)
  {
    Error_Handler();
  }
#endif
  /* USER CODE BEGIN USART2_Init 2 */

  /* USER CODE END USART2_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : TRIG_Pin */
  GPIO_InitStruct.Pin = TRIG_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH;
  HAL_GPIO_Init(TRIG_GPIO_Port, &GPIO_InitStruct);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```
# AT Command 추가 실습
✅ 결과   
ATZ를 치면 reset 기능을 한다.   
<img src="https://github.com/user-attachments/assets/9b8ed075-90ef-43df-aa32-2e57a3ea956f" width="350" height="300">   
   
✅ main.c   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stm32l0xx_nucleo.h"
#include "stdio.h"
#include "string.h"
#include "hw_vcom.h"
#include "command.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
#define BUFSIZE 128
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim3;
TIM_HandleTypeDef htim6;
TIM_HandleTypeDef htim7;

UART_HandleTypeDef huart2;

/* USER CODE BEGIN PV */
volatile uint32_t nVibCnt = 0;
volatile uint8_t bVib = 0;
volatile uint8_t ir_1sec = 0;

//for Ultra Sonic Sensor
volatile uint8_t bCalculatedDistance = 0;
volatile uint32_t IC_Val1 = 0;
volatile uint32_t IC_Val2 = 0;
volatile uint32_t Difference = 0;
volatile uint8_t Is_First_Captured = 0;  // is the first value captured ?
volatile float Distance  = 0;
uint8_t uart_rcvbuf;
volatile uint8_t bUART_RX = 0;
// for uart
static uint8_t buff[BUFSIZE]="Hello Nucleo-L073RZ\r\n"; // array should not be used with volatile keyword

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM7_Init(void);
static void MX_TIM3_Init(void);
static void MX_TIM6_Init(void);
static void MX_USART2_UART_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void delay_us (uint16_t us)
{
  __HAL_TIM_SET_COUNTER(&htim6, 0);
  while (__HAL_TIM_GET_COUNTER (&htim6) < us);
}

void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)     // it works when rising edge triggered
{
	if(htim->Instance != htim3.Instance)
		return;
	
	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_1)  // if the interrupt source is channel1
	{
		if (Is_First_Captured==0) // if the first value is not captured
		{
			IC_Val1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1); // read the first value
			Is_First_Captured = 1;  // set the first captured as true
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_FALLING); 	// Now change the polarity to falling edge
		}
		else if (Is_First_Captured==1)   // if the first is already captured
		{
			IC_Val2 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1);  // read second value
			__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

			if (IC_Val2 > IC_Val1)
			{
				Difference = IC_Val2-IC_Val1;
			}

			else if (IC_Val1 > IC_Val2)
			{
				Difference = (0xffff - IC_Val1) + IC_Val2;
			}

			Distance = Difference * 0.034/2;
			Is_First_Captured = 0; // set it back to false			
			bCalculatedDistance = 1;
			__HAL_TIM_SET_COUNTER(htim, 0); // reset the counter
			// set polarity to rising edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
			HAL_TIM_IC_Stop_IT(&htim3,TIM_CHANNEL_1);
		}
	}
}

void HCSR04_Read (void)
{
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_SET);  // pull the TRIG pin HIGH
	delay_us(10);  // wait for 10 us
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);  // pull the TRIG pin low

	HAL_TIM_IC_Start_IT(&htim3, TIM_CHANNEL_1);
}


void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == TIM7)
  {   
		ir_1sec = 1;
  }
}

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) // when receive, board sets bUART_RX = 1
{
 // bUART_RX =1;
	if(huart->Instance == huart2.Instance)
	{
		HW_VCOM_RxCpltCallback(huart);
	}
}
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
//	uint16_t oc_pulse = 0;
//	uint16_t oc_level = 0;
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM7_Init();
  MX_TIM3_Init();
  MX_TIM6_Init();
  MX_USART2_UART_Init();
  /* USER CODE BEGIN 2 */

//	HAL_UART_Transmit(&huart2, buff, strlen((const char *)buff), 100);	
//	HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1);
	HAL_TIM_Base_Start_IT(&htim7);
	HAL_TIM_Base_Start(&htim6);	
	Printf("%d\r\n",1234567890); // to check ATZ works
	CMD_Init();
//	// LED Light Level
//	oc_level = (htim3.Init.Period / 4);
//	HAL_TIM_PWM_Start(&htim3,TIM_CHANNEL_3);

  /* USER CODE END 2 */
	
  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */
		/*
		if (bUART_RX) 
		{
			bUART_RX = 0;
			HAL_UART_Transmit(&huart2, &uart_rcvbuf, 1, 300);
			HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1); // 1 is size. when board receive 1bit it interrupts
		}
		*/

		CMD_Process();
		if(ir_1sec)
		{
			ir_1sec = 0;
			if(bCalculatedDistance && Distance > 30)
			{
				bCalculatedDistance = 0;	
				Printf("%.3f\r\n",Distance);
			}
			HCSR04_Read();
		}

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_BYPASS;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART2;
  PeriphClkInit.Usart2ClockSelection = RCC_USART2CLKSOURCE_PCLK1;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */

static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_IC_InitTypeDef sConfigIC = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 8-1;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 65535;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_IC_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigIC.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;
  sConfigIC.ICSelection = TIM_ICSELECTION_DIRECTTI;
  sConfigIC.ICPrescaler = TIM_ICPSC_DIV1;
  sConfigIC.ICFilter = 0;
  if (HAL_TIM_IC_ConfigChannel(&htim3, &sConfigIC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIMEx_RemapConfig(&htim3, TIM3_TI1_GPIO) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */

  /* USER CODE END TIM3_Init 2 */

}

/**
  * @brief TIM6 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM6_Init(void)
{

  /* USER CODE BEGIN TIM6_Init 0 */

  /* USER CODE END TIM6_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM6_Init 1 */

  /* USER CODE END TIM6_Init 1 */
  htim6.Instance = TIM6;
  htim6.Init.Prescaler = 8 - 1;
  htim6.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim6.Init.Period = 65535;
  htim6.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim6) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim6, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM6_Init 2 */

  /* USER CODE END TIM6_Init 2 */

}

/**
  * @brief TIM7 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM7_Init(void)
{

  /* USER CODE BEGIN TIM7_Init 0 */

  /* USER CODE END TIM7_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM7_Init 1 */

  /* USER CODE END TIM7_Init 1 */
  htim7.Instance = TIM7;
  htim7.Init.Prescaler = 8000-1;
  htim7.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim7.Init.Period = 1000-1;
  htim7.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim7) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim7, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM7_Init 2 */

  /* USER CODE END TIM7_Init 2 */

}

/**
  * @brief USART2 Initialization Function
  * @param None
  * @retval None
  */
static void MX_USART2_UART_Init(void)
{

  /* USER CODE BEGIN USART2_Init 0 */

  /* USER CODE END USART2_Init 0 */

  /* USER CODE BEGIN USART2_Init 1 */
	HW_VCOM_Init(&huart2);
#if 0
  /* USER CODE END USART2_Init 1 */
  huart2.Instance = USART2;
  huart2.Init.BaudRate = 9600;
  huart2.Init.WordLength = UART_WORDLENGTH_8B;
  huart2.Init.StopBits = UART_STOPBITS_1;
  huart2.Init.Parity = UART_PARITY_NONE;
  huart2.Init.Mode = UART_MODE_TX_RX;
  huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart2.Init.OverSampling = UART_OVERSAMPLING_16;
  huart2.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart2.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart2) != HAL_OK)
  {
    Error_Handler();
  }
#endif
  /* USER CODE BEGIN USART2_Init 2 */

  /* USER CODE END USART2_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : TRIG_Pin */
  GPIO_InitStruct.Pin = TRIG_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH;
  HAL_GPIO_Init(TRIG_GPIO_Port, &GPIO_InitStruct);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```

## AT 로 LED 제어하기
✅ 세팅   
<img src="https://github.com/user-attachments/assets/0f7fe49c-9b21-47cd-9056-f1abfb97c396" width="600" height="340">   
   
✅ main.c   
```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "stm32l0xx_nucleo.h"
#include "stdio.h"
#include "string.h"
#include "hw_vcom.h"
#include "command.h"
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
#define BUFSIZE 128
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim3;
TIM_HandleTypeDef htim6;
TIM_HandleTypeDef htim7;

UART_HandleTypeDef huart2;

/* USER CODE BEGIN PV */
volatile uint32_t nVibCnt = 0;
volatile uint8_t bVib = 0;
volatile uint8_t ir_1sec = 0;

//for Ultra Sonic Sensor
volatile uint8_t bCalculatedDistance = 0;
volatile uint32_t IC_Val1 = 0;
volatile uint32_t IC_Val2 = 0;
volatile uint32_t Difference = 0;
volatile uint8_t Is_First_Captured = 0;  // is the first value captured ?
volatile float Distance  = 0;
uint8_t uart_rcvbuf;
volatile uint8_t bUART_RX = 0;
// for uart
static uint8_t buff[BUFSIZE]="Hello Nucleo-L073RZ\r\n"; // array should not be used with volatile keyword

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM7_Init(void);
static void MX_TIM3_Init(void);
static void MX_TIM6_Init(void);
static void MX_USART2_UART_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void delay(unsigned int delay_cnt)
{
		volatile int counter = 0;

		while(counter < delay_cnt) //delay loop
		{
			counter++;
		}
}

void delay_us (uint16_t us)
{
  __HAL_TIM_SET_COUNTER(&htim6, 0);
  while (__HAL_TIM_GET_COUNTER (&htim6) < us);
}

void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)     // it works when rising edge triggered
{
	if(htim->Instance != htim3.Instance)
		return;
	
	if (htim->Channel == HAL_TIM_ACTIVE_CHANNEL_1)  // if the interrupt source is channel1
	{
		if (Is_First_Captured==0) // if the first value is not captured
		{
			IC_Val1 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1); // read the first value
			Is_First_Captured = 1;  // set the first captured as true
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_FALLING); 	// Now change the polarity to falling edge
		}
		else if (Is_First_Captured==1)   // if the first is already captured
		{
			IC_Val2 = HAL_TIM_ReadCapturedValue(htim, TIM_CHANNEL_1);  // read second value
			__HAL_TIM_SET_COUNTER(htim, 0);  // reset the counter

			if (IC_Val2 > IC_Val1)
			{
				Difference = IC_Val2-IC_Val1;
			}

			else if (IC_Val1 > IC_Val2)
			{
				Difference = (0xffff - IC_Val1) + IC_Val2;
			}

			Distance = Difference * 0.034/2;
			Is_First_Captured = 0; // set it back to false			
			bCalculatedDistance = 1;
			__HAL_TIM_SET_COUNTER(htim, 0); // reset the counter
			// set polarity to rising edge
			__HAL_TIM_SET_CAPTUREPOLARITY(htim, TIM_CHANNEL_1, TIM_INPUTCHANNELPOLARITY_RISING);
			HAL_TIM_IC_Stop_IT(&htim3,TIM_CHANNEL_1);
		}
	}
}

void HCSR04_Read (void)
{
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_SET);  // pull the TRIG pin HIGH
	delay_us(10);  // wait for 10 us
	HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);  // pull the TRIG pin low

	HAL_TIM_IC_Start_IT(&htim3, TIM_CHANNEL_1);
}


void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  if(htim->Instance == TIM7)
  {   
		ir_1sec = 1;
  }
}

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) // when receive, board sets bUART_RX = 1
{
 // bUART_RX =1;
	if(huart->Instance == huart2.Instance)
	{
		HW_VCOM_RxCpltCallback(huart);
	}
}
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */
//	uint16_t oc_pulse = 0;
//	uint16_t oc_level = 0;
  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM7_Init();
  MX_TIM3_Init();
  MX_TIM6_Init();
  MX_USART2_UART_Init();
  /* USER CODE BEGIN 2 */

//	HAL_UART_Transmit(&huart2, buff, strlen((const char *)buff), 100);	
//	HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1);
	HAL_TIM_Base_Start_IT(&htim7);
	HAL_TIM_Base_Start(&htim6);	
	Printf("%d\r\n",1234567890); // to check ATZ works
	CMD_Init();
//	// LED Light Level
//	oc_level = (htim3.Init.Period / 4);
//	HAL_TIM_PWM_Start(&htim3,TIM_CHANNEL_3);

  /* USER CODE END 2 */
	
  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */
		/*
		if (bUART_RX) 
		{
			bUART_RX = 0;
			HAL_UART_Transmit(&huart2, &uart_rcvbuf, 1, 300);
			HAL_UART_Receive_IT(&huart2, &uart_rcvbuf, 1); // 1 is size. when board receive 1bit it interrupts
		}
		*/

		CMD_Process();
		if(ir_1sec)
		{
			ir_1sec = 0;
			if(bCalculatedDistance && Distance > 30)
			{
				bCalculatedDistance = 0;	
				Printf("%.3f\r\n",Distance);
			}
			HCSR04_Read();
		}

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

  /** Configure the main internal regulator output voltage
  */
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_BYPASS;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSE;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART2;
  PeriphClkInit.Usart2ClockSelection = RCC_USART2CLKSOURCE_PCLK1;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM3 Initialization Function
  * @param None
  * @retval None
  */

static void MX_TIM3_Init(void)
{

  /* USER CODE BEGIN TIM3_Init 0 */

  /* USER CODE END TIM3_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_IC_InitTypeDef sConfigIC = {0};

  /* USER CODE BEGIN TIM3_Init 1 */

  /* USER CODE END TIM3_Init 1 */
  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 8-1;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 65535;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_IC_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigIC.ICPolarity = TIM_INPUTCHANNELPOLARITY_RISING;
  sConfigIC.ICSelection = TIM_ICSELECTION_DIRECTTI;
  sConfigIC.ICPrescaler = TIM_ICPSC_DIV1;
  sConfigIC.ICFilter = 0;
  if (HAL_TIM_IC_ConfigChannel(&htim3, &sConfigIC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIMEx_RemapConfig(&htim3, TIM3_TI1_GPIO) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM3_Init 2 */

  /* USER CODE END TIM3_Init 2 */

}

/**
  * @brief TIM6 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM6_Init(void)
{

  /* USER CODE BEGIN TIM6_Init 0 */

  /* USER CODE END TIM6_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM6_Init 1 */

  /* USER CODE END TIM6_Init 1 */
  htim6.Instance = TIM6;
  htim6.Init.Prescaler = 8 - 1;
  htim6.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim6.Init.Period = 65535;
  htim6.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim6) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim6, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM6_Init 2 */

  /* USER CODE END TIM6_Init 2 */

}

/**
  * @brief TIM7 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM7_Init(void)
{

  /* USER CODE BEGIN TIM7_Init 0 */

  /* USER CODE END TIM7_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};

  /* USER CODE BEGIN TIM7_Init 1 */

  /* USER CODE END TIM7_Init 1 */
  htim7.Instance = TIM7;
  htim7.Init.Prescaler = 8000-1;
  htim7.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim7.Init.Period = 1000-1;
  htim7.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim7) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim7, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM7_Init 2 */

  /* USER CODE END TIM7_Init 2 */

}

/**
  * @brief USART2 Initialization Function
  * @param None
  * @retval None
  */
static void MX_USART2_UART_Init(void)
{

  /* USER CODE BEGIN USART2_Init 0 */

  /* USER CODE END USART2_Init 0 */

  /* USER CODE BEGIN USART2_Init 1 */
	HW_VCOM_Init(&huart2);
#if 0
  /* USER CODE END USART2_Init 1 */
  huart2.Instance = USART2;
  huart2.Init.BaudRate = 9600;
  huart2.Init.WordLength = UART_WORDLENGTH_8B;
  huart2.Init.StopBits = UART_STOPBITS_1;
  huart2.Init.Parity = UART_PARITY_NONE;
  huart2.Init.Mode = UART_MODE_TX_RX;
  huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart2.Init.OverSampling = UART_OVERSAMPLING_16;
  huart2.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart2.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart2) != HAL_OK)
  {
    Error_Handler();
  }
#endif
  /* USER CODE BEGIN USART2_Init 2 */

  /* USER CODE END USART2_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};
/* USER CODE BEGIN MX_GPIO_Init_1 */
/* USER CODE END MX_GPIO_Init_1 */

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(TRIG_GPIO_Port, TRIG_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : TRIG_Pin */
  GPIO_InitStruct.Pin = TRIG_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_VERY_HIGH;
  HAL_GPIO_Init(TRIG_GPIO_Port, &GPIO_InitStruct);

/* USER CODE BEGIN MX_GPIO_Init_2 */
	BSP_LED_Init(LED2);
/* USER CODE END MX_GPIO_Init_2 */
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```
   
✅ at.c   
```c
/*******************************************************************************
 * @file    at.c
 * @author  MCD Application Team
 * @version V1.1.2
 * @date    08-September-2017
 * @brief   at command API
 ******************************************************************************
 * @attention
 *
 * <h2><center>&copy; Copyright (c) 2017 STMicroelectronics International N.V.
 * All rights reserved.</center></h2>
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted, provided that the following conditions are met:
 *
 * 1. Redistribution of source code must retain the above copyright notice,
 *    this list of conditions and the following disclaimer.
 * 2. Redistributions in binary form must reproduce the above copyright notice,
 *    this list of conditions and the following disclaimer in the documentation
 *    and/or other materials provided with the distribution.
 * 3. Neither the name of STMicroelectronics nor the names of other
 *    contributors to this software may be used to endorse or promote products
 *    derived from this software without specific written permission.
 * 4. This software, including modifications and/or derivative works of this
 *    software, must execute solely and exclusively on microcontroller or
 *    microprocessor devices manufactured by or for STMicroelectronics.
 * 5. Redistribution and use of this software other than as permitted under
 *    this license is void and will automatically terminate your rights under
 *    this license.
 *
 * THIS SOFTWARE IS PROVIDED BY STMICROELECTRONICS AND CONTRIBUTORS "AS IS"
 * AND ANY EXPRESS, IMPLIED OR STATUTORY WARRANTIES, INCLUDING, BUT NOT
 * LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
 * PARTICULAR PURPOSE AND NON-INFRINGEMENT OF THIRD PARTY INTELLECTUAL PROPERTY
 * RIGHTS ARE DISCLAIMED TO THE FULLEST EXTENT PERMITTED BY LAW. IN NO EVENT
 * SHALL STMICROELECTRONICS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
 * INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
 * LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA,
 * OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
 * LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
 * NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE,
 * EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 *
 ******************************************************************************
 */

/* Includes ------------------------------------------------------------------*/
#include <stdbool.h>
#include <stdint.h>
#include <string.h>
#include "at.h"

/* External variables --------------------------------------------------------*/
/* Private typedef -----------------------------------------------------------*/
/* Private define ------------------------------------------------------------*/
/**
 * @brief Max size of the data that can be received
 */
#define MAX_RECEIVED_DATA 255

/* Private macro -------------------------------------------------------------*/
/**
 * @brief Macro to return when an error occurs
 */
#define CHECK_STATUS(status) do {                    \
    ATEerror_t at_status = translate_status(status); \
    if (at_status != AT_OK) { return at_status; }    \
  } while (0)

/* Private variables ---------------------------------------------------------*/
/* Private function prototypes -----------------------------------------------*/

/* Exported functions ------------------------------------------------------- */
ATEerror_t at_return_ok(const char *param)
{
  return AT_OK;
}

ATEerror_t at_return_error(const char *param)
{
  return AT_ERROR;
}

ATEerror_t at_reset(const char * param)
{
	NVIC_SystemReset();
	return AT_OK;
}
ATEerror_t at_set_led(const char *param) {
	switch(param[0])
	{
		case '0':
			HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_RESET);
			break;
		case '1':
			HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET);			
			break;
		default:
			break;
	}
	return AT_OK;
}
```
   
✅ at.h   
```c
/*******************************************************************************
 * @file    at.h
 * @author  MCD Application Team
 * @version V1.1.2
 * @date    08-September-2017
 * @brief   Header for driver at.c module
 ******************************************************************************
 * @attention
 *
 * <h2><center>&copy; Copyright (c) 2017 STMicroelectronics International N.V.
 * All rights reserved.</center></h2>
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted, provided that the following conditions are met:
 *
 * 1. Redistribution of source code must retain the above copyright notice,
 *    this list of conditions and the following disclaimer.
 * 2. Redistributions in binary form must reproduce the above copyright notice,
 *    this list of conditions and the following disclaimer in the documentation
 *    and/or other materials provided with the distribution.
 * 3. Neither the name of STMicroelectronics nor the names of other
 *    contributors to this software may be used to endorse or promote products
 *    derived from this software without specific written permission.
 * 4. This software, including modifications and/or derivative works of this
 *    software, must execute solely and exclusively on microcontroller or
 *    microprocessor devices manufactured by or for STMicroelectronics.
 * 5. Redistribution and use of this software other than as permitted under
 *    this license is void and will automatically terminate your rights under
 *    this license.
 *
 * THIS SOFTWARE IS PROVIDED BY STMICROELECTRONICS AND CONTRIBUTORS "AS IS"
 * AND ANY EXPRESS, IMPLIED OR STATUTORY WARRANTIES, INCLUDING, BUT NOT
 * LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
 * PARTICULAR PURPOSE AND NON-INFRINGEMENT OF THIRD PARTY INTELLECTUAL PROPERTY
 * RIGHTS ARE DISCLAIMED TO THE FULLEST EXTENT PERMITTED BY LAW. IN NO EVENT
 * SHALL STMICROELECTRONICS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
 * INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
 * LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA,
 * OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
 * LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
 * NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE,
 * EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 *
 ******************************************************************************
 */

/* Define to prevent recursive inclusion -------------------------------------*/
#ifndef __AT_H__
#define __AT_H__

#ifdef __cplusplus
extern "C" {
#endif

/* Includes ------------------------------------------------------------------*/
#include <stdint.h>
#include <string.h>
#include "main.h"
	
/* Exported types ------------------------------------------------------------*/
/*
 * AT Command Id errors. Note that they are in sync with ATError_description static array
 * in command.c
 */
typedef enum eATEerror
{
  AT_OK = 0,
  AT_ERROR,
  AT_PARAM_ERROR,
  AT_BUSY_ERROR,
  AT_TEST_PARAM_OVERFLOW,
  AT_RX_ERROR,
  AT_MAX,
} ATEerror_t;

/* Exported constants --------------------------------------------------------*/
/* External variables --------------------------------------------------------*/
/* Exported macros -----------------------------------------------------------*/
/* AT Command strings. Commands start with AT */
#define ATZ_XXX      "+XXX"
#define AT_RESET     "Z"
#define AT_LED2CON	 "+LED2CON"
#define AT_US				 "+US"   // we can controll US by our decision... ultra sonic
ATEerror_t at_set_led(const char *param);

ATEerror_t at_reset(const char *param);
/* Exported functions ------------------------------------------------------- */
/**
 * @brief  Return AT_OK in all cases
 * @param  Param string of the AT command - unused
 * @retval AT_OK
 */
ATEerror_t at_return_ok(const char *param);

/**
 * @brief  Return AT_ERROR in all cases
 * @param  Param string of the AT command - unused
 * @retval AT_ERROR
 */
ATEerror_t at_return_error(const char *param);
  
#ifdef __cplusplus
}
#endif

#endif /* __AT_H__ */

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/
```
   
✅ command.c   
```c
/*******************************************************************************
 * @file    command.c
 * @author  MCD Application Team
 * @version V1.1.2
 * @date    08-September-2017
 * @brief   main command driver dedicated to command AT
 ******************************************************************************
 * @attention
 *
 * <h2><center>&copy; Copyright (c) 2017 STMicroelectronics International N.V.
 * All rights reserved.</center></h2>
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted, provided that the following conditions are met:
 *
 * 1. Redistribution of source code must retain the above copyright notice,
 *    this list of conditions and the following disclaimer.
 * 2. Redistributions in binary form must reproduce the above copyright notice,
 *    this list of conditions and the following disclaimer in the documentation
 *    and/or other materials provided with the distribution.
 * 3. Neither the name of STMicroelectronics nor the names of other
 *    contributors to this software may be used to endorse or promote products
 *    derived from this software without specific written permission.
 * 4. This software, including modifications and/or derivative works of this
 *    software, must execute solely and exclusively on microcontroller or
 *    microprocessor devices manufactured by or for STMicroelectronics.
 * 5. Redistribution and use of this software other than as permitted under
 *    this license is void and will automatically terminate your rights under
 *    this license.
 *
 * THIS SOFTWARE IS PROVIDED BY STMICROELECTRONICS AND CONTRIBUTORS "AS IS"
 * AND ANY EXPRESS, IMPLIED OR STATUTORY WARRANTIES, INCLUDING, BUT NOT
 * LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
 * PARTICULAR PURPOSE AND NON-INFRINGEMENT OF THIRD PARTY INTELLECTUAL PROPERTY
 * RIGHTS ARE DISCLAIMED TO THE FULLEST EXTENT PERMITTED BY LAW. IN NO EVENT
 * SHALL STMICROELECTRONICS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
 * INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
 * LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA,
 * OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
 * LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
 * NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE,
 * EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 *
 ******************************************************************************
 */

/* Includes ------------------------------------------------------------------*/
#include <stdlib.h>
#include "at.h"
#include "command.h"
#include "hw_vcom.h"

/* comment the following to have help message */
/* #define NO_HELP */

/* Private typedef -----------------------------------------------------------*/
/**
 * @brief  Structure defining an AT Command
 */
struct ATCommand_s {
  const char *string;                       /*< command string, after the "AT" */
  const int size_string;                    /*< size of the command string, not including the final \0 */
  ATEerror_t (*get)(const char *param);     /*< =? after the string to get the current value*/
  ATEerror_t (*set)(const char *param);     /*< = (but not =?\0) after the string to set a value */
  ATEerror_t (*run)(const char *param);     /*< \0 after the string - run the command */
#if !defined(NO_HELP)
  const char *help_string;                  /*< to be printed when ? after the string */
#endif
};

/* Private define ------------------------------------------------------------*/
#define CMD_SIZE 128

/* Private macro -------------------------------------------------------------*/
/* Private variables ---------------------------------------------------------*/
static const char *const ATError_description[] =
{
  "\r\nOK\r\n",                     /* AT_OK */
  "\r\nAT_ERROR\r\n",               /* AT_ERROR */
  "\r\nAT_PARAM_ERROR\r\n",         /* AT_PARAM_ERROR */
  "\r\nAT_BUSY_ERROR\r\n",          /* AT_BUSY_ERROR */
  "\r\nAT_TEST_PARAM_OVERFLOW\r\n", /* AT_TEST_PARAM_OVERFLOW */
  "\r\nAT_NO_NETWORK_JOINED\r\n",   /* AT_NO_NET_JOINED */
  "\r\nAT_RX_ERROR\r\n",            /* AT_RX_ERROR */
  "\r\nerror unknown\r\n",          /* AT_MAX */
};

/**
 * @brief  Array of all supported AT Commands
 */
static const struct ATCommand_s ATCommand[] =
{
  {
    .string = AT_RESET,
    .size_string = sizeof(AT_RESET) - 1,
#ifndef NO_HELP
    .help_string = "AT"AT_RESET ": Trig a reset of the MCU\r\n",
#endif
    .get = at_return_error,
    .set = at_return_error,
    .run = at_reset,
  },
	{
		.string = AT_LED2CON,
		.size_string = sizeof(AT_LED2CON) - 1,
#ifndef NO_HELP
    .help_string = "AT"AT_LED2CON ": Control LED2\r\n",
#endif
    .get = at_return_error,
    .set = at_set_led,
    .run = at_return_error,		
	}
};


/* Private function prototypes -----------------------------------------------*/

/**
 * @brief  Print a string corresponding to an ATEerror_t
 * @param  The AT error code
 * @retval None
 */
static void com_error(ATEerror_t error_type);


/**
 * @brief  Parse a command and process it
 * @param  The command
 * @retval None
 */
static void parse_cmd(const char *cmd);

/* Exported functions ---------------------------------------------------------*/

void CMD_Init(void)
{

}

void CMD_Process(void)
{
  static char command[CMD_SIZE];
  static unsigned i = 0;

  /* Process all commands */
  while (IsNewCharReceived() == SET)
  {
    command[i] = GetNewChar();

    Printf("%c", command[i]);

    if (command[i] == AT_ERROR_RX_CHAR)
    {
      i = 0;
      com_error(AT_RX_ERROR);
      break;
    }
    else
    if ((command[i] == '\r') || (command[i] == '\n'))
    {
      if (i != 0)
      {
        command[i] = '\0';
        parse_cmd(command);
        i = 0;
      }
    }
    else
    if (i == (CMD_SIZE - 1))
    {
      i = 0;
      com_error(AT_TEST_PARAM_OVERFLOW);
    }
    else
    {
      i++;
    }
  }
}

/* Private functions ---------------------------------------------------------*/
static void com_error(ATEerror_t error_type)
{
  if (error_type > AT_MAX)
  {
    error_type = AT_MAX;
  }
  Printf(ATError_description[error_type]);
}

static void parse_cmd(const char *cmd)
{
  ATEerror_t status = AT_OK;
  const struct ATCommand_s *Current_ATCommand;
  int i;

  if ((cmd[0] != 'A') || (cmd[1] != 'T'))
  {
    status = AT_ERROR;
  }
  else
  if (cmd[2] == '\0')
  {
    /* status = AT_OK; */
  }
  else
  if (cmd[2] == '?')
  {
#ifdef NO_HELP
#else
    Printf("AT+<CMD>?        : Help on <CMD>\r\n"
              "AT+<CMD>         : Run <CMD>\r\n"
              "AT+<CMD>=<value> : Set the value\r\n"
              "AT+<CMD>=?       : Get the value\r\n");
    for (i = 0; i < (sizeof(ATCommand) / sizeof(struct ATCommand_s)); i++)
    {
      Printf(ATCommand[i].help_string);
    }
#endif
  }
  else
  {
    /* point to the start of the command, excluding AT */
    status = AT_ERROR;
    cmd += 2;
    for (i = 0; i < (sizeof(ATCommand) / sizeof(struct ATCommand_s)); i++)
    {
      if (strncmp(cmd, ATCommand[i].string, ATCommand[i].size_string) == 0)
      {
        Current_ATCommand = &(ATCommand[i]);
        /* point to the string after the command to parse it */
        cmd += Current_ATCommand->size_string;

        /* parse after the command */
        switch (cmd[0])
        {
          case '\0':    /* nothing after the command */
            status = Current_ATCommand->run(cmd);
            break;
          case '=':
            if ((cmd[1] == '?') && (cmd[2] == '\0'))
            {
              status = Current_ATCommand->get(cmd + 1);
            }
            else
            {
              status = Current_ATCommand->set(cmd + 1);
            }
            break;
          case '?':
#ifndef NO_HELP
            Printf(Current_ATCommand->help_string);
#endif
            status = AT_OK;
            break;
          default:
            /* not recognized */
            break;
        }

        /* we end the loop as the command was found */
        break;
      }
    }
  }

  com_error(status);
}

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/
```

##
