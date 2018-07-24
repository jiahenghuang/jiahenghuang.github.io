---
title: python tensorflow-gpu 环境安装
date: 2018-07-24 19:56:26
tags: [tensorflow, python, gpu]
categories: tensorflow
---

- cuda环境变量（https://blog.csdn.net/fnhc462354756/article/details/80041021）：

```shell
export PATH=/usr/local/cuda-9.0/bin:$PATH  
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64:$LD_LIBRARY_PATH
export CUDA_HOME=/usr/local/cuda

$ sudo source ~/.bashrc  
$ sudo ldconfig
```

- cudnn安装：

  ```shell
  tar -zxvf cudnn-9.0-linux-x64-v7
  sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
  sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
  sudo chmod a+r /usr/local/cuda/include/cudnn.h
  sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
  ```

- tensorflow-gpu

  ```shell
  #下载tensorflow_gpu-1.8.0-cp27-cp27mu-manylinux1_x86_64.whl
  $ pip install tensorflow_gpu-1.8.0-cp27-cp27mu-manylinux1_x86_64.whl
  ```

注：监控NVIDIA gpu使用情况

```shell
 #https://blog.csdn.net/jasonzzj/article/details/52649174
 #nvidia-smi 命令解读:https://blog.csdn.net/sallyxyl1993/article/details/62220424
 watch -n 10 nvidia-smi
```

