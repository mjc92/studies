# ResNet in ResNet: Generalizing residual architectures
- Targ et al.
[[paper]](https://arxiv.org/pdf/1603.08029.pdf)
- 2016
- overview: simple workshop paper; added some networks to the original resnet

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
    - role of t: process information from either stream in a nonlinear manner w/o shortcut connections, and allows info from earlier states to be discarded
  - please check Figure 1
  - still unsure how earlier information is discarded


## Experiments
- hyperparameters
  - CIFAR-10 & CIFAR-100
  - SGD w/ momentum 0.9
  - minibatch size : 500
  - L2 penalty : 0.0001
  - train 82 epochs
  - scaled by 0.1 after 42 & 62 epochs
  - MSR initialization for all weight tensors
- CIFAR-10
  - fractional max-pooling performs best
  - 18-layer + wide RiR performs better than original ResNet
- CIFAR-100
  - 18-layer + wide RiR performs better
  - 18-layer + wide ResNet performs better than when using ResNet Init
