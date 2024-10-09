+++
author = "aobara"
title = "笔电续航提升研究笔记"
date = "2024-07-21"
categories = [
    "高级电源设置",
    "笔记",
]

+++
## 动机
本是想换电脑的，但是13，14代intel cpu问题又打消了我的念头。在牢不可破的wintel联盟下我又怎么会去选amd呢（还有积热问题）  
30系显卡容易买到矿卡，40系又溢价。8GB显存又能干啥。  
最后没买，用上了旧电脑。这台电脑有两个问题：
- 在低负载情况下风扇狂叫
- 续航短，大约只有2小时不到（48Whr）
因此开始折腾起这台电脑

## 过程
### 重装系统
对我电脑来说，联想自带的软件其实都不重要，加上这电脑有很多东西很难删干净，索性重装系统。
为了能使用WSL,我选择了 Win10 LTSC2021。  
但是重装完系统后有意思的事来了，上面的两个问题仍然出现...😂
打开电源设置一看，谁打开的性能模式！而且我省电模式呢！！！


### 高级电源设置
为了进一步提升续航，我开始研究起高级电源设置。
#### 启用高级电源设置
1. 更改注册表
```shell-script
 powercfg -attributes SUB_PROCESSOR CPMINCORES -ATTRIB_HIDE
 ```
2. 使用专门的软件  
Windows power plan settings explorer utility[^1]

[^1]:https://forums.guru3d.com/threads/windows-power-plan-settings-explorer-utility.416058/

### 设置
！！！请先新建一个电源计划！！！

---
**鄙人不才，论权威请参阅MSDN:  
https://learn.microsoft.com/zh-cn/windows-hardware/customize/power-settings/configure-power-settings** 


设备空闲策略：节能
#### 硬盘
AHCI Link Power Management - HIPM/DIPM[^2]（a.k.a. LPM）：视你情况（我是lowest）
| Setting    | Description 描述                                                                                     |
|------------|-----------------------------------------------------------------------------------------------------|
| Active  | Neither HIPM or DIPM allowed. Link power management is not used. <br> HIPM或DIPM均不允许。不使用链路电源管理。 |
| HIPM       | HIPM (Host Initiated Link Power Management) only is allowed <br> 仅允许HIPM（主机启动的链路电源管理）             |
| HIPM + DIPM | HIPM and DIPM are allowed <br> 允许使用HIPM和DIPM                                                   |
| DIPM       | DIPM (Device Initiated Link Power Management) only is allowed <br> 仅允许DIPM（设备启动链路电源管理）             |
| Lowest  | HIPM, DIPM, and DEVSLP (if DEVSLP is supported by the storage device) are allowed. <br> 允许使用HIPM、DIPM和DEVSLP（如果存储设备支持DEVSLP）。 |

NVMe NOPPME：关闭  
>它的用途是在ssd休眠时允许ssd使用更多功耗完成一些像碎片整理的后台任务。[^3] 

#### 睡眠
1. 允许离开模式策略  
    1. 什么是离开模式策略：离开电脑后，电脑并不会完全进入睡眠，而是继续执行任务
    2. 这个看你，我是关上了
2. 允许使用系统所需的策略：否
3. 允许使用唤醒定时器：禁用

#### intel显卡设置
最长电源寿命
#### PCI Express
链接状态电源管理：最大电源节省量  
#### 处理器电源管理[^4] [^5]
处理器性能降低策略：rocket  
处理器性能提升策略 (睿频):单一的、理想的、IdealAggressive、Rocket(保守->激进，省电的话别选rocket)   
处理器性能提高阈值：数字越大处理器升频越懒惰，数字越小处理器升频越积极  
处理器性能降低阈值：数字越大处理器降频越积极，数字越小处理器降频越懒惰  
处理器能源性能首选项策略:数字越小处理器越优先考虑提高性能，数字越大处理器越优先考虑降低功耗  

允许节流状态：启用  
处理器闲置禁用：启用  
停用已停用性能状态的处理器性能核心：最深性能状态  
针对第 1 类处理器电源效率停用已停用性能状态的处理器性能核心：最深性能状态  
处理器性能核心放置减小策略：所有可能的核心  
处理器闲置升级阈值：0  
处理器性能核心放置最小核心数量：100  
处理器最大频率：看你情况  
最小处理器状态和最大处理器状态：(此项设置将处理器的最高默频作为100%)  
例子：
>- If your server requires ultra-low latency, invariant CPU frequency (e.g., for repeatable testing), or the highest performance levels, you might not want the processors switching to lower-performance states.
>For such a server, you can cap the minimum processor performance state at 100 percent
>- If your server requires lower energy consumption, you might want to cap the processor performance state at a percentage of maximum. For example, you can restrict the processor to 75 percent of its maximum frequency   
>*-MSDN [^4]*

处理器性能自主模式：已启用  
系统散热方式：
| 设置 | 描述 |
|------|------|
| 主动 | 当温度过高时优先提高风扇转速 |
| 被动 | 当温度过高时优先降低处理器性能 |

**进阶请参阅参考文档**[^3] [^4] [^5] 
#### 图形设置
gpu首选项策略：低功耗  
#### 显示
高级色彩质量偏好：高级色彩节电偏好（眼睛对色彩不敏感关了算了）
#### 节能模式设置
显示器亮度权重：都为100%（就是关闭“开启省电模式后会降亮度”的策略，本来显示器就没多少nits,再降低就真看不清了）  
节能模式策略：主动

[^2]:https://www.tenforums.com/tutorials/72971-add-ahci-link-power-management-power-options-windows.html
[^3]:  [自用的WIndows电源设置（踩坑后的修订版）](https://www.bilibili.com/read/cv18942103/)
[^4]:  [Windows server 电源和性能优化 -MSDN](https://learn.microsoft.com/en-us/windows-server/administration/performance-tuning/hardware/power/power-performance-tuning)
[^5]:  [Windows电源选项注释及设置建议——手搓OS Turbo](https://www.bilibili.com/read/cv33821884/#:~:text=%E5%A4%84%E7%90%86%E5%99%A8%E6%80%A7%E8%83%BD%E8%B0%83%E6%8E%A7%E7%AD%96%E7%95%A5)