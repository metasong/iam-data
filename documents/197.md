# RNN and LSTM
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: [AI]
subPage: []
enable: [toc]

---
http://introtodeeplearning.com/slides/6S191_MIT_DeepLearning_L2.pdf
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T190617534Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T190931053Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T204826149Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T191239460Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T191705905Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T192252382Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T192828507Z-image.png)


## Backpropagation
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T200749780Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T201012491Z-image.png)

## Long-Term Dependencies
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T201307467Z-image.png)



![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T200106491Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T200446829Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T200244841Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T195950093Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T200150593Z-image.png)


## LSTM Networks
http://colah.github.io/posts/2015-08-Understanding-LSTMs/

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T205130305Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T205145080Z-image.png)

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T205700693Z-image.png)
 decide what information we’re going to throw away from the cell state. The sigmoid outputs numbers between zero and one, describing how much of each component should be let through. A value of zero means “let nothing through,” while a value of one means “let everything through!”
 
 
 ![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T210048749Z-image.png)
 decide what new information we’re going to store in the cell state. This has two parts. First, a sigmoid layer called the “input gate layer” decides which values we’ll update. Next, a tanh layer creates a vector of new candidate values, C~t
, that could be added to the state.

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T210604627Z-image.png)
to update the old cell state, Ct−1, into the new cell state Ct.

 
 ![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T210721846Z-image.png)
This output will be based on our cell state, but will be a filtered version. First, we run a sigmoid layer which decides what parts of the cell state we’re going to output. Then, we put the cell state through tanh  (to push the values to be between −1 and 1) and multiply it by the output of the sigmoid gate, so that we only output the parts we decided to.

### Variants on Long Short Term Memory
![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T211442987Z-image.png)
One popular LSTM variant, introduced by Gers & Schmidhuber (2000), is adding “peephole connections.” This means that we let the gate layers look at the cell state.

![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T211635680Z-image.png)
Another variation is to use coupled forget and input gates. Instead of separately deciding what to forget and what we should add new information to, we make those decisions together. We only forget when we’re going to input something in its place. We only input new values to the state when we forget something older.


![](https://raw.githubusercontent.com/metasong/iam-data/master/documents/197/image/20230605T211907213Z-image.png)
A slightly more dramatic variation on the LSTM is the Gated Recurrent Unit, or GRU, introduced by Cho, et al. (2014). It combines the forget and input gates into a single “update gate.” It also merges the cell state and hidden state, and makes some other changes. The resulting model is simpler than standard LSTM models, and has been growing increasingly popular.
 
### Conclusion
LSTMs were a big step in what we can accomplish with RNNs. It’s natural to wonder: is there another big step? A common opinion among researchers is: “Yes! There is a next step and it’s attention!” The idea is to let every step of an RNN pick information to look at from some larger collection of information.

## ARNN
https://distill.pub/2016/augmented-rnns/