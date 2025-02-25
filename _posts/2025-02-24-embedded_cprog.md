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
