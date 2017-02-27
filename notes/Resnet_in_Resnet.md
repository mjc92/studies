# ResNet in ResNet: Generalizing residual architectures
- Targ et al.
[[paper]](https://arxiv.org/pdf/1603.08029.pdf)
- 2016
- overview: a very simple paper

* Motivation: resnets have multiple levels of feature representation in a single layer
* Contribution: 
  1. architectures using generalized residual blocks can optimize as well as identity shortcuts connections + improve expressivity and ease of removing unneeded information
  2. ResNet in ResNet (RiR) : these generalized residual blocks within framework of a ResNet
* Method: 
* Results:
  1. state-of-the-art performance of the RiR architecture on CIFAR-100
* Future work: 

## Related work

## Prerequisites
- Mikolov et al. Distributed Representations of Words and Phrases and their Compositionality
[[paper]](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf) 
[[notes]]() 
: Word2Vec used on convolutional layer of this paper

## Model
- generalized residual block
  1. residual stream **r** : corresponds to **x** from an original renet block
  2. transient stream **t** : standard convolutional layer


## Datasets and Experimental Setup
- hyperparameters
  - ReLU
  - filters sizes: 3,4,5 with 100 each
  - l2 constraint(s): 3
  - dropout rate: 0.5
  - minibatch size: 50
  - early stopping on dev sets
  - stochastic gradient descent over shuffled mini-batches with Adadelta update rule
- Pretrained word vectors
  - word2vec trained on 100 billion words from Google News
  - dimensionality of 300
  - trained by CBOW
- Model variations
  - CNN-Rand: baseline, all words randomly initialized then modified during training
  - CNN-static: model with pretrained word2vec, which is kept tstatic
  - CNN-non-static: pretrained word2vec, fine-tune them for each task
  - CNN-multichannel: 2 sets of word vectors
    - each filter is applied to both "channels(each set of word vector)", but backprop to only one of them
    - both initialized by word2vec

## Results and Discussion
- Baseline model(CNN-rand) performs poorly
- Even a simple model (CNN-static) performs as well as complex models from other papers

### Multichannel vs Single Channel Models
- Multichannel architecture might prevent overfitting?
  - mixed results...

### Static vs Non-static Representations
- both single and multichannel non-static models can fine-tune its channel to task-at-hand

### Further Observations
- dropout is a good regularizer (2~4% performance boost)

## Conclusion
- Contributions:
  1. New model: a simple CNN with one layer of convolution performs well
  2. Tackling problem: 
  3. Performance: 
- Future works:
