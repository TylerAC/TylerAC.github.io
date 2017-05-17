---
layout: post
title: "Get started with tensorflow"
description: "this guide help you start program in tensorflow and know the structure of tensorflow"
date: 2017-05-17
tags: [tensorflow, python]
comments: true
share: true
---

this guide help you start program in tensorflow and know the structure of tensorflow

---

## 1. Importing tensorflow
tensorflow module includes all the class, methods and symbols which you will need when you config for your graph.
<pre><code>$ import tensorflow as tf
</code></pre>

and tensorflow give a lot of logs to you, maybe some is not in need for you, you could filter those by following:
<pre><code>import os
os.environ['TF_CPP_MIN_LOG_LEVEL']='2'
all the level is:
   0: all logs shown (that's the default setting)
   1: filter out INFO logs
   2: additionally filter out WARNING logs
   3: additionally filter out ERROR logs.
</code></pre>

## 2. The Computational Graph
All the TensorFlow programs consist of two: build the computational graph and run the computational graph

Before you build the computational graph, you must know that:
<pre><code>$ graph: used for calculating
Session: used for running the operation
tensor: represent data
variable: keeps the transaction state
feed: assign the value for operation
fetch: get the data from operation
</code></pre>

* create the tensors
<pre><code>node1 = tf.constant([[3., 3.]])
node2 = tf.constant([[2.],[2.]])
</code></pre>

* crate the operation
<pre><code>product = tf.matmul(node1, node2)
</code></pre>

* run the op(operation) in Session
You could by this:
<pre><code>sess = tf.Session()
print(sess.run(product))
then you need to close the Session
sess.close()
</code></pre>


but you'd better do by folowing:
<pre><code>with tf.Session() as sess:
    with tf.device("/gpu:0"):
       result = sess.run(product)
           print(result)
</code></pre>

This code will help you that when you operations are finished, the seesion will be closed automatically.
*with tf.device("/gpu:0")* will be optional, which help you choose which device to finish your task

* crate the variable
You could create the variable and update the its value by folowing:
<pre><code>state = tf.Variable(0, name = "hello")
one = tf.constant(1)
new_value = tf.add(state, one)
update = tf.assign(state, new_value)
</code></pre>


* fetch the data
Before you call the *run()* of *Session*, you could create the tensor to help you get the result
<pre><code>result = sess.run([mul2, mul1])
</code></pre>

will give you the result after finishing the operations of mul2 and mul1

* feed the data
You could use *tf.placeholder()* to init your variable, and the feed the value when your Session runs as following:
<pre><code>input1 = tf.placeholder(tf.float32)
input2 = tf.placeholder(tf.float32)
output = tf.multiply(input1, input2)

with *tf.Session()* as sess:
    #it will run wrong while feed_dict is not configed right
      print(sess.run([output], feed_dict = {input1:[7.], input2:[2.]}))
</code></pre>
