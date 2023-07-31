---
title: Mac部署 Stable Diffusion Webui
date: 2023-02-21 16:52:28 +0800
categories: [AIGC]
tags: [AIGC]
---

## 安装 anaconda

### 下载安装包

官网下载安装，[下载地址](https://www.anaconda.com/download)

### 修改配置

添加环境变量

```bash
vim ~/.bash_profile
# or
vim ~/.zshrc

# 添加下面配置内容

# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('$HOME/anaconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "$HOME/anaconda3/etc/profile.d/conda.sh" ]; then
        . "$HOME/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="$HOME/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<

# 刷新配置文件
source ~/.bash_profile
# or
source ~/.zshrc
```

修改 conda 源地址

```bash
vim ~/.condarc

# 打开文件后，把下面的内容粘贴进去保存
channels:
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - defaults

```

## 创建 python 虚拟环境

```bash
conda create -n webui python=3.10
```

## 安装依赖

```bash
# 下载项目
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
# or
# 前往 https://github.com/AUTOMATIC1111/stable-diffusion-webui 点击Code下面的Download ZIP下载解压

# 进入项目目录
cd stable-diffusion-webui

# 进入创建的虚拟环境
conda activate webui

# 安装依赖包 (失败可能需要科学工具做终端代理)
pip3 install -r requirements_versions.txt
```

## 启动项目

### 修改 webui-user.sh 文件

{: file='webui-user.sh'}

```bash
# Commandline arguments for webui.py, for example: export COMMANDLINE_ARGS="--medvram --opt-split-attention"
# export COMMANDLINE_ARGS="--skip-torch-cuda-test --no-half --use-cpu all"
export COMMANDLINE_ARGS="--medvram --opt-split-attention --skip-torch-cuda-test --no-half"
```

### 启动项目

```bash
./webui.sh
```

> 如果仍然报错 install gfpgan 失败，注释/modules/launch_utils.py 文件 319 和 320 行代码 (前提是前面安装依赖包全部安装成功)，然后重新执行`./webui.sh`

{: file='/modules/launch_utils.py'}

```python
    # if not is_installed("gfpgan"):
    #     run_pip(f"install {gfpgan_package}", "gfpgan")
```

## 下载模型文件

[模型下载地址](/posts/websites/)

### Checkpoint 模型

- SD 绘图的基础模型 (大模型/底模型/主模型)，不同的主模型画风不同，生成的图片耶完全不同。
- 通常为 ckpt、safetensors 后缀，文件较大，为 2-7G。
- 存放位置: models/Stable-diffusion

### LoRA 模型

- 微调模型，必须与主模型一起使用。
- 通常为 ckpt、safetensors、pt 后缀，文件较小，为 10-300M 不等。
- 存放位置: models/Lora

### VAE 模型

- 美化模型/变分自编码器，用于滤镜和微调，有的主模型自带 VAE。
- 通常为 ckpt、pt 后缀。
- 存放位置: models/VAE

不确定模型文件存放位置的可以使用[工具](https://spell.novelai.dev/)
