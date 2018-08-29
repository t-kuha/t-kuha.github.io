---
layout: post
title:  "How to Build PyTorch with Caffe2 for Windows"
date:   2018-08-29 16:00:00 +0900
categories: jekyll update
---


### Environemt
  - Pytorch: ver. 0.4.1
    - [https://github.com/pytorch/pytorch](https://github.com/pytorch/pytorch)

  - Python: Python 3.6.5 (Miniconda3 ver. 4.5.4 / 64bit)
    - [https://conda.io/miniconda.html](https://conda.io/miniconda.html)
    - https://github.com/tensorflow/tensorflow/archive/v1.10.0.tar.gz

  - CMake: ver. 3.7.2
    - [https://cmake.org/](https://cmake.org/)

  - Compiler: Visual Studio 2017 (15.4.5)
    - [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com/)
    - Note: Be careful of compatibility with CUDA NVCC if you are building with GPU support; Latest VS may be incompatilble with CUDA NVCC and cause compilation error.

  - git: Portable Git ver. 2.16.2
    - [https://git-scm.com/](https://git-scm.com/)
    - https://git-scm.com/download/win

  - CUDA: ver 9.2.148

  - cuDNN: 7.2.1


***
### Setup

- Open Visual Studio command Prompt (2017 / Win64).

- Start "Anaconda Prompt 3" from the VS command prompt.

- Set path:

  ```msdos
  set Path=<CMake folder>\bin;%Path%
  ```

- Go to PyTorch source folder.

- Set environment variable
  - Change _NO_CUDA_ to 1 if GPU version is not needed.
  - Set appropriate CUDA compute capability to  _TORCH_CUDA_ARCH_LIST_. (Go to [https://developer.nvidia.com/cuda-gpus](https://developer.nvidia.com/cuda-gpus) to look up Compute Capability.)

  ```msdos
  set NO_CUDA=0
  set DISTUTILS_USE_SDK=1
  set CMAKE_GENERATOR=Visual Studio 15 2017 Win64
  set TORCH_CUDA_ARCH_LIST=6.1
  set FULL_CAFFE2=1 
  ```




***

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/