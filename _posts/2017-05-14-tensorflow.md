---
layout: post
title: "Install tensorflow on Mac"
description: "this one is to share how to install tensorflow in mac with cpu and gpu version, and get started with python3"
date: 2017-05-14
tags: [tensorflow, python]
comments: true
share: true
---

this one is to share how to install tensorflow in mac with cpu and gpu version, and get started with python3

---

## 1. preparation for installing tensorflow with python3
Of course you could choose python2 or other version, offical demo is provide with python3, so python3 version will help you get ride of some troubles.
* [Homebrew](https://brew.sh/) is wanted for installing python.
  <pre><code>brew install python
  </code></pre>

* install pip
  <pre><code>$sudo easy_install pip
  $sudo easy_install --upgrade six
  </code></pre>

* install tensorflow
  <pre><code>pip install tensorflow
  </code></pre>

* update the version with you expect
  <pre><code>Mac OS X, CPU only, Python 2.7:
  $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.0.0-py2-none-any.whl
  Mac OS X, GPU enabled, Python 2.7:
  $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/gpu/tensorflow_gpu-1.0.0-py2-none-any.whl
  Mac OS X, CPU only, Python 3.4 or 3.5:
  $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.0.0-py3-none-any.whl
  Mac OS X, GPU enabled, Python 3.4 or 3.5:
  $ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/gpu/tensorflow_gpu-1.0.0-py3-none-any.whl
  find more versions in [tensorflow](https://www.tensorflow.org/install)

  then update TensorFlow:
  Python 2
  $ sudo pip install --upgrade $TF_BINARY_URL
  Python 3
  $ sudo pip3 install --upgrade $TF_BINARY_URL
  </code></pre>

* if you need to meet the GPU, you still get the [CUDA](https://developer.nvidia.com/cuda-toolkit-70) sdk and [CUDNN](https://developer.nvidia.com/rdp/cudnn-archive), and keep the drivers work

## 2. installing tensorflow in virtualenv
* tensorflow is suggested to installed in virtualenv so we could keep the configuration for tensorflow in one sandbox, including var environment path etc.
  <pre><code>$ sudo easy_install pip  
  $ sudo pip install --upgrade virtualenv
  </code></pre>

* create virtualenv environment with new folder
  <pre><code>$ virtualenv --system-site-packages ~/tensorflow
  $ cd ~/tensorflow
  </code></pre>

* activate virtualenv
  <pre><code>$ source bin/activate  # if use bash/zsh
  $ source bin/activate.csh  # if use csh
  </code></pre>

* install tensorflow
  <pre><code>(tensorflow) tyler$ pip install --upgrade tensorflow-0.5.0-py2-none-any.whl
  </code></pre>
  then you could run your case in virtualenv

## 3. offical demo
Python code
  <pre><code>import os
  os.environ['TF_CPP_MIN_LOG_LEVEL']='2'
  import tensorflow as tf
  hello = tf.constant('Hello, TensorFlow!')
  sess = tf.Session()
  print(sess.run(hello))
  print(tf.__version__)
  </code></pre>

then run the script and get:
![Alt text](/images/ss20170514.png)
