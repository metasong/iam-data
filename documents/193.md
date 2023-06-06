# AI Classification
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: [AI]
subPage: []
enable: [toc]

---
https://cs231n.github.io/classification/

## Nearest Neighbor Classifier
```python
Xtr, Ytr, Xte, Yte = load_CIFAR10('data/cifar10/') # tr: train, te: test; x: image, y: label
# flatten out all images to be one-dimensional
Xtr_rows = Xtr.reshape(Xtr.shape[0], 32 * 32 * 3) # Xtr_rows becomes 50000 x 3072; 32*32*3 = 3072, shape: 50,000 x 32 x 32 x 3 -> 50,000 x 3072
Xte_rows = Xte.reshape(Xte.shape[0], 32 * 32 * 3) # Xte_rows becomes 10000 x 3072
```
```python
nn = NearestNeighbor() # create a Nearest Neighbor classifier class
nn.train(Xtr_rows, Ytr) # train the classifier on the training images and labels
Yte_predict = nn.predict(Xte_rows) # predict labels on the test images
# and now print the classification accuracy, which is the average number
# of examples that are correctly predicted (i.e. label matches)
print 'accuracy: %f' % ( np.mean(Yte_predict == Yte) )
```
```python
import numpy as np

class NearestNeighbor(object):
  def __init__(self):
    pass

  def train(self, X, y):
    """ X is N x D where each row is an example. Y is 1-dimension of size N """
    # the nearest neighbor classifier simply remembers all the training data
    self.Xtr = X
    self.ytr = y

  def predict(self, X):
    """ X is N x D where each row is an example we wish to predict label for """
    num_test = X.shape[0]
    # lets make sure that the output type matches the input type
    Ypred = np.zeros(num_test, dtype = self.ytr.dtype)

    # loop over all test rows
    for i in range(num_test):
      # find the nearest training image to the i'th test image
      # using the L1 distance (sum of absolute value differences)
      distances = np.sum(np.abs(self.Xtr - X[i,:]), axis = 1)
      min_index = np.argmin(distances) # get the index with smallest distance
      Ypred[i] = self.ytr[min_index] # predict the label of the nearest example

    return Ypred
```
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/193/image/20230606T193326650Z-image.png)
https://www.kaggle.com/code/residentmario/l1-norms-versus-l2-norms/notebook

https://www.mathsisfun.com/data/standard-deviation.html
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/193/image/20230606T192920980Z-image.png)
> Why L2 is not better than L1 here?
> L2 presents big variance (difference), -> larger disctance -> not the label

## k - Nearest Neighbor Classifier
instead of finding the single closest image in the training set, we will find the top k closest images, and have them vote on the label of the test image. In particular, when k = 1, we recover the Nearest Neighbor classifier. Intuitively, higher values of k have a smoothing effect that makes the classifier more resistant to outliers:

> todo continue