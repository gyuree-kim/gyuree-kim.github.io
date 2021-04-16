---
title: "[deep learning] Number image classification with adam optimizer and dropout"
date: 2021-04-16 15:30
categories: deep-learning
---
# Image classification with adam optimizer and dropout: Is really adam opt and dropout good for performace?

## Objective 
This is all about modeling to figure out how `dropout` and `adam optimizer` affect to the performace of models compared to `gradient descendent optimizer`.

## Overall structure
It has one input, two hidden, and one output layers.

## Code description
This is basicly to classify number images into zero to nine. Each input image data is represented as 784 pixels from 28 pixels of row and column, respectively. First hidden layer will simplify those into 256 pixels, and the output will figure out the result among 0 to 9.


### Import libraries and data
```python
import tensorflow as tf
from tensorflow.examples.tutorials.mnist
import input_data
```

### Placeholders
```python
mnist = input_data.read_data_sets("./mnist/data/"  one_hot=True)

X = tf.placeholder(tf.float32, [None,784])
Y = tf.placeholder(tf.float32, [None,10])
keep_prob = tf.placeholder(tf.float32)
```
`keep_prob` is declared to implement and manage `dropout` so that it reduces `overfitting`. This machanism is based on the probability, choosing nodes randomly following the value assigned to `keep_prob` variable.

### First hidden layer
```python
# hidden layer1
W1 = tf.Variable(tf.random_uniform([784, 256], -1., 1.))
b1 = tf.Variable(tf.random_uniform([256], -1., 1.))
L1 = tf.sigmoid(tf.matmul(X,W1) + b1)
L1 = tf.nn.dropout(L1, keep_prob)

# to make warning message disappear
old_v = tf.logging.get_verbosity()
tf.logging.set_verbosity(tf.logging.ERROR)
```
We need a `weight` W1 and a bais `b1` to move to first hidden layer. `Hidden layers` are the layers beween input and output layer and it can be a single layer. If the number of these are getting high, it means the model you will make will be more complicated. This is going to make an effect of accerlating the convergence of cost function, which means the error would be reduced fitting the data. But you make sure it may cause overfitting as well.

### Second hidden layer
```python
# hidden layer2
W2 = tf.Variable(tf.random_uniform([256, 256], -1., 1.))
b2 = tf.Variable(tf.random_uniform([256], -1., 1.))
L2 = tf.sigmoid(tf.matmul(L1,W2) + b2)
L2 = tf.nn.dropout(L2, keep_prob)
```
In the same manner as the `first hidden layer` above. 
Fourth line of this code,
``` python
L2 = tf.nn.dropout(L2, keep_prob)
```
is implementing dropout with the layer L2 and the probablity keep_prob. As it does, only certain percentage of nodes will participate to the learning so that it prevents few nodes overfitting.

### Ouptut layer
```python
# ouput layer
W3 = tf.Variable(tf.random_uniform([256, 10], -1., 1.))
b3 = tf.Variable(tf.random_uniform([10], -1., 1.))
logits = tf.matmul(L2,W3) + b3
hypothesis = tf.nn.softmax(logits)
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(labels=Y, logits=logits))
```
Next layer is the output layer that should classify the image among 0 to 9, and that's why 10 is used to `W3` and `b3` variables.

### Optimize
```python
# Adam Optimizer
opt = tf.train.AdamOptimizer(
    learning_rate=0.001,
    beta1=0.9,
    beta2=0.999,
    use_locking=False,
    name='Adam').minimize(cost)
```
This is adam optimzer and you can see `gradient descent optimzer` below.

```python
# Gradient Descent Optimizer
opt = tf.train.GradientDescentOptimizer(learning_rate=0.1).minimize(cost)
```

### Learning and Result
```python
# the size to learn at once.
batch_size = 100

# open the session
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    # one epoch is for learning all input data once.
    for epoch in range(15): # run 15 times
        avg_cost = 0 #cost average
        total_batch = int(mnist.train.num_examples/batch_size)
        for i in range(total_batch):
          batch_xs, batch_ys = mnist.train.next_batch(batch_size)
          c, _ = sess.run([cost,opt], feed_dict={X:batch_xs, Y:batch_ys, keep_prob:0.75})
          avg_cost += c / total_batch
        # print cost for each epoch
        print(f'Epoch: {epoch+1}, cost= ', end='')
        print('{:.9f}'.format(avg_cost))

    # check correctness
    is_correct = tf.equal(tf.argmax(hypothesis,1), tf.argmax(Y,1))
    accuracy = tf.reduce_mean(tf.cast(is_correct, tf.float32))
    print("Accuracy", sess.run(accuracy, feed_dict={X:mnist.test.images, Y:mnist.test.labels, keep_prob:1}))
```
We want the expected value through the model is similar with the real value. We can figure out how it worked well with the test data and the variable `accuracy` shows the result intuitively. `keep_prob` in the last line is assigned `1` as all test data should be used to calculate accuracy. **The accuracy is obviously more high** compared to the model without dropout and adam optimizer, which means we can say 
> dropout and optimizer is helpful to improve model performance

