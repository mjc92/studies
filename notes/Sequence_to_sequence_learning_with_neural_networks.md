# Sequence-to-Sequence Learning with Neural Networks
- Sutskever et al.
[[paper]](http://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks.pdf)

* Motivation: DNNs cannot map sequences to sequences
* Contribution: 
  1. End-to-end approach to sequence learning without assumptions
  2. Single character-level encoder across multiple languages to build multilingual translation system without increasing model size
* Method: Multilayer LSTM to to map input sequence to a fixed vector, and another deep LSTM to decode the target sequence
* Results: EN-FR translation task receives BLEU of 34.8, better than a phrase-based SMT system
  - + reversing the words in source sentences only improved LSTM performance by introducing short term dependencies between source
  and target
* Future work: 

## Introduction
- DNNs can only be applied to problems with inputs/targets of fixed dimensionality
  - sequence lengths are not fixed in many occasions
- Using LSTM can solve sequence to sequence problems
  1. One LSTM reads input sequence one timestep at a time to obtain large fixed-dimensional vector representation
  2. Second LSTM conditioned on the input sequence extracts the output sequence

## Related work
- Attentional Neural Machine Translation
- Kalchbrenner and Blunsom. Recurrent continuous translation models 
[[paper]](http://www.aclweb.org/anthology/D13-1176) 
[[notes]]() 
: First to map the entire input sentence to vector
  - however, they map sentences to vectors using CNNs and lose ordering of the words
- Cho et al. Learning Phrase Representations using RNN Encoder-Decoder for Statistical Machine Translation
[[paper]](https://arxiv.org/pdf/1406.1078v3.pdf) 
[[notes]]() 
: Similar model
  - maps input sequence to a fixed-sized vector using one RNN and maps the vector to the target sequence with another RNN
- Graves et al. Generating Sequences With Recurrent Neural Networks
[[paper]](https://arxiv.org/pdf/1308.0850v5.pdf) 
[[notes]]() 
: Introduces differentiable attention mechenism that allows neural networks to focus on different parts of their input
- Bahdanau et al. **Get it somewhere**
[[paper]]() 
[[notes]]() 
: Implements the attention mechanism to machine translation

## Prerequisites
- left-to-right beam search decoder

## Methods
- LSTM instead of RNN
  - gets sequence vector v from input x1~xt, and computes probability of output sequence y1~yt'
- Our actual model differs from descriptions
  1. used 2 difference LSTMs (input, output)
  2. deep LSTMs work better (used 4 layer LSTM)
  3. it is valuable to reverse the order of the words of the input sentence
    - if a,b,c -> C,B,A then inputting c,b,a -> C,B,A gives higher results
    - but why isn't there supporting evidence?

## Experiments
- Dataset
- Reversing the Source Sentences
  - No complete explanation, but assuming the introduction of many short term dependencies to the dataset
  - Reduce "minimal time lag" as first few words in source language are now close to the first few words in target language

## Results
- BLEU
  - Rescoring baseline 100-best with an ensemble of 5 reversed LSTMS(best score): 36.5
  - state of the art (phrase based): 37.0
- Does well on long sentences

## Conclusion
- Contributions:
  1. New model: 2 deep LSTMs for encoding and decoding sequences with limited vocabulary and less optimization
  2. Tackling problem: it works
  3. Performance: better compared to SMT-based systems
    1. reversing greatly improves performance
    2. can translate very long sentences
- Future works:
  - further work can lead to even greater translation accuracies
