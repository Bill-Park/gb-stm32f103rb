---
layout: post
title:  "nucleo f103rb 사용하기 - 시리얼 통신 - 3"
categories: [avr_arm, stm32]
---

# 시리얼 통신 - printf 함수 사용

이제까지 통신 출력을 할 때 HAL_UART_Transmit 함수를 사용했었습니다.

HAL_UART_Transmit는 원하는 문자열을 보내기 위해서는 따로 메모리를 할당하여 변수를 미리 만들어야 했습니다.

이를 해결하기 위해 printf 함수를 이용하여 간단하게 원하는 문자열을 보내보도록 하겠습니다.

### STM32Cube

이번 시간에 할 내용은 저번 시간과 핀 배치가 똑같습니다.

이전 시간의 파일에서 File - Save Project As 혹은 Ctrl + A (다른이름으로 저장)를 눌러줍니다.

![]({{ site.url }}/img/nucleo/serial_3/0.png)

그리고 코드를 생성(톱니바퀴)해 줍니다.

### uVision5

USER CODE BEGIN 0 안에 fputc 함수를 넣어줍니다.

~~~
/* USER CODE BEGIN 0 */

int fputc(int ch, FILE *f)
{
	uint8_t temp[1] = {ch} ;
	
	HAL_UART_Transmit(&huart2, temp, 1, 50) ;
	
	return(ch) ;
}

/* USER CODE END 0 */
~~~

이제부터 printf 함수를 사용할 수 있습니다.

USER CODE BIGIN 3 에는 아래와 같이 추가해 줍니다.

~~~
/* USER CODE BEGIN 3 */
	
	printf("Hello\n") ;
	HAL_Delay(500) ;

~~~

Serial 통신 프로그램을 실행하고 보드를 재시작하여 "Hello"가 오는지 확인합니다.

![]({{ site.url }}/img/nucleo/serial_3/1.png)
