# AI Backpropagation
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: [AI]
subPage: []
enable: [toc]

---

http://colah.github.io/posts/2015-08-Backprop/
https://cs231n.github.io/optimization-2/

## Computational Graphs
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T184704495Z-image.png)
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T184748118Z-image.png)

## Derivatives on Computational Graphs
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T184941573Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T184952213Z-image.png)


d(f(g(x)))/dx = df/dg * dg/dx

de = d(c * d) = d * de/dc + c * de/d(d) 
> '+'  together

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T185304360Z-image.png)


## Factoring Paths
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T185051050Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T185610905Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T185811748Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T191023466Z-image.png)

Forward-mode differentiation tracks how one input affects every node. Reverse-mode differentiation tracks how every node affects one output. 

## Computational Victories
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T184952213Z-image.png)
### forward-mode
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T191447348Z-image.png)

### backward-mode
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T191520795Z-image.png)

de/da = de/dc * dc/da = 2 * 1 = 2

de/db = de/dc * dc/db + de/dd * dd/db = 2 * 1  + 3 * 1 = 5

When training neural networks, we think of the cost (a value describing how bad a neural network performs) as a function of the parameters (numbers describing how the network behaves). We want to calculate the derivatives of the cost with respect to all the parameters, for use in gradient descent. Now, there’s often millions, or even tens of millions of parameters in a neural network. So, reverse-mode differentiation, called backpropagation in the context of neural networks, gives us a massive speed up!

Where the reverse-mode gives the derivatives of one output with respect to all inputs, the forward-mode gives us the derivatives of all outputs with respect to one input. If one has a function with lots of outputs, forward-mode differentiation can be much, much, much faster.

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230606T215455851Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230606T215005501Z-image.png)

```python
w = [2,-3,-3] # assume some random weights and data
x = [-1, -2]

# forward pass
dot = w[0]*x[0] + w[1]*x[1] + w[2]
f = 1.0 / (1 + math.exp(-dot)) # sigmoid function

# backward pass through the neuron (backpropagation)
ddot = (1 - f) * f # gradient on dot variable, using the sigmoid gradient derivation
dx = [w[0] * ddot, w[1] * ddot] # backprop into x
dw = [x[0] * ddot, x[1] * ddot, 1.0 * ddot] # backprop into w
# we're done! we have the gradients on the inputs to the circuit
```

```python
# forward pass
W = np.random.randn(5, 10)
X = np.random.randn(10, 3)
D = W.dot(X)

# now suppose we had the gradient on D from above in the circuit
dD = np.random.randn(*D.shape) # same shape as D
dW = dD.dot(X.T) #.T gives the transpose of the matrix
dX = W.T.dot(dD)

```

## 
http://neuralnetworksanddeeplearning.com/chap2.html#the_hadamard_product_$s_\odot_t$

Hadamard product or Schur product. 
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/200/image/20230602T193116120Z-image.png)