---
title: "[google colab error] AttributeError: module 'tensorflow' has no attribute 'placeholder'"
date: 2021-04-13 23:10
categories: deep-learning
---

## AttributeError: module 'tensorflow' has no attribute 'placeholder' 에러 발생했을 경우

- 원인: `tensorflow`의 버전 1.0에서 사용되던 `placeholder`가 버전 2.0으로 업그레이드되면서 인식되지 않음
- 해결 방안: 

  ```python
  import tensorflow as tf
  ```

  대신 아래 코드 입력

  ```python
  import tensorflow.compat.v1 as tf
  tf.disable_v2_behavior()
  ```
