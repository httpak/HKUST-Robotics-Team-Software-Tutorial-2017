# Tutorial 1 - GPIO

Arthur: Emmett Yim<br>
Contact: yhyim@ust.hk

---

## Basic
### -Introduction-
**General-purpose input/output** (**GPIO**) is a generic pin on an integrated circuit or computer board whose behavior, including whether it is an *input* or *output* pin, is controllable by the user at run time.
---
### -Fundamental Program Structure-
1. Double click **ustrobo17_internal>ustrobo17_internal.uvprojx**
2. Open **user>main.c**
```C
//including header files
#include "main.h"

int main()
{
   //initialization
   ticks_init();
   tft_init(0,BLUE, WHITE, RED);
   
   //variables
   uint32_t ticks = 0;	
   //an infinite loop
      while(1)
      {
      //regulating time interval
      if(get_ticks() != ticks)
      {
         //updating the value stored in ticks
	 ticks=get_ticks();
	 
	 //print ticks on tft every 1000ms
	 if(ticks % 100 == 0)
	 {
	    tft_clear_line(1);
	    tft_prints(1,1,"%d",ticks/100%10);
	    tft_update();
	 }
	 
	 //some other tasks
	 if(ticks % 100 == 5)
	 {
	    //...
	 }
	 
	 if(ticks % 100 == 11)
	 {
	    //...
	 }
	 
	 if(ticks % 25 == 19)
	 {
	    //...
	 }
      }
   }
}

```
#### -Remark:
Try to replace ```int, long, long long, float, double``` for embedded system programming with the followings:

| Full name | Short form |
| :---: | :---: |
| uint8_t | u8 |
| uint16_t | u16 |
| uint32_t | u32 |
| int8_t | s8 |
| int16_t | s16 |
| int32_t | s32 |
| *uint64_t* | *u64* |
| *int64_t* | *s64* |
---
### -GPIO Configuration and i/o-
#### -Initialization
 In **library>gpio.h**, the following can be found:
* Function prototype
```C
void gpio_init(GPIO_ID gpio_id, GPIOMode_TypeDef gpio_mode);
```
* Definition of GPIO_ID
```C
typedef enum {
   GPIO1,
   GPIO2,
   GPIO3,
   GPIO4
} GPIO_ID;
```
 In **stm32f10x_std>stm32f10x_gpio.h**, the definition of **GPIOMode_TypeDef** can be found:
```C
typedef enum
{ GPIO_Mode_AIN = 0x0,
  GPIO_Mode_IN_FLOATING = 0x04,
  GPIO_Mode_IPD = 0x28,
  GPIO_Mode_IPU = 0x48,
  GPIO_Mode_Out_OD = 0x14,
  GPIO_Mode_Out_PP = 0x10,
  GPIO_Mode_AF_OD = 0x1C,
  GPIO_Mode_AF_PP = 0x18
}GPIOMode_TypeDef;
```
* For input, use ``` GPIO_Mode_IPU ```.
* For output, use ``` GPIO_Mode_Out_PP ```.

 Example:
```C
//***GPIO init***
//input
gpio_init(GPIO1,GPIO_Mode_IPD);
gpio_init(GPIO2,GPIO_Mode_IPD);
gpio_init(GPIO3,GPIO_Mode_IPD);
gpio_init(GPIO4,GPIO_Mode_IPD);
//output
gpio_init(GPIO1,GPIO_Mode_Out_PP);
gpio_init(GPIO2,GPIO_Mode_Out_PP);
gpio_init(GPIO3,GPIO_Mode_Out_PP);
gpio_init(GPIO4,GPIO_Mode_Out_PP);
```

#### -Read GPIO Input
 In **library>gpio.h**, function prototype of **gpio_read** can be found:
```C
u8 gpio_read(GPIO_ID gpio_id);
```
 Example:
```C
//capturing the input(1 for high, 0 for low) from GPIO1
u8 input = gpio_read(GPIO1);
```

#### -Read GPIO Output
 In **library>gpio.h**, the following can be found:
* Function prototype:
```C
void gpio_write(GPIO_ID gpio_id, BitAction BitVal);
```
 In **stm32f10x_std>stm32f10x_gpio.h**, the following can be found:
* Definition of BitAction:
```C
typedef enum
{ Bit_RESET = 0,
  Bit_SET
}BitAction;
```
* ```Bit_RESET``` means low
* ```Bit_SET``` means high

Example:
```C
//set GPIO1 to high
gpio_write(GPIO1,Bit_SET);
//set GPIO1 to low
gpio_write(GPIO1,Bit_RESET);
```
---
### -LED control-
#### -Initialization
 In **library>leds.h**, the following can be found:
* Function prototype
```C
void led_init(void);
```

#### -Led on
 In **library>leds.h**, the following can be found:
* Function prototype
```C
void led_on(LED_ID id);
```
* Definition of LED_ID
```C
typedef enum{
   LED1,
   LED2,
   LED3
}LED_ID;
```
 Example:
```C
//switch on led 1
led_on(LED1);
```

#### -Led off
 In **library>leds.h**, the following can be found:
 * Function prototype
