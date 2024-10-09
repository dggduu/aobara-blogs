+++
author = "aobara"
title = "如何通过 Diffsuer 来运行 StableDiffusion"
date = "2024-09-07"
description = "我将在此文中演示如何用 Diffsuer 来运行SD"

categories = [
    "笔记",
    "AI",
]

image = "title.jpg"
+++
## 使用 Diffsuer 的原因
- 没钱买frp服务(指Gradio,ngrok这种可以用Python调用的)
- 4060 8GB容易爆显存。就我自己而言，速度不重要，显存很重要，因为显存决定了你能不能跑更大参数的模型或是更复杂的任务。（可惜硬要本地部署的话只能上台式😫，当然富哥也可以用4080 Laptop 12GB）
---
## 什么是 Diffsuer
> 🤗 Diffusers is the go-to library for state-of-the-art pretrained diffusion models for generating images, audio, and even 3D structures of molecules.
Whether you’re looking for a simple inference solution or want to train your own diffusion model, 🤗 Diffusers is a modular toolbox that supports both. Our library is designed with a focus on usability over performance, simple over easy, and customizability over abstractions.  
*-HuggingFace[^1]*  

[^1]: [ diffsuers 文档](https://huggingface.co/docs/diffusers/index)

## 进入正题
1. 加载必要库
```zsh
!pip install diffusers["torch"] transformers
!pip install accelerate
!pip install git+https://github.com/huggingface/diffusers
```
2. 加载模型与Scheduler
```python3
import torch
from diffusers import StableDiffusionPipeline
from diffusers import DPMSolverMultistepScheduler

pipe = StableDiffusionPipeline.from_pretrained("User/modelName", 
    torch_dtype=torch.float16)

pipe = pipe.to("cuda")
pipe.safety_checker = None
pipe.scheduler = DPMSolverMultistepScheduler.from_config(pipe.scheduler.config)

```
3.加载 prompt , negtive prompt 以及其他参数
```python3
prompt = "1girl,school,delicated eyes"
h=800
w=640
steps=25
guidance=7.5
lora_weight=0
num_images=3
neg = "unreal body,NSFW"

images = pipe(prompt, num_images_per_prompt=num_images, cross_attention_kwargs={"scale": lora_weight}, height=h, width=w, num_inference_steps=steps, guidance_scale=guidance, negative_prompt=neg).images
for i in range(num_images):
  display(images[i])
  ```
至此我们完成了初步构建，你可以开始玩你的stableDiffusiion了。如果你想进阶的话，你可以访问diffsuer的文档[^1] 为其添加更多功能 

  ---
  ## 为其添加一些功能
  1. 添加safetensors模型支持
  >safetensors is a safe and fast file format for storing and loading tensors. Typically, PyTorch model weights are saved or pickled into a .bin file with Python’s pickle utility. However, pickle is not secure and pickled files may contain malicious code that can be executed. safetensors is a secure alternative to pickle, making it ideal for sharing model weights.  
  >*-HuggingFace*

    在加载模型的函数处后面加上 `use_safetensors=True` 参数即可。  
    例子：

```python3
pipe = StableDiffusionPipeline.from_pretrained("User/modelName", 
    torch_dtype=torch.float16,
    use_safetensors=True)
```
---
2. 更改 Scheduler (以经典的Euler为例)
    1. 导入 Euler
    ```python3
    from diffusers import EulerDiscreteScheduler
    ```
    2. 加载 Euler
    ```python3
    pipe.scheduler = EulerDiscreteScheduler.from_config(pipe.scheduler.config)
    ```
    进阶的话请访问:(你也可以在此文档中找到其他调度器的配置示例)  
    https://huggingface.co/docs/diffusers/main/en/api/schedulers/euler#diffusers.EulerDiscreteScheduler  
    **！！！注意注释掉原来的！！！**

3. 使用 StableDiffusionXL 模型(注意别爆显存了)
    ```python3
    pipe = StableDiffusionXLPipeline.from_pretrained("User/modelName", 
        torch_dtype=torch.float16, 
        variant="fp16", 
        use_safetensors=True)
    ```
    使用单个文件
    ```python3
    pipe = StableDiffusionXLPipeline.from_single_file(
        "https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/blob/main/sd_xl_base_1.0.safetensors", 
        torch_dtype=torch.float16
    )
    ```
4. 图生图？inpaint? ControlNet?  
    没错，diffusers 支持使用图生图，inpaint ，ControlNet 。它们都是想进阶 StableDiffusion 技术的必经之路，但是它们需要你手动绘制遮罩，你也不想通过 GDrive 上传遮罩吧。。。所以请自行查阅文档。
5. 使用 LORA ：  
    ```python3
    pipe.load_lora_weights("User/modelName", 
        weight_name="model.safetensors", 
        adapter_name="你想一个名称")
    ```
     - 激活 LORA :  
     ```python3
     pipe.set_adapters("adapter_name")
     ```
     **!记得在你的PROMPT里添加有关触发词**  
     - 使用多个 LORA :
     ```python3
     pipe.set_adapters(["pixel", "toy"], 
        adapter_weights=[0.5, 1.0])
    ```
    - 禁用所有 LORA :
     ```python3
      disable_lora()
    ```