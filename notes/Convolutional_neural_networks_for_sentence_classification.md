# Convolutional Neural Networks for Sentence Classification
- Yoon Kim
[[paper]](https://arxiv.org/pdf/1408.5882v2.pdf)
- 2014

* Motivation: NER usually requires loads of feature engineering
* Contribution: 
  1. train a simple CNN with 1 layer of convolution on top of word vectors obtained by Word2Vec
  2. simple modification to architecture to allow for the use of both pre-trained and task-specific vectors
* Method: 
* Results:
  1.  available word embeddings, our model performs similarly to state-of-the-art models
  2. Using two lexicons constructed from publicly available sources, our model exceeds those with heavy feature engineering
* Future work: 


## Related work
- Ling et al. Sequence to sequence learning with neural networks
[[paper]](http://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks.pdf) 
[[notes]](https://github.com/mjc92/studies/blob/master/notes/Sequence_to_sequence_learning_with_neural_networks.md) 
: presents biLSTM for character level
- Cho et al. On the properties of neural machine translation: Encoder-Decoder approaches
[[paper]]() 
[[notes]]() 
: shows how performance drops when sentences become longer
- Graves et al. Generating Sequences With Recurrent Neural Networks
[[paper]](https://arxiv.org/pdf/1308.0850v5.pdf) 
[[notes]]() 
: used bidirectional RNN(BiRNN) in speech recognition


## Prerequisites
- Mikolov et al. Distributed Representations of Words and Phrases and their Compositionality
[[paper]](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf) 
[[notes]]() 
: Word2Vec used on convolutional layer of this paper
- Graves et al. Speech Recognition with Deep Neural Networks
[[paper]](https://www.cs.toronto.edu/~fritz/absps/RNN13.pdf) 
[[notes]]() 
: applies BiLSTM in speech-recognition
- Santos et al. Boosting Named Entity Recognition with Neural Character Embeddings
[[paper]](https://arxiv.org/pdf/1505.05008v2.pdf) 
[[notes]]() 
: uses CNNs to extract character-level features for NER
- Labeau et al. Non-lexical neural architecture for fine-grained POS Tagging
[[paper]](http://www.aclweb.org/anthology/D15-1025) 
[[notes]]() 
: uses CNNs to extract character-level features for POS-tagging


## Model
- uses 2 channels of word vectors
  1. one kept static throughout training
  2. one that is fine-tuned via backpropagation
  ![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/text_classification_CNN.JPG)  
- uses dropout on penultimate layer
  - constrain l2-norm of weights whenever the l2-norm of w exceeds a certain value s

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