```C
void led_off(LED_ID id);
```
 Example:
```C
//switch off led 1
led_off(LED1);
```

#### ***Classwork***
```
Construct a program that:
-leds will take turns to light up
 [LED1 on]>[LED2 on]>[LED3 on]>[LED1 on]>[LED2 on]>...
```
---
### -Pneumatic control-
#### -Initialization
 In **library>pneumatics.h**, the following can be found:
* Fucntion prototype
```C
void pneumatic_init(void);
```

#### -Pneumatic control
 In **library>pneumatics.h**, the following can be found:
* Fucntion prototype
```C
void pneumatic_control(PNEUMATIC_ID id, u8 state);
```
#### -Remark:
```state``` can either be ```1``` or ```0```

* Definition of PNEUMATIC_ID
```C
typedef enum{
   PNEUMATIC1,
   PNEUMATIC2,
   PNEUMATIC3,
   PNEUMATIC4
}PNEUMATIC_ID;
```
 Example:
```C
//set bit
pneumatic_control(PNEUMATIC1,1);
//reset bit
pneumatic_control(PNEUMATIC1,0);
```
---
### -Button-
#### -Initialization
 In **library>button.h**, the following can be found:
* Fucntion prototype
```C
void button_init(void);
```

#### -Button input
 In **library>button.h**, the following can be found:
* Fucntion prototype
```C
u8 read_button(BUTTON_ID id);
```
* Definition of BUTTON_ID
```C
typedef enum{
   BUTTON1,
   BUTTON2,
   BUTTON3
}BUTTON_ID;
```
 Example:
```C
u8 input = read_button(BUTTON1);
```
#### -Remark: ```input``` will be ```0``` when the button is pressed, and vise versa.

#### ***Classwork***
```
1. Construct a program that:
 - LEDi will light up only when BUTTONi is pressed

2. Construct a program that:
 - LEDi is toggled when BUTTONi is pressed

3. Construct a program that:
 - LEDi will light up only when BUTTONi is pressed
 - If multiple buttons are pressed, only LEDi will light up where BUTTONi is the first button pressed.

4. Construct a program that:
 - LEDi will light up only when BUTTONi is pressed
 - If multiple buttons are pressed, only LEDi will light up where BUTTONi is the first button pressed.
 - LEDi will flash once per 300*i ms when BUTTONi is pressed.

5.Construct a program that:
 - leds will take turns to light up when any button is pressed
   [LED1 on]->[LED2 on]->[LED3 on]->[LED1 on]->[LED2 on]->...
 - upon releasing buttons, the previous procedue stops and only one led remains to be on
```
---
### -Software debouncing-
#### -Definition
**Bouncing** is the tendency of any two metal contacts in an electronic device to generate multiple signals as the contacts close or open.<br>
**Debouncing** is any kind of hardware device or software that ensures that only a **single signal** will be acted upon for a single opening or closing of a contact.

#### -Implementation
```C
if(ticks % 50 == 1)
{
   static u8 debounce;
   //reset debounce if button is not pressed
   if(read_button(BUTTON1) && debounce)
      debounce = 0;
   //set debounce if button is initially pressed
   if(!read_button(BUTTON1) && !debounce)
      debounce = 1;
   //handle button triggered event after debouncing
   else if(!read_button(BUTTON1) && debounce)
   {
      //...
   }
}
```
---
### -Buzzer-
#### -Initialization
```C
void buzzer_init(void);
```

#### -Buzzer on
```C
void buzzer_on(void);
```

#### -Buzzer off
```C
void buzzer_off(void);
```

#### -Remark:
* It can be used to show state / debug, similar to leds
* Please do not distroy my ears
* Play music?

#### *Classwork*
```
1. Construct a program that:
 - Include "buzzer.h" in "main.h"
 - BUZZER will beep once per 1000*i ms when BUTTONi is pressed.
 - If multiple buttons are pressed, the BUZZER will only beep at the corresponding rate of the first button pressed.
```

---

## Extra
---
### -GPIO Mode-

Among
```C
typedef enum
{ GPIO_Mode_AIN = 0x0,
  GPIO_Mode_IN_FLOATING = 0x04,
  GPIO_Mode_IPD = 0x28,
  GPIO_Mode_IPU = 0x48,
  GPIO_Mode_Out_OD = 0x14,
  GPIO_Mode_Out_PP = 0x10,
  GPIO_Mode_AF_OD = 0x1C,
  GPIO_Mode_AF_PP = 0x18
}GPIOMode_TypeDef;
```
only ```GPIO_Mode_IPD```, ```GPIO_Mode_IPU```, ```GPIO_Mode_Out_OD``` and ```GPIO_Mode_Out_PP``` will be covered.

#### -GPIO_Mode_IPD
**Input Pull Down** (**IPD**) has to be set when the GPIO is connected to a pull down resistor.<br>

 Hardware connection:<br>
![when the GPIO is connected to a pull down resistor](https://i.imgur.com/sd7FdFC.jpg)

#### -GPIO_Mode_IPU
**Input Pull Up** (**IPU**) has to be set when the GPIO is connected to a pull up resistor.<br>

 Hardware connection:<br>
![when the GPIO is connected to a pull up resistor](https://i.imgur.com/iS3Od90.jpg)

#### -GPIO_Mode_Out_OD
**Output Open Drain** (**Out_OD**) can be used for the following function:
1. communication pin setting (e.g. i2c).
2. power external circuit which would consume higher voltage(>3.3V) with suitable hardware circuit.
3. many other usage that you may do research yourself.

#### -GPIO_Mode_Out_PP
**Output Push Pull** (**Out_PP**) outputs ```VCC``` when GPIO is set to ```high```; ```GND``` when GPIO is set to ```low```.<br>

Hardware connection:<br>
![outputs ```VCC``` when GPIO is set to ```high```; ```GND``` when GPIO is set to ```low```](https://i.imgur.com/UerwY9k.png)

---
### -EXTI & NVIC-

*the following code is taking GPIO1 (PA13) as an example*

#### -Definition of EXTI
**EXTI** means **external interrupt**.<br>
Interrupt is a prioritized event that would be handled first at any time, while temporarily pausing the current event until the interrupt is finished.<br>
External interrupt is an interrupt triggered externally, for example by gpio input.

#### -EXTI setup example
```C
EXTI_InitTypeDef EXTI_InitStructure;
GPIO_EXTILineConfig(GPIO_PortSourceGPIOA, GPIO_PinSource13);
EXTI_InitStructure.EXTI_Line = EXTI_Line13;
EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
//interrupt triggered by rising edge
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising ;
//interrupt triggered by falling edge
//EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling ; 
EXTI_InitStructure.EXTI_LineCmd = ENABLE;
EXTI_Init(&EXTI_InitStructure);
EXTI_GenerateSWInterrupt(EXTI_Line13);
```

#### -Definition of NVIC
**NVIC** means **Nested Vectored Interrupt Controller**.<br>
It controlls the handling of different interrupts(*EXTI, ADC and TIM*) and their priorities.

#### -NVIC setup example
```C
NVIC_InitTypeDef NVIC_InitStructure;
NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1);
NVIC_InitStructure.NVIC_IRQChannel = EXTI15_10_IRQn;
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1;
NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
NVIC_Init(&NVIC_InitStructure);
```

#### -IRQ Handler example
```C
if ( EXTI_GetITStatus(EXTI_Line13) != RESET )
{
   //...
   EXTI_ClearITPendingBit(EXTI_Line13);
}
```

#### -Sample setup of GPIO1 as external interrupt source
```C
//GPIO setup
GPIO_InitTypeDef GPIO_InitStructure;
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
GPIO_InitStructure.GPIO_Pin =  GPIO_Pin_13;
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
GPIO_Init(GPIOA, &GPIO_InitStructure);
//EXTI setup
EXTI_InitTypeDef EXTI_InitStructure;
GPIO_EXTILineConfig(GPIO_PortSourceGPIOA, GPIO_PinSource13);
EXTI_InitStructure.EXTI_Line = EXTI_Line13;
EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;
//interrupt triggered by rising edge
EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising ;
//interrupt triggered by falling edge
//EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling ; 
EXTI_InitStructure.EXTI_LineCmd = ENABLE;
EXTI_Init(&EXTI_InitStructure);
EXTI_GenerateSWInterrupt(EXTI_Line13);
//NVIC setup
NVIC_InitTypeDef NVIC_InitStructure;
NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1);
NVIC_InitStructure.NVIC_IRQChannel = EXTI15_10_IRQn;
NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;
NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1;
NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;
NVIC_Init(&NVIC_InitStructure);
```

#### -Pin configuration reference for EXTI

| Pin | GPIO_Pin | GPIO_PinSource | EXTI_Line | NVIC_IRQChannel | IRQHandler |
| :---: | :---: | :---: | :---: | :---: | :---: |
| Pin 0 | GPIO_Pin_0 | GPIO_PinSource0 | EXTI_Line0 | EXTI0_IRQn | EXTI0_IRQHandler |
| Pin 1 | GPIO_Pin_1 | GPIO_PinSource1 | EXTI_Line1 | EXTI1_IRQn | EXTI1_IRQHandler |
| Pin 2 | GPIO_Pin_2 | GPIO_PinSource2 | EXTI_Line2 | EXTI2_IRQn | EXTI2_IRQHandler |
| Pin 3 | GPIO_Pin_3 | GPIO_PinSource3 | EXTI_Line3 | EXTI3_IRQn | EXTI3_IRQHandler |
| Pin 4 | GPIO_Pin_4 | GPIO_PinSource4 | EXTI_Line4 | EXTI4_IRQn | EXTI4_IRQHandler |
| Pin 5 - 9 | GPIO_Pin_5 to 9 | GPIO_PinSource5 to 9 | EXTI_Line9_5 | EXTI9_5_IRQn | EXTI9_5_IRQHandler |
| Pin 10 - 15 | GPIO_Pin_10 to 15 | GPIO_PinSource10 to 15 | EXTI_Line15_10 | EXTI15_10_IRQn | EXTI15_10_IRQHandler |

---

## Homework
```
release during your tutorial section via email
```