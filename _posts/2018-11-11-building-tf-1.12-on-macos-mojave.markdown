---
layout: post
title:  "How to build TensorFlow 1.12.0 on macOS Mojave"
date:   2018-11-11 22:00:00 +0900
categories: jekyll update
---

### Environment

- macOS Mojave

- TensorFlow 1.12.0 (https://github.com/tensorflow/tensorflow/archive/v1.12.0.tar.gz)

- Python 3.7.1


### How-to

- Edit _tensorflow/workspace.bzl_ so that TF uses protobuf 3.6.1:

```text
    PROTOBUF_URLS = [
        "https://mirror.bazel.build/github.com/google/protobuf/archive/v3.6.1.tar.gz",
        "https://github.com/google/protobuf/archive/v3.6.1.tar.gz",
    ]
    PROTOBUF_SHA256 = "3d4e589d81b2006ca603c1ab712c9715a76227293032d05b26fca603f90b3f5b"
    PROTOBUF_STRIP_PREFIX = "protobuf-3.6.1"
```


- Edit bazel BUILD files (tensorflow/BUILD, tensorflow/java/BUILD, tensorflow/contrib/lite/c/BUILD, tensorflow/contrib/lite/experimental/c/BUILD), so that the entries with "exported_symbols_list" beconmes one line.

    - Reference: https://github.com/tensorflow/tensorflow/issues/22759


- Start build


- After build failes, edit ptotobuf sources (descirptor.cc, extension_dict.cc, message.cc, descriptor_pool.cc, decriptor.cc) in _bazel-tensorflow-1.12.0/external/protobuf_archive/python/google/protobuf/pyext_:

    - Before:
    ```c++
    #define PyString_AsStringAndSize(ob, charpp, sizep) \
        (PyUnicode_Check(ob)? \
        ((*(charpp) = PyUnicode_AsUTF8AndSize(ob, (sizep))) == NULL? -1: 0): \
        PyBytes_AsStringAndSize(ob, (charpp), (sizep)))
    ```

    - After
    ```c++
    #define PyString_AsStringAndSize(ob, charpp, sizep) \
        (PyUnicode_Check(ob)? \
        ((*(charpp) = const_cast<char*>(PyUnicode_AsUTF8AndSize(ob, (sizep)))) == NULL? -1: 0): \
        PyBytes_AsStringAndSize(ob, (charpp), (sizep)))
    ```

    - Reference: https://github.com/protocolbuffers/protobuf/blob/master/python/google/protobuf/pyext/descriptor_containers.cc


- Resume build 


***

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
