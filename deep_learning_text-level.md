# Deep learning in text-level

- Inspired by work of Yunjey Choi (https://github.com/yunjey/deeplearning-papers#deeplearning-papers-2016119-)

## Paper lists

### Neural Machine Translation
- Sutskever et al. Sequence-to-sequence learning with neural networks
[[paper]](http://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks.pdf) 
[[notes]](https://github.com/mjc92/studies/blob/master/notes/Sequence_to_sequence_learning_with_neural_networks.md) 
    - End-to-end approach to sequence learning by using a multilayer LSTM to map input sequence to a fixed vector,
  and another LSTM to decode the fixed vector into a target sequence

- Bahdanau et al. Neural machine translation by jointly learning to align and translate
[[paper]](https://arxiv.org/pdf/1409.0473v7.pdf) 
[[notes]](https://github.com/mjc92/studies/blob/master/notes/Neural_Machine_translation_by_Jointly_Learning_to_Align_and_Translate.md) 
  - Attentional Neural Machine Translation
    - first introduces term "attention"
    - uses bidirectional RNN
  
- Lee et al. Fully character-level neural machine translation without explicit segmentation
[[paper]](https://arxiv.org/pdf/1603.06147v4.pdf)
[[notes]](https://github.com/mjc92/studies/blob/master/notes/Fully_Character-level_Neural_Machine_Translation_without_Explicit_Segmentation.md)
  - Displays character-to-character NMT model without explicit segmentation

### Text classification
- Kim, Convolutional Neural Networks for Sentence Classification
[[paper]](https://arxiv.org/pdf/1408.5882v2.pdf)
[[notes]]()
  - very simple form of text CNN
  - still, it worked

- Lai et al, Recurrent Convolutional Neural Networks for Text Classification
[[paper]](http://www.aaai.org/ocs/index.php/AAAI/AAAI15/paper/download/9745/9552)
[[notes]](https://github.com/mjc92/studies/blob/master/notes/Recurrent_convolutional_neural_networks_for_text_classification.md)
  - a form of BiRNN to get short-term context + max-pooling
  - not sure if it can be called RNN + CNN when only one max-pooling layer was used

- Zhou et al, Text Classification Improved by Integrating Bidirectional LSTM with Two-dimensional Max Pooling
[[paper]](https://arxiv.org/pdf/1611.06639v1.pdf)
[[notes]]()
