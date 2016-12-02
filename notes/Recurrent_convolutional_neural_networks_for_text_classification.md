# Recurrent Convolutional Neural Networks for Text Classification
- Lai et al. [[paper]](http://www.aaai.org/ocs/index.php/AAAI/AAAI15/paper/download/9745/9552) 


* Motivation: Text classification requires many human-designed features
* Contribution: 
  - Recurrent convolutional neural network for text classification, without human features
* Method: 
    1. A recurrent structure to capture contextual information as far as possible
    when learning word representations
    2. A max-pooling layer that automatically judges which words play key roles in text classification to 
    capture the key components in texts
* Results: 
* Future work: 

## Introduction
- key problem in text classifcation : feature representation
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
- Socher et al(2011a).  
[[paper]](https://arxiv.org/pdf/1509.01626v3.pdf) 
[[notes]]() 
: paraphrase detection method with RNN
- Socher et al(2011b).  
[[paper]](https://arxiv.org/pdf/1509.01626v3.pdf) 
[[notes]]() 
: semi-supervised recursive autoencoders to predict the semtiment of a sentence
- Kalchbrenner and Blunsom.  
[[paper]](https://arxiv.org/pdf/1509.01626v3.pdf) 
[[notes]]() 
: RNN for dialogue act classification

## Prerequisites


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
![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/RCNN_text_classification_result.JPG)
  

## Conclusion
- Contributions:
  1. New model: RCNN
  2. Tackling problem: effective model without external features
  3. Performance: proven effective in 4 text classification datasets
  4. Proposed material:
- Future works:
