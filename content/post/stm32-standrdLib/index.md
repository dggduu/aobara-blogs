+++
author = "aobara"
title = "江科协stm32(标准库)学习笔记"
description = ""
date = "2025-04-19"
categories = [
    "stm32",
    "笔记",
]
image = ""
+++
# 创建工程
copy:
```c
\STM32F10x_StdPeriph_Lib_V3.5.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x\startup\arm  
\STM32F10x_StdPeriph_Lib_V3.5.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x
\STM32F10x_StdPeriph_Lib_V3.5.0\Libraries\CMSIS\CM3\CoreSupport
```
# GPIO输出（点灯）
```c
//使用到的函数
void GPIO_SetBits(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);//可以使用或运算来控制多个GPIO口
void GPIO_ResetBits(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
void GPIO_WriteBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin, BitAction BitVal);
void GPIO_Write(GPIO_TypeDef* GPIOx, uint16_t PortVal);
```
注意添加给的Delay.h,Delay.c文件  
## 示例工程
```c
#include "stm32f10x.h"                  // Device header
#include <Delay.h>
int main(void) {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);//GPIO口都占用APB2,所以要对APB2的RCC使能
    
    GPIO_InitTypeDef GPIO_InitStruct;
    GPIO_InitStruct.GPIO_Mode = GPIO_Mode_Out_PP;//推挽输出
    GPIO_InitStruct.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOA,&GPIO_InitStruct);
    GPIO_WriteBit(GPIOA,GPIO_Pin_0,Bit_SET);
    while(1) {
        GPIO_WriteBit(GPIOA,GPIO_Pin_0,Bit_SET);
        Delay_ms(500);
        GPIO_WriteBit(GPIOA,GPIO_Pin_0,Bit_RESET);
        Delay_ms(500);
    }
}
```
# GPIO 输入(按键控制)
```c
void Key_Init(void)
{
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);
    
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_10;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOB, &GPIO_InitStructure);
}

uint8_t Key_GetNum(void)
{
    uint8_t KeyNum = 0;
    if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_0) == 0)
    {
        Delay_ms(20);
        while (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_0) == 0);
        Delay_ms(20);
        KeyNum = 1;
    }
    if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_10) == 0)
    {
        Delay_ms(20);
        while (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_10) == 0);
        Delay_ms(20);
        KeyNum = 2;
    }
    
    return KeyNum;
}
```
# OLED 屏幕
导入oled.h,oled.c  
写到这里发现OLED商家漏发了qwq  
# 中断
## EXIT中断
触发方式：上升沿，下降沿，双边沿，软件触发  
注：相同的PIN不能同时触发中断（如GPIOA_Pin_0与GPIOB_Pin_0）  
## TIM 中断
### PWM
#### 占空比
高电平 5V,等效电平 = 5V * 钻占空比  
占空比越大，模拟电压越趋近于高电平  
## DMA中断
# I2C
# FLASH读写