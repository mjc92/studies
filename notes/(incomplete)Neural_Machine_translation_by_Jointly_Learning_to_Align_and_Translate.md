# Neural Machine translation by Jointly Learning to Align and Translate
- Bahdanau et al.
[[paper]](https://arxiv.org/pdf/1409.0473v7.pdf)
- ICLR 2015

* Motivation: In NMT, encoding a source sentence into a fixed-length vector is a bottleneck
* Contribution: 
  1. End-to-end approach to sequence learning without assumptions
  2. Single character-level encoder across multiple languages to build multilingual translation system without increasing model size
* Method: 
* Results: Translation performance comparable to state-of-the-art in EN-FR translation
  - qualitative analysis reveals that (soft-)alignments found by the model are agreeable
* Future work: 

## Introduction
- When using encoder-decoder approach for NMT, fixing all info of a source sentence into a fixed-length vector
may deteriorate performance
- Se we introduce an encoder-decoder model which learns to align and translate jointly
  - Each time the model generates a word in a translation, it (soft-)searches for a set of posiitons in a source sentence
  where the most relevant information is concentrated
  - The model then predicts a target word based on the context vectors associated with these positions and all previouly generated
  target words
  - Simply put, **ATTENTION**

## Related work
- Encoder-Decoder NMT models
- Sutskever et al. Sequence to sequence learning with neural networks
[[paper]](http://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks.pdf) 
[[notes]](https://github.com/mjc92/studies/blob/master/notes/Sequence_to_sequence_learning_with_neural_networks.md) 
: one example of an encoder-decoder, uses 2 LSTM models
- Cho et al. Learning Phrase Representations using RNN Encoder-Decoder for Statistical Machine Translation
[[paper]](https://arxiv.org/pdf/1406.1078v3.pdf) 
[[notes]]() 
: one example of an encoder-decoder
- Cho et al. On the properties of neural machine translation: Encoder-Decoder approaches
[[paper]]() 
[[notes]]() 
: shows how performance drops when sentences become longer
- Graves et al. Generating Sequences With Recurrent Neural Networks
[[paper]](https://arxiv.org/pdf/1308.0850v5.pdf) 
[[notes]]() 
: used bidirectional RNN(BiRNN) in speech recognition


## Prerequisites
- RNN Encoder-Decoder

## Methods
- bidirectional RNN as encoder
- decoder that emulates searching through a source sentence during decoding a translation

### Decoder: General description
- Use context vector to get annotations to which an encoder maps the input sentence
- Alignment model *a* is a feedforward neural network which is jointly trained with all other components
of the proposed system
- **Here, the alignment is not a latent variable but a soft alignment**
  - allows for gradient backpropagation
- With a decoder having an attention mechanism, the encoder doesn't have to encode all info in one fixed-length vector
 (add decoder_attention_rnn.JPG)

### Encoder: Bidirectional RNN for annotating sequences
- With BiRNN, the annotation of each can summarize not only preceding words but also following words


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
