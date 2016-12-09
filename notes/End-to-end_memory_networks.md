# End-To-End Memory Networks
- Sukhbaatar et al. [[paper]](https://arxiv.org/pdf/1503.08895v5) 


* Motivation: 
  - build models that can make multiple computational steps in QA or task completion
  - models that can describe long term dependencies in sequential data
* Contribution: 
  1. RNN where recurrence reads from a large external memory multiple times before outputting a symbol
  2. Same performance as Memory Network model with wider applicability
    - excellent performance on 4 out of 6 tasks
* Method: 
* Results: 
* Future work: 

## Introduction
- Two approaches have been made to build models that make multiple computational steps in QA or task completion + describe long term dependencies in sequential data
  - Memory networks
  - Attention + RNN
- Here we combine them to create an RNN architecture where recurrence reads from an external memory multiple times before outputting a symbol
- unlike memory networks, ours is continuous and can be trained end-to-end and requires less supervision
- unlike RNNsearch, ours has multiple computational steps("hops") which shows better performance

## Related work
- Graves et al. Neural turing machines   
[[paper]]() 
[[notes]]() 
: also uses a continuous memory representation
  - uses both content and address-based access (ours uses only content-based access)
  - a more complex model (ex. requires sharpening)
  - more abstract operations of sorting and recall are challenges compared to this model which is applied to textual reasoning
- Xu et al. Show, Attend and Tell: Neural Image Caption Generation with Visual Attention  
[[paper]]() 
[[notes]]() 
: Similar in that it uses an attention model
- Zaremba et al. Recurrent neural network regularization
[[paper]]() 
[[notes]]() 
: state-of-the-art for language modeling
  - uses very large LSTMs with Dropout
- Mikolov et al. Learning longer memory in recurrent neural networks
[[paper]]() 
[[notes]]() 
: state-of-the-art for language modeling
  - uses RNNs with diagonal constraints on the weight matrix


## Prerequisites

- Weston et al, Memory Networks
[[paper]](https://web.eecs.umich.edu/~honglak/naacl2016-dscnn.pdf)
[[notes]]()
: unlike memory networks, our model is trained end-to-end, requires less supervision during traing
  - this is a continuous form of the Memory Network
    - that model was not eas to train via backpropagation, and required supervision at each layer of the network
- Bahdanau et al, Neural machine translation by jointly learning to align and translate
[[paper]](복붙)
[[notes]](복붙)
: similar, pllus multiple computational steps performed per output symbol


- Misunderstanding: 1D convolutional filter in NLP tasks has one dimension?
  - (X) two dimensions (k,d)
- Zhang et al, Dependency sensitive convolutional neural networks for modeling sentences and documents
[[paper]](https://web.eecs.umich.edu/~honglak/naacl2016-dscnn.pdf)
[[notes]]()
: introduces DSCNN, utilizes LSTM, applies 1D convolution and 1D max pooling
- Wen et al, Learning text representation using recurrent convolutional neural network with highway layers
[[paper]](https://arxiv.org/pdf/1606.06905.pdf)
[[notes]]()
: introduces RCNN, utilizes biRNN, applies 1D convolution and 1D max pooling


## Approach
- Model structure
![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/end-to-end-mem_network_model.JPG)
- Weight tying scheme
  1. adjacent
  2. layer-wise (RNN-like)
    - model resembles an RNN where outputs are divided into "internal" and "external" outputs
    - internal output : considering memory
    - external output : predicting a label

## Experimental Setup

- Datasets
  - QA tasks consisting of a set of statements, followed by a question whose answer (typically) is a single word
    a. Sam walks into the kitchen.
    b. Sam picks up an apple.
    c. Sam walks into the bedroom.
    d. Sam drops the apple.
    *Q: Where is the apple?*
    **A: Bedroom**
  - Settings
    - there are I<=320 sentences for each example problem; a question sentence *q* and answer *a*
      - maximum 320 sentences for 1 question??
    - sentence representation
      ![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/end-to-end_mem_network_sentence_representation.JPG)

      
- Model Details
  - K=3 hops used with (type1) adjacent weight sharing
  - for sentence representation, we use 
    
- Word embeddings
  - GloVe on 6 billion tokens of Wikipedia 2014 and Gigaword 5
- Hyper-parameter Settings
  - 10% as development set
  - Macro-F1 measure for 20Newsgroups, accuracy for other datasets
  - Hyper-parameters
    - word embedding dimension: 300
    - hidden units of LSTM: 300
    - 100 convolutional filters each for window size (3,3), 2D pooling of (2,2)
    - mini-batch size as 10
    - AdaDelta with default value 1.0
    - Dropout with 0.5 for word embeddings, 0.2 for BLSTM and 0.4 for penultimate layer
    - l2 penalty with coef 10^-5 pver [ara,eters
  - values chosen via a grid search on the SST-1 development set


## Results
- Overall performance
  - BLSTM-2DCNN does good in 4/6 tasks
  - BLSTM-2DPooling does worse than state-of-the-art models
- Effect of Sentence Length
  - accuracies decline with the length of sentences increasing
- Effect of 2D convolutional filter and 2D max pooling size
  - best with 2D filter size (5,5) and 2D max pooling size (5,5)
  

## Conclusion
- Contributions:
  1. New model: BLSTM-2DPooling, BLSTM-2DCNN (extension of first model)
  2. Tackling problem: hold feature vector dimension information as well as time-step dimension information
  3. Performance: 2DCNN outperforms previous models, 2DPooling and DSCNN
    - sensitivity analysis on SST-1 dataset shows that large filters can detect more features
  4. Proposed material:
- Future works:
