---
layout: post
title: "Train the model with MNIST data "
description: "this guide help you know how to train a model with MNIST data"
date: 2017-06-05
tags: [tensorflow, python, MNIST]
comments: true
share: true
---

this guide help you know how to train a model with MNIST data

---

## 1.MNIST data
The MNIST data is hosted on Yann LeCun's website, you could download with the input_data.py as following:
<pre><code>from tensorflow.examples.tutorials.mnist import input_data
mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)
</code></pre>

so the data collections will be put in MNIST_data folder in your project.

The MNIST data is split into three parts: 55,000 data points of training data (mnist.train), 10,000 points of test data (mnist.test), and 5,000 points of validation data (mnist.validation). Every MNIST data point has two parts: the training images are mnist.train.images and the training labels are mnist.train.labels.


## 2. show the data with Struct
struct framework from python could help us show the data.

* read the data
  <pre><code>def readfile():
    # os.path.abspath('.') get the current filepath
    with open(os.path.abspath('.')  + '/MNIST_data/train-images-idx3-ubyte','rb') as f1:
        buf1 = f1.read()
    with open(os.path.abspath('.') + '/MNIST_data/train-labels-idx1-ubyte','rb') as f2:
        buf2 = f2.read()
    return buf1, buf2
  </code></pre>

* get the images
  <pre><code>def get_image(buf1):
    image_index = 0
    image_index += struct.calcsize('>IIII')
    im = []
    for i in range(9):
        temp = struct.unpack_from('>784B', buf1, image_index)
        im.append(np.reshape(temp,(28,28)))
        image_index += struct.calcsize('>784B')  
    return im
  </code></pre>

* get the labels
  <pre><code>def get_label(buf2):
    label_index = 0
    label_index += struct.calcsize('>II')
    return struct.unpack_from('>9B', buf2, label_index)
  </code></pre>

* show the image and labels
<pre><code>def showImage():
    image_data, label_data = readfile()
    im = get_image(image_data)
    label = get_label(label_data)

    for i in range(9):
        figure = plt.subplot(3, 3, i + 1)
        title = str(label[i])
        plt.title(title)
        plt.imshow(im[i], cmap='gray')

        #hide x.y ticks
        figure.set_xticks([])
        figure.set_yticks([])

    plt.show()
</code></pre>
As following:
![Alt text](/images/ss2017060501.png)

## 3. Train the model with MNIST data
[Cross-entropy](http://colah.github.io/posts/2015-09-Visual-Information/) represents how far off our model is from our desired outcome. Training our model by reduce the offset of Cross-entropy to make our model come close to outcome. that's training.
<pre><code>def showAccuracy():
    mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)
    x = tf.placeholder(tf.float32, [None, 784])
    W = tf.Variable(tf.zeros([784, 10]))
    b = tf.Variable(tf.zeros([10]))

    y = tf.nn.softmax(tf.matmul(x, W) + b)
    y_ = tf.placeholder(tf.float32, [None, 10])

    cross_entropy = tf.reduce_mean(-tf.reduce_sum(y_ * tf.log(y), reduction_indices=[1]))
    train_step = tf.train.GradientDescentOptimizer(0.5).minimize(cross_entropy)

    sess = tf.InteractiveSession()
    tf.global_variables_initializer().run()

    for _ in range(1000):
      batch_xs, batch_ys = mnist.train.next_batch(100)
      sess.run(train_step, feed_dict={x: batch_xs, y_: batch_ys})

    correct_prediction = tf.equal(tf.argmax(y,1), tf.argmax(y_,1))
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    print(sess.run(accuracy, feed_dict={x: mnist.test.images, y_: mnist.test.labels}))
</code></pre>
finally check the real the data with trained model to get the accuracy, it will be different every time while you choosed the trained data instead of all.
as following:
![Alt text](/images/ss2017060502.png)
![Alt text](/images/ss2017060503.png)
