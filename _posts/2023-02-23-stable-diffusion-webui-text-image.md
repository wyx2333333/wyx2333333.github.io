---
title: 初探 Stable Diffusion Webui - txt2img
date: 2023-02-23 10:38:15 +0800
categories: [AIGC]
tags: [AIGC, Stable Diffusion Webui]
---

## 下载模型文件 (C 站下载)

- chilloutmix_NiPrunedFp32Fix.safetensors (主模型)
- iu-v2-5.safetensors (sshs_model_hash: 0040bc33e5f4...)
- koreanDollLikeness_v15.safetensors (sshs_model_hash: 3c8b9d1e9b98...)
- yaeMikoRealistic_yaemikoMixed.safetensors (sshs_model_hash: 8eafb307ecf8...)

## Prompt (摘自 C 站)

`(RAW photo, best quality), (realistic, photo-realistic:1.3), best quality ,masterpiece, an extremely delicate and beautiful, extremely detailed ,CG ,unity ,8k wallpaper, Amazing, finely detail, masterpiece,best quality, extremely detailed CG unity 8k wallpaper,absurdres, incredibly absurdres, huge filesize , ultra-detailed, highres, extremely detailed, iu,asymmetrical bangs,short bangs,bangs,pureerosface_v1,beautiful detailed girl, extremely detailed eyes and face, beautiful detailed eyes,light on face,looking at viewer, straight-on, staring, closed mouth,black hair,short hair, (short_ponytail:1.1), collarbone, bare shoulders, longeyelashes, cum on body,lace ,lace trim`

## Negative prompt (摘自 C 站)

`EasyNegative, paintings, sketches, (worst quality:2), (low quality:2), (normal quality:2), lowres, normal quality, ((monochrome)), ((grayscale)), skin spots, acnes, skin blemishes, age spot, glans,extra fingers,fewer fingers,((watermark:2)),(white letters:1), (multi nipples), lowres, bad anatomy, bad hands, text, error, missing fingers,extra digit, fewer digits, cropped, worst quality, low qualitynormal quality, jpeg artifacts, signature, watermark, username,bad feet, {Multiple people},lowres,bad anatomy,bad hands, text, error, missing fingers,extra digit, fewer digits, cropped, worstquality, low quality, normal quality,jpegartifacts,signature, watermark, blurry,bad feet,cropped,poorly drawn hands,poorly drawn face,mutation,deformed,worst quality,low quality,normal quality,jpeg artifacts,signature,extra fingers,fewer digits,extra limbs,extra arms,extra legs,malformed limbs,fused fingers,too many fingers,long neck,cross-eyed,mutated hands,polar lowres,bad body,bad proportions,gross proportions,text,error,missing fingers,missing arms,missing legs,extra digit`

## LoRA 模型

点击`Generate`按钮下方红黑 icon 按钮，打开 extra networks 面板， 在 Lora 页签中选择下面模型

- `iu-v2-5.safetensors`: 权重设置 0.6 `<lora:iu-v2-5:0.6>`
- `koreanDollLikeness_v15.safetensors`: 权重设置 0.3 `<lora:koreanDollLikeness_v15:0.3>`
- `yaeMikoRealistic_yaemikoMixed.safetensors`: 权重设置 0.3 `<lora:YaeMiko_Test:0.3>`

> 点击 Lora 模型后 prompt 中就会显示类似`<lora:iu-v2-5:0.6>`的标签，冒号后面数字代表权重，修改权重为上面配置内容。

## 修改下方配置

- `Sampling method`: 选择`DPM++ SDE Karras`
- `Sampling steps`: 22
- 勾选`Hires.fix`
- `Upscaler`: 选择`Latend`
- `Hires steps`: 15
- `Width`: 784
- `Height`: 576
- `Seed`: 1267529735

## 修改 `Setting` - `Sampler parameters`

- `Eta noise seed delta`: 31337
- 点击上方`Apply settings`按钮

## 点击`Generate`生成图片

![](/assets/img/webui/txt2img_demo.png)
