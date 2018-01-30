---
layout: post
title:  "nucleo f103rb 사용하기 - 시리얼 통신 - 2"
categories: [avr_arm, stm32]
---

# 시리얼 통신 - 수신값으로 GPIO 제어

시리얼 통신은 GPIO와 밀접한 관계가 있습니다.

외부에서 값을 입력하거나, 보드의 상태를 볼 때 시리얼 통신을 주로 사용하기 때문입니다.

이번 시간에는 시리얼 통신으로 LED 제어와 버튼의 입력 상태를 받아보도록 하겠습니다.

### STM32Cube

이전 시간의 파일에서 File - Save Project As 혹은 Ctrl + A (다른이름으로 저장)를 눌러줍니다.

![]({{ site.url }}/img/nucleo/serial_2/0.png)

PC13에 GPIO_Input을, PA5에 GPIO_Output을 설정해 줍니다.

![]({{ site.url }}/img/nucleo/serial_2/1.png)

Configuration의 GPIO에서 PA5의 라벨에는 internal_led를

PC13의 라벨에는 b1을 적어줍니다.

![]({{ site.url }}/img/nucleo/serial_2/2.png)

이제 코드를 생성(톱니바퀴)해 줍니다.

### uVision5

HAL_UART_Transmit 함수와 HAL_UART_Receive 함수

그리고 이전 시간에 사용했던 HAL_GPIO_ReadPin과 HAL_GPIO_WritePin을 사용합니다.

USER CODE BIGIN 2 밑에 아래 변수들과 함수를 추가해 줍니다.

b_in과 pin_state만 추가해 주면 됩니다.

~~~
  /* USER CODE BEGIN 2 */
	
	HAL_StatusTypeDef RcvStat ;
	uint8_t bufftx[10] = "Hello!\n" ;
	uint8_t b_in[10] = "b_in\n" ;
	uint8_t UsartData[10] ;
	uint8_t pin_state ;
	
	HAL_UART_Transmit(&huart2, bufftx, 10, 100) ;
~~~

USER CODE BIGIN 3 에는 아래와 같이 추가해 줍니다.

~~~
  /* USER CODE BEGIN 3 */
		
	RcvStat = HAL_UART_Receive(&huart2, UsartData, 1, 100) ;
		
	if (RcvStat == HAL_OK) {
		if (UsartData[0] == 'a') {  // if serial input is 'a', led on
			HAL_GPIO_WritePin(internal_led_GPIO_Port, internal_led_Pin, GPIO_PIN_SET) ;
		} else if (UsartData[0] == 'b') {  // if serial input is 'a', led off
			HAL_GPIO_WritePin(internal_led_GPIO_Port, internal_led_Pin, GPIO_PIN_RESET) ;
		}
		
		HAL_UART_Transmit(&huart2, UsartData, 1, 100) ;
	}
	
	pin_state = HAL_GPIO_ReadPin(b1_GPIO_Port, b1_Pin) ;
	
	if (!pin_state) {  // if b1 is low, send b_in
		HAL_UART_Transmit(&huart2, b_in, 10, 100) ;
	}
~~~

저번 시간과 같이 Serial 통신 프로그램을 설정해 줍니다.

RESET 버튼을 눌러 보드를 재시작하면 Hello! 문자가 나옵니다.

![]({{ site.url }}/img/nucleo/serial_2/3.png)

B1 버튼을 누르면 b_in이 나옵니다.

![]({{ site.url }}/img/nucleo/serial_2/4.png)

문자열 'a'를 보내면 led가 켜지고 'b'를 보내면 꺼지게 됩니다.

![]({{ site.url }}/img/nucleo/serial_2/5.png)
