---
layout: post
title:  "How to Build TensorFlow for Windows"
date:   2018-08-28 16:00:00 +0900
categories: jekyll update
---


#### Environemt
  - TensorFlow: ver. 1.10.0
    - https://github.com/tensorflow/tensorflow
    - https://github.com/tensorflow/tensorflow/archive/v1.10.0.tar.gz


  - Python: Python 3.6.5 (Miniconda3 ver. 4.5.4 / 64bit)
    - https://conda.io/miniconda.html
    - https://github.com/tensorflow/tensorflow/archive/v1.10.0.tar.gz


  - CMake: ver. 3.7.2
    - https://cmake.org/

  - Compiler: Visual Studio 2017 (15.4.5)

    - https://visualstudio.microsoft.com/
    - Note: Be careful of compatibility with CUDA NVCC if you are building with GPU support; Latest VS may be incompatilble with CUDA NVCC and cause compilation error.

  - SWIG: ver. 3.0.12

    - http://www.swig.org/

  - git: Portable Git ver. 2.16.2

    - https://git-scm.com/
    - https://git-scm.com/download/win

  - CUDA: ver 9.2.148

  - cuDNN: 7.2.1


***
#### Setup

- Open Visual Studio command Prompt (2017 / Win64).

- Start "Anaconda Prompt 3" from the VS command prompt.

- Set path:

  ```msdos
  set Path=<CMake folder>\bin;<SWIG folder>;<Portable git folder>\bin;%Path%
  ```

- Go to output directory.

- Modify CMakeLists.txt (GPU version only):

  - Insert appropriate CUDA compute capability into _arch=compute_XX,code=\"sm_XX,compute_XX\"_ and _-DTF_EXTRA_CUDA_CAPABILITIES=X.X_

  - Go to  to look up Compute Capability.

  ```text
  set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS};-gencode arch=compute_61,code=\"sm_61,compute_61\";)

  ...

  if (WIN32)
    add_definitions(-DGOOGLE_CUDA=1 -DTF_EXTRA_CUDA_CAPABILITIES=6.1)
  else (WIN32)
  ```


- Configuration:

  - GPU version:

    - Change "Visual Studio 15 2017 Win64" according to the compiler you are using.

  ```msdos
  cmake ^
  <TF source>\tensorflow\contrib\cmake ^
  -G"Visual Studio 15 2017 Win64" ^
  -DCMAKE_BUILD_TYPE=Release ^
  -Dtensorflow_DISABLE_EIGEN_FORCEINLINE=ON ^
  -Dtensorflow_BUILD_CC_TESTS=OFF ^
  -Dtensorflow_BUILD_MORE_PYTHON_TESTS=OFF ^
  -Dtensorflow_ENABLE_GPU=OFF
  ```

  - CPU-only version

    - Change "tensorflow_CUDA_VERSION=9.2" according to your CUDA environment.
    - "CUDA_HOST_COMPILER" must be specified when VS 2017 is used; otherwise compilation will not complete.

  ```
  cmake ^
  <TF source>\tensorflow\contrib\cmake ^
  -G"Visual Studio 15 2017 Win64" ^
  -DCUDA_HOST_COMPILER="C:/Program Files (x86)/Microsoft Visual Studio/2017/Community/VC/Tools/MSVC/14.11.25503/bin/Hostx64/x64" ^
  -DCMAKE_BUILD_TYPE=Release ^
  -Dtensorflow_DISABLE_EIGEN_FORCEINLINE=ON ^
  -Dtensorflow_BUILD_CC_TESTS=OFF ^
  -Dtensorflow_BUILD_MORE_PYTHON_TESTS=OFF ^
  -Dtensorflow_ENABLE_GPU=ON ^
  -Dtensorflow_CUDA_VERSION=9.2
  ```

- Apply patch:

  - Reference: 
    - https://github.com/tensorflow/tensorflow/issues/19198
    - http://eigen.tuxfamily.org/bz/attachment.cgi?id=834


- Build Python wheel:

  ```msdos
  cmake --build . --config Release --target tf_python_build_pip_package
  ```


***
#### Install

- GPU version:

  ```msdos
  pip install tensorflow_gpu-1.10.0-cp36-cp36m-win_amd64.whl
  ```

- CPU-only version:

  ```
  pip install tensorflow-1.10.0-cp36-cp36m-win_amd64.whl
  ```


***

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/