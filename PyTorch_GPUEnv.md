---
title: pytorch+gpu 环境配置
date: 2023-03-04 15:23:34
tags: 
	- pytorch
	- gpu
categories: 配置教程
cover:  https://w.wallhaven.cc/full/p9/wallhaven-p9o51m.png
---
<!-- more -->   
## 一、安装步骤

> 1. Anaconda与Pycharm安装
> 2. Cuda安装
> 3. Pytorch安装

逻辑关系整理：

![image-20230219123104058](https://gitee.com/juzihhu/image_bed/raw/master/img/202302222030590.png)

- Anaconda与Pycharm安装官网下载即可；

- cuda安装前需要查看显卡支持的最大版本：win+R打开cmd，输入nvidia-smi；可以知道当前支持的最大cuda版本为11.6

  ![image-20230219150826062](https://gitee.com/juzihhu/image_bed/raw/master/img/202302222032470.png)

- Pytorch安装：简单理解为下载pytorch的时候其实是下载三个python第三方库一个叫torch、一个叫torchvision

  

## 二、 详细安装

1. Anaconda与Pycharm安装：

   **Anacond下载**：https://www.anaconda.com/download/

2. Cuda安装：https://developer.nvidia.com/cuda-toolkit-archive

   安装完成验证：nvcc -V

   ![image-20230219152002352](https://gitee.com/juzihhu/image_bed/raw/master/img/202302222032885.png)

3. Pytorch安装：

   

```python
# 在指定目录下创建名为mmlab的环境并进入
conda activate
conda create --prefix=D:\envs\mmlab python=3.7
conda activate D:\envs\mmlab

conda create --prefix=D:\envs\taichi_0.8.0 python=3.7
#删除环境
#conda remove --prefix=D:\envs\taichi_0.8.0 --all
```

CUDA Toolkit 和PyTorch的对应关系

![img](https://gitee.com/juzihhu/image_bed/raw/master/img/202302222033188.png)

**[torch](https://so.csdn.net/so/search?q=torch&spm=1001.2101.3001.7020) - torchvision - python 版本对应关系**

![20210630110512453](https://gitee.com/juzihhu/image_bed/raw/master/img/202302222033189.png)

**在线安装：**https://pytorch.org/get-started/previous-versions/  （官网命令可能速度慢）

**下载 .whl 文件离线安装**：https://download.pytorch.org/whl/torch_stable.html

**举例：torch1.8.0+torchvision0.9.0+python3.7**

进入mmlab环境输入：

> pip install D:\FirefoxDownload\torch-1.8.0+cu111-cp37-cp37m-win_amd64.whl
>
> pip install D:\FirefoxDownload\torchvision-0.9.0+cu111-cp37-cp37m-win_amd64.whl

**D:\FirefoxDownload**为.whl文件路径

注意：cu表示为安装GPU版本



验证：

```python
#在安装环境之前，我的建议是先查看一下目前自己的环境
#打开cmd或者linux终端
nvcc -V #查看之前是否装过cuda 之前装过也不用卸载，因为不同版本之间可以通过修改环境变量更换。
python
>>> import torch
>>> torch.__version__ #查看目前安装的pytorch版本
>>> torch.cuda.is_available() #查看是否可以使用cuda加速，只有有cuda且与pytorch兼容的情况才是True
# 如果返回True说明兼容
```



## 三、镜像源添加

```python
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge 
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --set show_channel_urls yes

conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/
conda config --set show_channel_urls yes
```





参考博客：

[Pytorch(GPU)配环境原理：cuda+cudnn+pytorch配环境的每一步到底干了些什么？](https://blog.csdn.net/cyl_csdn_1/article/details/124721494)

[Pytorch最全安装教程](https://blog.csdn.net/weixin_44904136/article/details/123285884)

[GPU版本安装Pytorch教程最新方法](https://blog.csdn.net/qq_45956730/article/details/126600028)

[Torch 、torchvision 、Python 版本对应关系以及安装 GPU 或 CPU 版本的 pytorch](https://blog.csdn.net/qq_40630902/article/details/118356845)