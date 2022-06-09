---
layout: post
title: Rudimentary Math for Deep Learning: Activation Functions
description: activation 함수들
date: 2022-06-01
last_modified_at: 2022-06-02
categories: [nlp, math, activation functions, 활성화 함수]
---

딥러닝을 이해할 때 필요한 기본 수학 중 활성화 함수에 대해 정리하자.

## activation function

### sigmoid
![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/Logistic-curve.svg/1200px-Logistic-curve.svg.png)
$$ 
sigmoid(x) = \frac{1}{1+e^{-x}}
$$

sigmoid는 모든 x에 대해 (0,1)의 값을 맵핑하고 이는 분류 과제에서 확률로도 해석할 수 있기 때문에 유용하다. 그러나 약간이라도 큰 x값에 있어서 기울기가 0에 가깝기 때문에 레이어가 많은 모델에서 역전파시 점점 기울기가 0에 수렴하여 사실상 없어진다는 단점이 있다.

### softmax

$$
s({z}_i) = \frac{e^{z_i}}{\sum_{j}{e^{z_j}}}
$$
sigmoid 함수처럼 softmax 함수도 $\R^n \to (0, 1)^n$ 이기 때문에 확률분포로 사용할 수 있다. 그러나 softmax는 분모에서 보듯이 normalize하기 때문에 결과값의 합이 1이라는 특성 때문에 다중분류(multi-class classification)에 사용할 수 있다.

![](https://www.researchgate.net/profile/Nabi-Nabiyev-2/publication/349662206/figure/fig3/AS:995882686246913@1614448343589/Working-principles-of-softmax-function.jpg)
미분값을 계산할 때는 주로 계산의 편의성 때문에 log를 씌워 계산한다.
$$
\log{s({z}_i)} = \log{e^{z_i}} - \log{\sum_{j}{e^{z_j}}} \\=z_i - \log{\sum_{j}{e^{z_j}}}
\\
\frac{\partial}{\partial {z_j}}\log{s({z}_i)} = \frac{\partial}{\partial {z_j}}(z_i - \log{\sum_{j}{e^{z_j}}}) 
\\= \frac{\partial z_i}{\partial z_j} - \frac{1}{\sum_{j}{e^{z_j}}}(\frac{\partial}{\partial z_j} \sum_{j}{e^{z_j}})
\\=\frac{\partial z_i}{\partial z_j} - \frac{1}{\sum_{j}{e^{z_j}}} \cdot \frac{\partial}{\partial {z_j}}[e^{z_1} + \dots+e^{z_j}] 
$$
이다. i = j 일 때와 i != j일 때 미분값이 달라지는데
1. $i = j$
$$ 
\frac{\partial}{\partial {z_j}}\log{\sigma({z}_i)} = 1 - \frac{e^{z_i}}{\sum_{j}{e^{z_j}}} = 1 - s(z_i)
$$

2) $i \ne j$
$$
\frac{\partial}{\partial {z_j}}\log{\sigma({z}_i)} =0 - \frac{e^{z_i}}{\sum_{j}{e^{z_j}}} = - s(z_i)
$$