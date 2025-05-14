+++
author = "aobara"
title = "ESP32 学习笔记( Arduino 表述 )"
description = ""
date = "2025-04-20"
categories = [
    "stm32",
    "笔记",
]
image = ""
+++
# PWM LED
## 1
```c
#define LED_PIN 12
void setup() {
  // put your setup code here, to run once:
    pinMode(LED_PIN,OUTPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  for(int i=0;i<256;i++) {
    analogWrite(LED_PIN,i);
    delay(10);
  }
  for(int i=255;i>=0;i--) {
    analogWrite(LED_PIN,i);
    delay(10);
  }
}

```
## 2
```c
#define LED_PIN 12
#define freq 2000
#define channel 0
#define resolution 8 //分辨率

void setup() {
  // put your setup code here, to run once:
    //pinMode(LED_PIN,OUTPUT);
    ledcSetup(channel,freq,resolution);
    ledcAttachPin(LED_PIN,channel);
}

void loop() {
  // put your main code here, to run repeatedly:
  for(int i=0;i<pow(2,resolution);i++) {
    ledcWrite(channel,i);
    delay(10);
  }
  for(int i=pow(2,resolution)-1;i>=0;i--) {
    ledcWrite(channel,i);
    delay(10);
  }
}

```
