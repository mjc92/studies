# Fully Character-Level Neural Machine Translation without Explicit Segmentation

* Motivation: NMT systems operate at words and require segmentation to extract tokens
* Contribution: 
  1. character-to-character NMT model without explicit segmentation
  2. single character-level encoder across multiple languages to build multilingual translation system without increasing model size
* Method: Character-level CNN with max-pooling at encoder to reduce length of source
* Results: Char-to-char model outperforms baseline with subword-level encoder, character-level encoder outperforms subword-level encoder
* Future work: Compression techniques can be applied to RNNs in other domains such as machine translation

## Introduction
- word-level NMT models have weaknesses
  1. unable to model out-of-vocabulary words
  2. using a large vocabulary to combat this increases complexity of training and decoding
- advantages of character-level models
  1. better for multilingual translation (word-level requires separate vocabulary for each language)
    a. between European languages, majority of alphabets overlaps, sharing similar meanings
  2. even better scores in BLEU and human evaluation
  3. by not segmenting source sentences into words, the model discovers and internal structure of a sentence by itself
  and how a sequence of symbols can be mapped to a continuous meaning representation

## Related work
- Attentional Neural Machine Translation
- Bahdanau et al. **Neural machine translation by jointly learning to align and translate** (must-read) 
[[paper]](https://arxiv.org/pdf/1409.0473v7.pdf) 
[[notes]]() 
: **Attentional Neural Machine Translation**
  - first introduces term "attention"
  - uses bidirectional RNN
  - well-known example of word-level NMT
- Zhang et al. Character-level Convolutional Networks for Text Classification 
[[paper]](https://arxiv.org/pdf/1509.01626v3.pdf) 
[[notes]]() 
: Applies CNN in text classification
- Costa-Jussa and Fonollosa. Character-based neural machine translation
[[paper]](https://arxiv.org/pdf/1603.00810v3.pdf) 
[[notes]]() 
: replaced word-lookup table with convolutional and highway layers on top of character embeddings,
segmented source and target sentences into words
- Ling et al. Character-based neural machine translation
[[paper]](https://arxiv.org/pdf/1511.04586v1.pdf) 
[[notes]]() 
: bidirectional LSTM to compose character embeddings into word embeddings, at target side another LSTM takes decoder hidden state
and generates target word **character by character**
- Luong and Manning. Achieving open vocabulary neural machine translation with hybrid word-character models
[[paper]](https://arxiv.org/pdf/1604.00788v2.pdf) 
[[notes]]() 
: hybrid scheme that consults char-level information whenever model encounters an OOV word, also implementing char-level NMT model
with 4 layers of unidirectional LSTMs with 512 cells and attention over each character

## Prerequisites
- Chung et al. A Character-Level Decoder without Explicit Segmentation for Neural Machine Translation
[[paper]](https://arxiv.org/pdf/1603.06147v4.pdf) 
[[notes]]() 
: benefits of char-level translation over word-level translation
  1. do not suffer from OOV issues
  2. able to model different, rare morphological variants of a word
  3. do not require segmentation
  - use subword-level encoder and fully character-level decoder (bpe2char)
- Sennrich et al. Neural machine translation of rare words with subword units
[[paper]](https://arxiv.org/pdf/1508.07909v5.pdf) 
[[notes]]() 
: introduces a subword-level NMT model capable of open-vocabulary translation using subword-level segmentation based on
the byte pair encoding (BPE) algorithm
  - starts from character vocabulary
  - identifies frequent character n-grams in tr data and add them to vocabulary
  - ultimately gives a subword vocabulary of words, subwords, characters
  - model performs **subword-to-subword translation** (bpe2bpe)
- Srivastava et al. Training very deep networks
[[paper]](https://arxiv.org/pdf/1507.06228v2.pdf) 
[[notes]]() 
: introduces the highway network that is used to train very deep networks, 
which can be used to improve char-level machine translation using CNNs

## Methods
- Model structure
  - more details required on highway networks, attention models and bidirectional LSTMs
![alt tag](https://github.com/mjc92/studies/blob/master/notes/fully_char_NMT_structure.JPG)