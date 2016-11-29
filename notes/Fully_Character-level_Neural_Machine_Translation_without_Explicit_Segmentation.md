# Fully Character-Level Neural Machine Translation without Explicit Segmentation

- Lee et al. [[paper]](https://arxiv.org/pdf/1610.03017v2.pdf) 

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
[[notes]](https://github.com/mjc92/studies/blob/master/notes/Neural_Machine_translation_by_Jointly_Learning_to_Align_and_Translate.md) 
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
![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/fully_char_NMT_structure.JPG)

## Experimental Settings
- Baselines
  a. bilingual bpe2bpe
  b. bilingual bpe2char
  c. bilingual char2char
  d. multilingual bpe2char
  e. multilingual char2char
- Tasks
  1. bilingual setting (single language pair)
  2. multilingual setting (train single model from all four language pairs)

## Quantitative Analysis
- BLEU score
  a. in bilingual setting, char2char outperforms subword-level baselines
  b. in multilingual setting, char-level encoder outperforms subword-level encoder in all languages
  c. translating at level of characters allows the model to discover shared constructs between languages more efficiently
  d. for char-level translation, multilingual translation exceeds single-pair translation
- Human evaluation
  - humans measure adequacy and fluency of system translation
  - used for test
    1. bilingual bpe2char
    2. bilingual char2char
    3. multilingual char2char
  - multilingual & bilingual char2char models tied for adequacy
  - multilingual char2char has best fluency
  
## Qualitative analysis
- Intra-sentence *code-switching*, or mixed utterances from two or more languages
- char-level is more robust to typos and spelling mistakes
- char-level correctly segments long, rare words
- char-level can correctly detect character patterns and arrive at a correct translation from nonce words (ex. workoliday, friyay)
- char2char models are 35% slower than bpe2char baselines (bilingual char2char takes 2 weeks)

## Conclusion
- Contributions:
  1. New model: fully char2char NMT without explicitly hard-coded knowledge of words
  2. Tackling problem: shows that character-level translation is beneficial
  3. Performance: exceeds bpe2char models in bilingual and multilingual translation
    1. more parameter efficient
    2. can handle intra-sentence code-switching as a result of many-to-one task
  4. Proposed material:
    1. repository :  https://github.com/nyu-dl/dl4mt-c2c (source code, pre-trained models)
- Future works:
  - many-to-one extends to many-to-many
  - model architectures and hyperparameters
