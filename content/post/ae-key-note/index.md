+++
author = "aobara"
title = "AE 笔记"
date = "2025-04-26"
categories = [
    "视频",
    "笔记",
]
+++
# 效果
## 待分类
扭曲-极坐标  
通道-CC Compose  
CC Toner (复古胶片)  
动态拼贴（移动图片但缺失部分用图片其他部分填充）  
光学补偿（调节 FOV 做镜面效果的）
bpm control (由bpm 控制的变换)
## 丰富画面细节
百叶窗->条纹
repeater (重复)
## 字体相关
添加文字背景（简单阻塞工具）  
逐字动画 动画-不透明度-范围选择器  
# 形状相关
波形变形 - 做分割的小长方形  
# 表达式
## 文字摆动旋转
```js
amp = 20; // 摆动幅度为20度，这样总摆动范围就是-20到+20度
freq = 0.5; // 频率，控制摆动速度
timeOffset = 0; // 时间偏移量，可以用来延迟动画开始的时间

// 基础旋转角度是0度，因为我们是在-20到+20度之间摆动
baseAngle = 0;

// 使用Math.sin函数根据时间生成正弦波，乘以幅度和频率，并加上基础角度
baseAngle + amp * Math.sin((time + timeOffset) * freq * Math.PI * 2)
```
or 
```js
wiggle(Time,range[2*?];
```