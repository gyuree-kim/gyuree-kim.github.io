---
title: "[Deep Learning][Autoencoder] 0부터 9까지 숫자 이미지에 오토인코더 적용하기"
date: 2021-04-29 14:29:28 -0400
categories: deep-learning
---

# Autoencoder

## Overall structure
![autoencoder_schema](./images/autoencoder_schema.jpg)<br>
[이미지 링크](https://www.google.com/search?q=autoencoder+deep+learning&safe=active&sxsrf=ALeKk03Z-Xfxr63soyZ4dVk8Vac9jFafYA:1619618439170&source=lnms&tbm=isch&sa=X&ved=2ahUKEwj97fPtjKHwAhVtF6YKHY8tA_IQ_AUoAXoECAIQAw&biw=1264&bih=560#imgrc=t8UiO_9h1cy-UM) 
위 그림과 같이 `auto encoder(오토 인코더)`는 원본 숫자 이미지를 `encoder`를 통해 압축한 뒤, `decoder`를 통해 복원하는 구조를 갖는다.

![autoencoder_layer_schema](./images/autoencoder_layer_schema.png)
[이미지 링크](https://www.google.com/search?q=autoencoder+deep+learning&safe=active&sxsrf=ALeKk03Z-Xfxr63soyZ4dVk8Vac9jFafYA:1619618439170&source=lnms&tbm=isch&sa=X&ved=2ahUKEwj97fPtjKHwAhVtF6YKHY8tA_IQ_AUoAXoECAIQAw&biw=1264&bih=560#imgrc=mtVZOXI1aOPYOM)
계층 구조는 이와 같다. 본 모델에서 사용할 0부터 9까지의 숫자 이미지인 `input data`는 28*28 크기로 할당되며, 여러개의 은닉 계층을 거치며 차원을 축소시킴으로서 `encoder`를 적용한다. 이 후 반대의 방법으로 기존 이미지의 차원의 크기로 복원하는 과정을 거친다. 본 모델에서는 input data에 `random noise`를 더해준다.

## Code description

### Import libraries
모델링에 필요한 라이브러리 `tensorflow`, `numpy`, `matplotlib`를 import한다.
```python
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt
import os
os.environ['KMP_DUPLICATE_LTB_OK']='True'
```

### Import input data
`input data`로 사용할 데이터들을 import 한다.
```python
from tensorflow.examples.tutorials.mnist import input_data
mnist = input_data.read_data_sets("./mnist/data/", one_hot=True)
```

### Assign values to hyperparameters
학습에 필요한 `hyperparameters`의 값을 지정한다.
```python
batch_size = 100        # 한 번에 학습할 데이터 사이즈. 배치 크기
learning_rate = 0.01    # 학습 속도. 수렴속도 alpha
epoch_num = 20          # epoch(전체 데이터 셋 크기)만큼 학습할 횟수
n_input = 28*28         # 하나의 input data의 크기는 28*28으로 할당 
n_hidden = 256          # 하나의 hiden node의 크기는 256으로 할당
noise_level = 0.6       # noise 정도
```

### Placeholders
`input layer`를 위한 placehodlers를 생성한다.
```python
X_noisy = tf.placeholder(tf.float32, [None, n_input])
Y = tf.placeholder(tf.float32, [None, n_input])
```

### Encoder
데이터를 `encoding`하기 위해 필요한 `weight`값과 `bias`값을 정의한 후 식 `WX+b`를 활용하여 행렬곱 연산을 실행한다. 이후 `activation function(활성함수)`인 `sigmoid`함수를 통과시킨다. `input layer`에서 `hidden layer`로 가는 과정이기 때문에 가중치에 [n_input, n_hidden], 바이어스에 [n_hidden] 크기가 할당된 것을 확인할 수 있다. 
```python
W_encode = tf.Variable(tf.random_uniform([n_input, n_hidden], -1., 1.))
b_encode = tf.Variable(tf.random_uniform([n_hidden], -1., 1.))

encoder = tf.nn.sigmoid(tf.add(tf.matmul(X_noisy, W_encode), b_encode)) 
```
은닉 계층의 수를 늘리고 싶다면 여기에 코드를 추가하면 된다. 하단 `full source`에서 확인할 수 있다.

### Decoder
`Encoder`와 반대로 차원을 복원한다. 계산과정은 위와 동일하게 `weight`, `bias` 를 활용해 계산한 이후 `sigmoid`함수에 넣어주는 방식이다. 본문의 최상단 이미지를 이해했다면 아래 코드에서 `W_decode`, `b_decode`에 할당된 크기를 이해하는데 어렵지 않다.
```python
W_decode = tf.Variable(tf.random_uniform([n_hidden, n_input], -1., 1.))
b_decode = tf.Variable(tf.random_uniform([n_input], -1., 1.))

decoder = tf.nn.sigmoid(tf.add(tf.matmul(encoder, W_decode), b_decode)) 
```

### Cost and Optimizer
`cost`를 정의하고 `adam optimizer`를 적용시킨다.
```python
cost = tf.reduce_mean(tf.square(Y-decoder))
optimizer = tf.train.AdamOptimizer(learning_rate).minimize(cost)
```

### Learning
```python
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    total_batch = int(mnist.train.num_examples/batch_size)
    
    for epoch in range(epoch_num):
        avg_cost = 0
        for i in range(total_batch):
            batch_xs, batch_ys = mnist.train.next_batch(batch_size)
            # create random noise
            batch_x_noisy = batch_xs + noise_level * np.random.normal(loc=0., scale=1., size=batch_xs.shape)
            _, cost_val = sess.run([optimizer, cost], feed_dict={X_noisy: batch_x_noisy, Y: batch_xs})
            avg_cost += cost_val / total_batch
        print('Epoch:', '%d'%(epoch+1), 'cost:', '{:9f}'.format(avg_cost))
        
    test_X = mnist.test.images[:10] + noise_level * np.random.normal(loc=0., scale=1., size=mnist.test.images[:10].shape)
    
    samples = sess.run(decoder, feed_dict={X_noisy: test_X})
    fig, ax = plt.subplots(3, 10, figsize=(10,3))
    
    for i in range(10):
        ax[0][i].set_axis_off()
        ax[1][i].set_axis_off()
        ax[2][i].set_axis_off()
        ax[0][i].imshow(np.reshape(mnist.test.images[i], (28, 28)))
        ax[1][i].imshow(np.reshape(mnist.test.images[i], (28, 28)))
        ax[2][i].imshow(np.reshape(mnist.test.images[i], (28, 28)))
```

## Result description
### First case 
은닉계층이 하나인 경우 결과는 다음과 같다. `Code description`과 동일한 코드를 사용하였으며 **cost는 약 0.030173**이고 이미지의 복원도 훌륭한 것을 확인할 수 있다.
![first-case](./images/first_case.PNG)
![first-result](./images/one-hidden-layer.PNG)

### Second case
`encoder`와 `decoder`의 계층을 하나씩 추가했을 때의 결과는 다음과 같으며 전체 소스 코드는 아래 `Full source`에서 확인할 수 있다. **cost는 0.031088**로 `first case`의 성능과 비슷하게 나타난다.
![second-case](./images/second_case.PNG)
![second-result](./images/second_case_result.PNG)

## Full source
더 깊은 구조를 위한 코드를 포함하여 작성하였다.

```python
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt
import os
os.environ['KMP_DUPLICATE_LTB_OK']='True'

from tensorflow.examples.tutorials.mnist import input_data
mnist = input_data.read_data_sets("./mnist/data/", one_hot=True)

# hyperparameters
batch_size = 100
learning_rate = 0.01
epoch_num = 20
n_input = 28*28
n_hidden1 = 256
n_hidden2 = 128     # 두 번째 은닉계층
noise_level = 0.6

# placeholders
X_noisy = tf.placeholder(tf.float32, [None, n_input])
Y = tf.placeholder(tf.float32, [None, n_input])

# encoders(1)
W_encode1 = tf.Variable(tf.random_uniform([n_input, n_hidden1], -1., 1.))
b_encode1 = tf.Variable(tf.random_uniform([n_hidden1], -1., 1.))

encoder_h1 = tf.nn.sigmoid(tf.add(tf.matmul(X_noisy, W_encode1), b_encode1)) 

# encoders(2) - 추가됨
W_encode2 = tf.Variable(tf.random_uniform([n_hidden1, n_hidden2], -1., 1.))
b_encode2 = tf.Variable(tf.random_uniform([n_hidden2], -1., 1.))

encoder_h2 = tf.nn.sigmoid(tf.add(tf.matmul(encoder_h1, W_encode2), b_encode2)) 

# decoders(1) - 추가됨
W_decode1 = tf.Variable(tf.random_uniform([n_hidden2, n_hidden1], -1., 1.))
b_decode1 = tf.Variable(tf.random_uniform([n_hidden1], -1., 1.))

decoder = tf.nn.sigmoid(tf.add(tf.matmul(encoder_h2, W_decode1), b_decode1)) 

# decoders(2)
W_decode2 = tf.Variable(tf.random_uniform([n_hidden1, n_input], -1., 1.))
b_decode2 = tf.Variable(tf.random_uniform([n_input], -1., 1.))

output = tf.nn.sigmoid(tf.add(tf.matmul(decoder, W_decode2), b_decode2)) 

# to evaluate cost
cost = tf.reduce_mean(tf.square(Y-output))
# use adam optimizer
optimizer = tf.train.AdamOptimizer(learning_rate).minimize(cost)

# run the model and print results
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    total_batch = int(mnist.train.num_examples/batch_size)
    
    for epoch in range(epoch_num):
        avg_cost = 0
        for i in range(total_batch):
            batch_xs, batch_ys = mnist.train.next_batch(batch_size)
            batch_x_noisy = batch_xs + noise_level * np.random.normal(loc=0., scale=1., size=batch_xs.shape)
            _, cost_val = sess.run([optimizer, cost], feed_dict={X_noisy: batch_x_noisy, Y: batch_xs})
            avg_cost += cost_val / total_batch
        print('Epoch:', '%d'%(epoch+1), 'cost:', '{:9f}'.format(avg_cost))
        
    test_X = mnist.test.images[:10] + noise_level * np.random.normal(loc=0., scale=1., size=mnist.test.images[:10].shape)
    
    samples = sess.run(output, feed_dict={X_noisy: test_X})
    fig, ax = plt.subplots(3, 10, figsize=(10,3))
    
    for i in range(10):
        ax[0][i].set_axis_off()
        ax[1][i].set_axis_off()
        ax[2][i].set_axis_off()
        ax[0][i].imshow(np.reshape(mnist.test.images[i], (28, 28)))
        ax[1][i].imshow(np.reshape(mnist.test.images[i], (28, 28)))
        ax[2][i].imshow(np.reshape(mnist.test.images[i], (28, 28)))
```