---
layout: post
title:  "Compiling TensorFlow from source (Windows ver.)"
date:   2019-02-28 18:00:00 +0900
categories: jekyll update
---

## Environment

- Windows 10
- TensorFlow: v1.13.1
- Bazel: v0.20
  - v0.21 does not work?
- Python: 3.6.5 (Miniconda)
- CUDA 10.0

## Configuration

```msdos
> python configure.py
```

## Build

- With CUDA

```msdos
:: Create  Python package
> bazel --output_user_root=D:\nn\tf\_bazel_cache build ^
--config=opt --config=cuda --define=no_tensorflow_py_deps=true ^
--copt=-nvcc_options=disable-warnings ^
//tensorflow/tools/pip_package:build_pip_package

> bazel-bin\tensorflow\tools\pip_package\build_pip_package C:/tmp/tensorflow_pkg

:: Create C/C++ library
> bazel --output_user_root=D:\nn\tf\_bazel_cache build ^
--config=opt --config=cuda --define=no_tensorflow_py_deps=true ^
--copt=-nvcc_options=disable-warnings ^
//tensorflow:libtensorflow.so

> bazel --output_user_root=D:\nn\tf\_bazel_cache build ^
--config=opt --config=cuda --define=no_tensorflow_py_deps=true ^
--copt=-nvcc_options=disable-warnings ^
//tensorflow:libtensorflow_cc.so

:: End
> bazel shutdown
```

- Without CUDA

```msdos
:: Create  Python package
> bazel --output_user_root=D:\nn\tf\_bazel_cache build ^
--config=opt //tensorflow/tools/pip_package:build_pip_package

> bazel-bin\tensorflow\tools\pip_package\build_pip_package C:/tmp/tensorflow_pkg

:: Create C/C++ library
> bazel --output_user_root=D:\nn\tf\_bazel_cache build ^
--config=opt //tensorflow:libtensorflow.so

> bazel --output_user_root=D:\nn\tf\_bazel_cache build ^
--config=opt //tensorflow:libtensorflow_cc.so

:: End
> bazel shutdown
```

***

## Reference

- [https://www.tensorflow.org/install/source_windows](https://www.tensorflow.org/install/source_windows)

***

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
