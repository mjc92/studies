# Text Classification Improved by Integrating Bidirectional LSTM with Two-dimensional Max Pooling
- Zhou et al. [[paper]](https://arxiv.org/pdf/1611.06639v1.pdf) 


* Motivation: previously used 1D max-pooling over the time-step dimension of RNNs may destroy the structure of feature representation
* Contribution: 
  1. A combined framework proposed
  2. 2 combined models (BLSTM-2DPooling & BLSTM-2DCNN) tested on six text classification tasks
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
  - max pooling instead of average pooling (Collobert et al.)
    - because only a few words and their combination are useful for capturing the meaning of a document
    - time complexity O(n)
![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/RCNN_text_classification_model.JPG)
- Pre-training word embedding
  - Skip-gram used to pre-train the word embedding
- speedup approaches (e.g. hierarchical softmax) will be used

## Experiment settings

- Datasets
  a. 20Newsgroups (category)
  b. Fudan set (document classification)
  c. ACL Anthology Network (language)
  d. Stanford Sentiment Treebank (very negative ~ very positive)

- Experiment Settings
  - hyper-parameter settings brought from Collobert et al.
  - learning rate: 0.01, hidden layer size: 100, word embedding size: 50, context vector size: 50
  
- Comparison of Methods
  - use unigram/bigram as features, measure with LR and SVM
  - use weighted average of word embeddings and then apply softmax layer, weight of each word is tf-idf value
  - LDA for capturing the semantics of texts in classification tasks (ClassifyLDA-EM, Labeled-LDA)
  - tree kernels as features for language classification tasks (context-free grammar, re-ranking feature set(C&J)
  - RecursiveNN + Recursive Neural Tensor Network taken as comparison measures
  - CNN by Collobert et al. for comparison


## Results and Discussion
- NN outperforms traditional methods for all datasets
  - NN-based approach can compose the semantic representation of texts than BoW models, and suffer less from data sparsity
- (R)CNNs perform better tharn RecursiveNNs
  - CNN-framework is more suitable for constructing the semantic representation of texts compared with previous NNs
  - CNn can select more discriminative features through the max-pooling layer and capture contextual info through
    conv. layer, while RecursiveNN only captures from the textual tree
  - lower time complexity (O(n)) compared to O(n^2)
  - RCNN outperforms state-of-the-art in most fields
  - RCNN outperforms CNN in all cases
- Contextual Information
  - CNN requires a fixed window size: small window(loss of long-dist. patterns) vs. wide window(data sparsity)
![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/RCNN_text_classification_results.JPG)
  

## Conclusion
- Contributions:
  1. New model: RCNN
  2. Tackling problem: effective model without external features
  3. Performance: proven effective in 4 text classification datasets
  4. Proposed material:
- Future works:
