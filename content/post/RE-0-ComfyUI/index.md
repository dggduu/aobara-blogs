+++
author = "aobara"
title = "从零开始的comfyUI的使用"
description = ""
date = "2024-10-30"
categories = [
    "AI",
    "笔记",
]
image = "title.png"
+++
## 安装
https://github.com/comfyanonymous/ComfyUI/releases  
博主使用的是win，所以就直接使用整合包算了。  
## 使用模型
### 从WebUI迁移旧模型
找到extra_model_paths.yaml.example文件  
> ~\ComfyUI\extra_model_paths.yaml.example

找到这行：
```yaml
base_path: path/to/stable-diffusion-webui/
```
将path/to/stable-diffusion-webui/ 改为你的webui根目录；  
将文件名后面的example去掉使之以.yaml结尾。  
### 使用新的模型
扔入~\ComfyUI\models\checkpoints即可

## 配置
### 汉化
AIGODLIKE-ComfyUI-Translation  
https://github.com/AIGODLIKE/AIGODLIKE-COMFYUI-TRANSLATION  
~\ComfyUI\custom_nodes  
git clone https://github.com/AIGODLIKE/AIGODLIKE-COMFYUI-TRANSLATION.git  
### 插件管理ComfyUI-Manager
https://github.com/ltdrdata/ComfyUI-Manager  
git clone https://github.com/ltdrdata/ComfyUI-Manager.git  
### ComfyUI-Advanced-ControlNet
https://github.com/Kosinkadink/ComfyUI-Advanced-ControlNet  
git clone https://github.com/Kosinkadink/ComfyUI-Advanced-ControlNet.git  
模型在这下：  
https://huggingface.co/lllyasviel/ControlNet-v1-1/tree/main  
### 使用Embedding模型
git clone https://github.com/pythongosssss/ComfyUI-Custom-Scripts.git  
### 使用A1111风格提示词（smZNodes）
https://github.com/shiimizu/ComfyUI_smZNodes  
git clone https://github.com/shiimizu/ComfyUI_smZNodes.git  
### 使用IPAdapter
ComfyUI IPAdapter plus  
https://github.com/cubiq/ComfyUI_IPAdapter_plus  
git clone https://github.com/cubiq/ComfyUI_IPAdapter_plus.git  