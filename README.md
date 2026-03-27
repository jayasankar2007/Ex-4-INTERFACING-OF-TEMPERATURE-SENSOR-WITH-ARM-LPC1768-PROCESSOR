# Ex-4-INTERFACING-OF-TEMPERATURE-SENSOR-WITH-ARM-LPC1768-PROCESSOR
## AIM:
To write an embedded c program to interface TEMPERATURE SENSOR with ARM processor

## COMPONENTS REQUIRED:
## HARDWARE:
LPC1768 / LPC1343

LM 35

LCD module

## SOFTWARE:

Coocox IDE Docklight

## PROCEDURE:

Step 1: Go to start All programs  COIDE

Step 2: Give a suitable file name for your project and give the destination folder and then next Step 3: Go to chip NXP LPC 13XX  LPC1343  Next

Step 4: Select the required library file (SYSCON and GPIO) from the repository Step 5: A new project will be created

Step 6: Double click on main.c and type the program

Step 7: Add the required library source file to the project (Right click on include Add file to group and add the source file)

Step 8: Build the program using build option

Step 9: Flash the program by clicking on download code to flash Step 10: Interface the required component and note down the output For Docklight:

Step 1: Go to start Docklight Ok

Step 2: Start with the blank project /blank script continue 4647

Step 3: Enable keyboard console by clicking on it

Step 4: Now flash (run) the program on COIDE, the output will be displayed on the serial window in the Docklight System window

## ADD FILES:

### Repository:

Clibrary , retarget printf, CMSIS core, CMSIS boot, common header files, SYSCON ,GPIO, IOCON, UART,ADC , time.

### Source files:

simple example.c, Uart Receiver interrupt.c, lcd.c, lcd.h

## PROGRAM
```

#include <LPC17xx.h>
#include "lcd.h"
#include "delay.h"     
#include "gpio.h"
#define SBIT_BURST      16u
#define SBIT_START      24u
#define SBIT_PDN        21u
#define SBIT_EDGE       27u 
#define SBIT_DONE       31u
#define SBIT_RESULT     4u
#define SBIT_CLCKDIV    8u

unsigned char key(void);

int main() 
{
 	uint16_t adc_result,d0,d1;
    SystemInit();
	
 	 
	LCD_SetUp(P1_18,P1_19,P1_20,P_NC,P_NC,P_NC,P_NC,P1_21,P1_22,P1_23,P1_24); 
    LCD_Init(2,16);
	LCD_CmdWrite(0x80);
	LCD_String("**TEMPRATURE**");
	  
    LPC_SC->PCONP |= (1 << 12);                            /* Enable CLOCK for internal ADC controller */

    LPC_ADC->ADCR = ((1<<SBIT_PDN) | (10<<SBIT_CLCKDIV));  /*Set the clock and Power ON ADC module */

    LPC_PINCON->PINSEL1|= 0x01<<14;      	                 /* Select the P0_23 AD0[0] for ADC function */
   	

  
    while (1) 
    {
       LPC_ADC->ADCR  |= 0x01;                                /* Select Channel 0 by setting 0th bit of ADCR */
        DELAY_us(10);                                          /* allow the channel voltage to stabilize*/
         
        util_BitSet(LPC_ADC->ADCR,SBIT_START);                 /*Start ADC conversion*/
        
        while(util_GetBitStatus(LPC_ADC->ADGDR,SBIT_DONE)==0); /* wait till conversion completes */
        
        adc_result = (LPC_ADC->ADGDR >> SBIT_RESULT) & 0xfff;  /*Read the 12bit adc result*/
		adc_result=	adc_result/11.21;
    	d0=adc_result/10;
		d1=adc_result%10;
		LCD_CmdWrite(0xC0);
		LCD_CmdWrite(0x06);
      
	     LCD_Data(d0+48);
	     LCD_Data(d1+48);                                         
                         
	   
    }    
}
```
## OUTPUT
![WhatsApp Image 2026-03-17 at 10 53 26 AM](https://github.com/user-attachments/assets/78f25964-91ee-4203-bfd8-92c523dc1c10)

## RESULT:
Thus, an embedded c program to interface temperature sensor with ARM processor was executed and output was verified successfully.
 
