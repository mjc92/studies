# Identity mappings in deep residual networks
- He et al.[[paper]](https://arxiv.org/abs/1603.05027)

* Motivation: to analyze propagation formulations behind the residual building blocks and come up with a better residual model
* Contribution:
  - results on CIFAR-10/100 with a 1001-layer ResNet
  - improved results on ImageNet with a 200-layer ResNet, while original 200-layer ResNet overfits
* Method:
  - design a new residual network that adds the original form **after** applying activation functions
* Results: 
  - 
* Future work: 

## Analysis of deep residual networks
- ResNet units
  - original : y_l = h(x_l) + F(x_l, W_l), x_(l+1) = f(y_l)
    - x_l : input to l-th layer, h(x) = x (identity mapping), f : ReLU
  - modified version : add x_(l+1) = y_l, to the original so that
    - x_(l+1) = x_l + F(x_l, W_l)
  - so every x_l now is the summation of the outputs of all preceding residual functions F (plus initial x_0)
    - in original "plain network", x_l is a series of matrix-vector products (obtained by multiplication)
  - easier for backprop
- Discussions
  - two identity mappings : 1) h(x_l) = x_l, 2) f is an identity mapping

## On the importance of identity skip connections
- modification -> if we change h to h(x) = lmb*X,
  - then we lose simplicity (look at paper)
- Experiments on skip connections
  - CIFAR-10
  - 100-layer ResNet; 54 two-layer Residual units; 3x3 conv layers
  - median accuracy of 5 runs
  - f = ReLU
  - various shortcut connections tested
    - constant scaling (add only 0.5*times from both original and residual)
    - exclusive gating (add result from sigmoid)
    - shortcut-only gating (apply sigmoid only to the shortcut)
    - conv shortcut (apply conv to the shortcut)
    - dropout shortcut (apply dropout to the shortcut)
  - various usages of activation within ResNet (Figure 4)

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
