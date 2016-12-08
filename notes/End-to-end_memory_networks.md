# End-To-End Memory Networks
- Sukhbaatar et al. [[paper]](https://arxiv.org/pdf/1503.08895v5) 


* Motivation: 
  - build models that can make multiple computational steps in QA or task completion
  - models that can describe long term dependencies in sequential data
* Contribution: 
  1. RNN where recurrence reads from a large external memory multiple times before outputting a symbol
  2. Same performance as Memory Network model with wider applicability
    - excellent performance on 4 out of 6 tasks
  3. experiments on Stanford Sentiment Treebank fine-grained task
    - performance of models on different length of sentences
    - sensitivity analysis of 2D filter and max pooling size
* Method: 
    1. BLSTM to capture long-term sentence dependencies
    2. extracts features by 2D convolution and 2D max pooling operation for sequence modeling
* Results: 
* Future work: 

## Introduction
- RNNs can capture features from text of variable length
  1. convert tokens comprising each text into vectors (word embeddings), and form a matrix
  1. feature selection methods (frequency, MI, pLSA, lDA) used
  2. however, they ignore the contextual information or word order in texts and can't capture semantics well
- Pre-trained word embeddings and DNNs can help capture the semantic representation of texts
  1. Recursive Neural Network (RecursiveNN) constructs sentence representation well, but captures semantics via a tree structure
    - performance dependes on textual tree construction performance
    - time complexity O(n^2)
    - so, it is unsuitable for modeling long sentences or documents
  2. Recurrent Neural Network
    - time complexity O(n)
    - can capture contextual information and the semantics of long texts
    - biased: latter words are more dominant than earlier words, less effective in capturing semantics of a whole document
  3. Convolutional Neural Network
    - unbiased: can fairly determine discriminative phrases in a text with a max-pooling layer
    - time complexity O(n)
    - using convolutional kernels with a fixed window size; hard to determine size (info loss vs large parameter space)
  4. **Solution: Recurrent Convolutional Neural Network**
    1. bi-directional recurrent structure
      -less noise compared to traditional window-based network when 
      capturing contextual information as much as possible when learning word representations
      - the model can reserve a larger range of the word ordering when learning representations of text
    2. max-pooling layer that judges which features are important in text classification

## Related work
- Zhou et al. A c-lstm neural network for text classification   
[[paper]](https://pdfs.semanticscholar.org/10f6/2af29c3fc5e2572baddca559ffbfd6be8787.pdf) 
[[notes]]() 
: used CNN to extract a sequence of higher-level phrase representations and feed them into an LSTM to obtain the sentence representation
- Zhou et al. Attention-based bidirectional long short-term memory networks for relation classification  
[[paper]](https://arxiv.org/pdf/1509.01626v3.pdf) 
[[notes]]() 
: BLSTM with attention mechanism to automatically select features that have a decisive effect on classification

- Yang et al. Hierarchical attention networks for document classification
[[paper]](https://www.cs.cmu.edu/~diyiy/docs/naacl16.pdf) 
[[notes]]() 
: introduces a hierarchical network with two levels of attention mechanisms (word / sentence attention) for document classification
- Kalchbrenner et al. A convolutional neural network for modelling sentences
[[paper]](https://arxiv.org/pdf/1404.2188v1.pdf) 
[[notes]]() 
: a dynamic k-max pooling mechanism for sentence modeling


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


## Model
- Model structure
![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/text_classification_2D_model.JPG)
![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/text_classification_2D_setting.JPG)

## Experimental Setup

- Datasets
  a. MR (pos/neg)
  b. SST-1 (Stanford Sentiment Treebank) (very negative ~ very positive)
  c. SST-2 (pos/neg)
  d. Subj (subjective/objective)
  e. TREC (classify question type)
  f. 20Newsgroups (category)

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
