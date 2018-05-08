---
layout: post
title:  "How to build pytorch/glow"
date:   2018-05-08 07:41:22 +0000
categories: jekyll update
---

#### 0. Environment
  - Ubuntu 16.04
  - LLVM 5.0.1


#### 1. Get source
  - Get glow's source code

  ```bash
  $ git clone --recursive https://github.com/pytorch/glow.git
  ```


#### 2. Get LLVM
  - Download from https://llvm.org/ & extract the .tar.xz

  - Extract the downloaded archive:

  ```bash
  $ tar xf clang+llvm-5.0.1-x86_64-linux-gnu-ubuntu-16.04.tar.xz
  ```


#### 3. Configure
  ```bash
  $ cd glow
  $ mkdir _build
  $ cd _build
  $ PATH=<Path to extracted LLVM>/bin:${PATH} \
  cmake .. \
  -G"Unix Makefiles" \
  -DCMAKE_INSTALL_PREFIX=`pwd`/_install \
  -DGLOW_WITH_OPENCL=OFF \
  -DCMAKE_BUILD_TYPE=Release \
  ```

  - Open _lib/Backends/CPU/CMakeFiles/CPURuntime.dir/link.txt_ and delete trailing part after "#".


#### 4. Build

  ```bash
  $ make -j`nproc`
  $ make install
  ```


#### 5. Run MNIST example

  ```bash
  $ cd ..
  $ python utils/download_test_db.py --all
  $ _build/bin/mnist
  ```

***

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
