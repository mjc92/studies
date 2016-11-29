# Neural Machine Translation of Rare Words with Subword Units
- Sennrich et al. [[paper]](https://arxiv.org/pdf/1508.07909v5.pdf) 

* Motivation: To model open-vocabulary translation in the NMT network without requiring a back-off model for rare words
* Contribution: 
  1. open-vocabulary NMT is possible by encoding (rare) words via subword units to build a simpler and more effective architecture
  2. adapt *byte pair encoding* (BPE), a compression algorithm, for word segmentation. Open vocabulary is made possible through a
  fixed-sized vocabulary of variable-length character sequences, and thus is a suitable word segmentation strategy for NN models.
* Method: 
* Results: 
  1. Reducing the vocabulary size of subword models can improve performance of OOV words and rare in-vocabulary words
* Future work: 
  1. Find optimal vocabulary size fo a translation task depending on language pari and amount of training data, automatically
  2. Improved bilingually informed segmentation algorithms to create more alignable subword units

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
- Bahdanau et al. Neural machine translation by jointly learning to align and translate
[[paper]](https://arxiv.org/pdf/1409.0473v7.pdf) 
[[notes]](https://github.com/mjc92/studies/blob/master/notes/Neural_Machine_translation_by_Jointly_Learning_to_Align_and_Translate.md) 
: Attentional Neural Machine Translation
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
- Jean et al. On Using Very Large Target Vocabulary for Neural Machine Translation
[[paper]](https://arxiv.org/pdf/1412.2007v2.pdf) 
[[notes]]() 
: Dealing with OOV words
  - uses large vocabulary, copying unknown words into the target text
- Luong et al. Better Word Representations with Recursive Neural Networks for Morphology
[[paper]](http://nlp.stanford.edu/~lmthang/data/papers/conll13_morpho.pdf) 
[[notes]]() 
: Introduces technique to produce fixed-length continuous words vectors based on characters or morphemes
- Luong et al. Addressing the Rare Word Problem in Neural Machine Translation
[[paper]](https://arxiv.org/pdf/1410.8206v4.pdf) 
[[notes]]() 
: Dealing with OOV words
  - back-off to a dictionary look-up(?), copying unknown words into the target text

## Prerequisites
- Ling et al. Character-based neural machine translation
[[paper]](https://arxiv.org/pdf/1511.04586v1.pdf) 
[[notes]]() 
: Similar to our work in that it tries to apply word vectors based on characters/morphemes
  - no significant improvement over word-based approaches
  - attention mechanism still operates on the level of words in this model, and representation of each word is fixed-length

### Subword Translation
- Translation of some unknown words is transparent, in that they are translatable based on known subword units such as morphemes
or phonemes
- Examples
  1. Named entities: (Eng) Barack Obama -> (Kor) Buh-Rak-O-Ba-Ma
  2. Cognates (words with same origin) and loanwords (words that originate from other languages)
  3. Words with multiple morphemes : (Eng) solar system -> (Ger) Sonnesystem (Sonne+system)
- In an analysis of 100 rare German words, the majority can be translated to English through smaller tokens

### Byte Pair Encoding (BPE)
- data compression technique that iteratively replaces the most frequent pair of bytes in a sequence with a single, unused byte
- **Instead of merging frequent paris of bytes, we merge characters or character sequences**
- Process
  1. initialize symbol vocabulary with the character vocabulary
  2. represent each word as sequence of characters + end-of-word symbol '.' (to restore original tokenization after translation)
  3. count all symbol pairs and replace each of the most frequent pair('a','b') with a new symbol ('ab')
    - each merge produces a new symbol which represents a character n-gram
    - also merge frequent n-grams into a single symbol
  4. final vocabulary size is the initial vocab size + number of merge operations
- example: "Lower"
  1. 'l' 'o' 'w' 'e' 'r' '.'
  2. 'l' 'o' 'w' 'e' 'r.'
  3. 'lo' 'w' 'e' 'r.'
  4. 'low' 'e' 'r.'
  5. 'low' 'er.'
- Application on NMT
  1. Learning two encodings: one for the source, one for the target vocabulary
    - more compact in vocab size
    - strongly guarantees that each subword unit has been seen in the training text(?)
    - may segment the same name differently in two languages, harder for model to learn a mapping between subword units
  2. Learning the encoding on the union of the two vocabularies (joint BPE)
    - improves consistency between the source and target segmentation

## Evaluation
- Tasks
  1. Improve translation of rare and unseen words in NMT by representing them via subword units
  2. Find best segmentation in terms of vocabulary size, text size, and translation quality

------------------------------------------------------incomplete---------------------------------------------
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
