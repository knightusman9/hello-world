/**
  ******************************************************************************
  * @file    Sending commands to the STM32F4 Discovery via USART using a USB to TTL Uart Serial converter
  * @author  Lee Dyche - Tecsploit.com
  * @version V1.0.0
  * @date    05/05/2014
  * @brief   Learn how to connect your STM32F4 Discovery to your PC using one of these cheap connector cables, and then send
  * commands to the STM32F4 Discovery using a simple tool such as Putty
  ******************************************************************************
  * @attention
  * This is program is provided as is with no warranty!!
  ******************************************************************************
  */

#include <stm32f4xx.h>
#include <stdio.h>
#include <stm32f4xx_conf.h>
#include <stm32f4xx_exti.h>
#include <misc.h>
#include <stm32f4xx_rcc.h>
#include <stm32f4xx_gpio.h>
#include <stm32f4xx_usart.h>
#include "stm32f4_discovery.h"
#include "TecsploitUtils.h"

#include "usbd_cdc_core.h"
#include "usbd_usr.h"
#include "usbd_desc.h"
#include "usbd_cdc_vcp.h"
#include "usb_dcd_int.h"










#define BUFFER_SIZE 16

//notice the use of the volatile keyword this is important as without it the compiler may make
//optimisations assuming the value of this variable hasnt changed
volatile char received_buffer[BUFFER_SIZE+1];

static uint8_t DataReceivedCounter = 0; //tracks the number of characters received so far, reset after each command

//function prototypes
void USARTCommandReceived(char * command);
void ClearCommand();
void Delay(int nCount);
void ConfigureUsart(int baudrate);

//Configures the USART using pin B6 as TX and B7 as RX and the passed in baudrate
void ConfigureUsart(int baudrate){

	//structures used configure the hardware
	GPIO_InitTypeDef GPIO_InitStruct;
	USART_InitTypeDef USART_InitStruct;
	NVIC_InitTypeDef NVIC_InitStructure;

	//enable the clocks for the GPIOB and the USART
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_USART1, ENABLE);
	RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOB, ENABLE);

	//Initialise pins GPIOB 6 and GPIOB 7
	GPIO_InitStruct.GPIO_Pin = GPIO_Pin_6 | GPIO_Pin_7;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_AF; //we are setting the pin to be alternative function
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_InitStruct.GPIO_OType = GPIO_OType_PP;
	GPIO_InitStruct.GPIO_PuPd = GPIO_PuPd_UP;
	GPIO_Init(GPIOB, &GPIO_InitStruct);

	//Connect the TX and RX pins to their alternate function pins
	GPIO_PinAFConfig(GPIOB, GPIO_PinSource6, GPIO_AF_USART1); //
	GPIO_PinAFConfig(GPIOB, GPIO_PinSource7, GPIO_AF_USART1);

	//configure USART
	USART_InitStruct.USART_BaudRate = baudrate;
	USART_InitStruct.USART_WordLength = USART_WordLength_8b;
	USART_InitStruct.USART_StopBits = USART_StopBits_1;
	USART_InitStruct.USART_Parity = USART_Parity_No;
	USART_InitStruct.USART_HardwareFlowControl = USART_HardwareFlowControl_None;
	USART_InitStruct.USART_Mode = USART_Mode_Tx | USART_Mode_Rx; //enable send and receive (Tx and Rx)
	USART_Init(USART1, &USART_InitStruct);

	//Enable the interupt
	USART_ITConfig(USART1, USART_IT_RXNE, ENABLE);

	NVIC_InitStructure.NVIC_IRQChannel = USART1_IRQn;
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
	NVIC_Init(&NVIC_InitStructure);

	// finally this enables the complete USART1 peripheral
	USART_Cmd(USART1, ENABLE);
}

//writes out a string to the passed in usart. The string is passed as a pointer
void SendData(USART_TypeDef* USARTx, volatile char *s){

	while(*s){
		// wait until data register is empty
		while( !(USARTx->SR & 0x00000040) );
		USART_SendData(USARTx, *s);
		*s++;
	}
}

int main(void) {
	STM_EVAL_LEDInit(LED3);
	STM_EVAL_LEDInit(LED4);
	STM_EVAL_LEDInit(LED5);
	STM_EVAL_LEDInit(LED6);
	ConfigureUsart(9600); // setup usart 1 with a baudrate of 9600

  SendData(USART1, "tecsploits USART connection initialised, visit us @tecsploit.com");

  while (1){
	  Delay(900000);
  }
}

//This method is executed when data is received on the RX line (this is the interrupt), this method can process the
//data thats been received and decide what to do. It is executed for each character received, it reads each character
//and checks to see if it received the enter key (ascii code 13) or if the total number of characters received is greater
//that the buffer size.
//Note that there is no reference to this method in our setup code, this is because the name of this method is defined in the
//startup_stm32f4xx.S (you can find this in the startup_src folder). When listening for interrupts from USART 2 or 3 you would
//define methods named USART2_IRQHandler or USART3_IRQHandler
void USART1_IRQHandler(void){
	//check the type of interrupt to make sure we have received some data.
	if( USART_GetITStatus(USART1, USART_IT_RXNE) ){
		char t = USART1->DR; //Read the character that we have received

		if( (DataReceivedCounter < BUFFER_SIZE) && t != 13 ){
			received_buffer[DataReceivedCounter] = t;
			DataReceivedCounter++;
		}
		else{ // otherwise reset the character counter and print the received string
			DataReceivedCounter = 0;
			//only raise a command event if the enter key was pressed otherwise just clear
			if(t == 13){
				USARTCommandReceived(received_buffer);
			}

			ClearCommand();

		}
	}
}

//this method is called when a command is received from the USART, a command is only received when enter
//is pressed, if the buffer length is exceeded the buffer is cleared without raising a command
void USARTCommandReceived(char * command){
	SendData(USART1, received_buffer);

	if        (compare(command, "L5ON") == 0){
		STM_EVAL_LEDOn(LED5);
	}else 	if(compare(command, "L5OFF") == 0){
		STM_EVAL_LEDOff(LED5);

	}else 	if(compare(command, "L6ON") == 0){
		STM_EVAL_LEDOn(LED6);
	}else 	if(compare(command, "L6OFF") == 0){
		STM_EVAL_LEDOff(LED6);

	}else 	if(compare(command, "L3ON") == 0){
		STM_EVAL_LEDOn(LED3);
	}else 	if(compare(command, "L3OFF") == 0){
		STM_EVAL_LEDOff(LED3);

	}else 	if(compare(command, "L4ON") == 0){
		STM_EVAL_LEDOn(LED4);
	}else 	if(compare(command, "L4OFF") == 0){
		STM_EVAL_LEDOff(LED4);
	}
}
void ClearCommand(){
	int i =0;
	for(i=0;i < BUFFER_SIZE; i++){
		received_buffer[i] = 0;
	}

}

void Delay(int nCount) {
  while(nCount--) {
  }
}
