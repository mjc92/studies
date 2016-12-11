# Memory Networks
- Weston et al. [[paper]](https://arxiv.org/pdf/1410.3916v11.pdf) 


* Motivation: 
  - difficult for ML models to read and write to part of a (large) long-term memory component & combine this with inference
  - past knowledge is compressed for RNNs, hard to perform memorization such as copying the input into an output sequence
* Contribution: 
  1. Memory Networks, combine inference-techniques using memory component to neural network
* Method: 
* Results: 
* Future work: 

## Related work
- Graves et al. Neural turing machines   
[[paper]](https://arxiv.org/pdf/1410.5401v2.pdf) 
[[notes]]() 
: also uses a continuous memory representation
  - uses both content and address-based access (ours uses only content-based access)
  - a more complex model (ex. requires sharpening)
  - more abstract operations of sorting and recall are challenges compared to this model which is applied to textual reasoning
- Xu et al. Show, Attend and Tell: Neural Image Caption Generation with Visual Attention  
[[paper]](https://arxiv.org/pdf/1502.03044v3.pdf) 
[[notes]]() 
: Similar in that it uses an attention model
- Zaremba et al. Recurrent neural network regularization
[[paper]](https://arxiv.org/pdf/1409.2329v5.pdf) 
[[notes]]() 
: state-of-the-art for language modeling
  - uses very large LSTMs with Dropout
- Mikolov et al. Learning longer memory in recurrent neural networks
[[paper]](https://arxiv.org/pdf/1412.7753v2.pdf) 
[[notes]]() 
: state-of-the-art for language modeling
  - uses RNNs with diagonal constraints on the weight matrix


## Prerequisites
- Weston et al, Memory Networks
[[paper]](https://web.eecs.umich.edu/~honglak/naacl2016-dscnn.pdf)
[[notes]]()
: unlike memory networks, our model is trained end-to-end, requires less supervision during traing
  - this is a continuous form of the Memory Network
    - that model was not eas to train via backpropagation, and required supervision at each layer of the network
- Bahdanau et al, Neural machine translation by jointly learning to align and translate
[[paper]](복붙)
[[notes]](복붙)
: similar, pllus multiple computational steps performed per output symbol


## Memory Networks
- Structure
  - I: (input feature map) - convert input to the internal feature representation
  - G: (generalization) - updates old memories (for future use) given new input
  - O: (output feature map) - produces a new output (in the feature representation space), given the new input & current memory state
  - R: (response) - converts the output O into the response format desired
![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/end-to-end_mem_network_model.JPG)
- *I* component
  - preprocessing the original input via encoding, parsing etc.
- *G* component
  - use H(x) to obtain the index of the memory to store I(x) -> m_H(x) = I(x)
  - if the input is at character/word level, group inputs by char/word and store each chunk in a memory slot
  - if memory is huge, organize storage by storing memories by entity or topic
  - G and O need not operate on all memories
  - if memory is full, "forgetting" can be implemented with H by choosing which memory to replace (not covered yet)
- *O*
  - read from memory and perform inference (e.g. calculating relevant memories to perform a good response)
- *R*
  - produce final response given O
    - e.g. actual wording of an answer
  - e.g. R can be an RNN that is conditioned on the output of O

## A MemNN implementation for text
- Basic Model
  - I: input text (sentence), stored in memory as original form
    - S(x) returns the next empty memory slot N (N<-N+1)
  - G: just stores original sentence
  - O: finds k supporting memories given x (k~2)
    - k=1: o1=O1(x,m) = argmax s_o(xm_i)
    - k=2: o2=O2(x,m) = argmax s_O([x,m_o1],m1)
    - final output o = [x,m_o1,m_o2]
  - R: simply returns m_ok, or by RNN
    - r = argmax s_R([x,m_o1,m_o2],w) <---- find w
    - scoring functions s_O and s_R have the same form
  - Training
- Word sequences as input
- Efficient memory via hashing
  - hashing words (inefficient)
  - hashing via clustering word embeddings
- Modeling write time
  - 
- Modeling previously unseen words
- Exact matches and unseen words

- Datasets
  - QA tasks consisting of a set of statements, followed by a question whose answer (typically) is a single word
    a. Sam walks into the kitchen.
    b. Sam picks up an apple.
    c. Sam walks into the bedroom.
    d. Sam drops the apple.
    *Q: Where is the apple?*
    **A: Bedroom**
  - Settings
    - there are I<=320 sentences for each example problem; a question sentence *q* and answer *a*
      - maximum 320 sentences for 1 question??
      
- Model Details
  - K=3 hops used with (type1) adjacent weight sharing
  - sentence representation
    ![alt tag](https://github.com/mjc92/studies/blob/master/notes/images/end-to-end_mem_network_sentence_representation.JPG)
  - add temporal encoding
    - m_i = (sig_j) A x_ij + T_A(i) (<--- *i*th row of a matrix T_A that encodes temporal info, learned through training)
  - add random noise (RN) for regularizing T_A
    - at training we randomly add 10% of empty memories to stories 
    
- Baselines
  - MemN2N: ours
  - MemNN : AM+NG+NL Memory Networks from "Towards AI-complete question answering: A set of prerequisite toy tasks"
  - MemNN-WSH : weakly supervised heuristic version of MemNN
  - LSTM : standard LSTM

## Results
- Design choices
  (i) BoW vs Position Encoding (PE)
  (ii) all 20 tasks independently using embedding of d=20 vs jointly training using d=50
  (iii) two phase training (linear start where softmaxes are removed initially) vs training with softmaxes from the start
  (iv) memory hops from 1 to 3
- Interesting points
  - best MemN2N models are close but inferior to supervised models
  - comfortably beats the weakly supervised baseline methods
  - PE improves over BoW
  - linear start (LS) helps avoid local minima
  - using random empty memories (RN) for time index gives small but consistent boost in performance
  - joint training always helps
  - **more computational hops give improved performance**

## Language Modeling Experiments
- predict the next word in a text sequence given the previous words *x*
  - word level instead of sentence level
  - the previous *N* words in the sequence are embedded into memory separately
  - each memory cell holds only a single word -> no need for BoW or linear mapping (sentence representations)
- changes to model
  - *q* is fixed to a constant vector 0.1 without embedding (no question)
  - output softmax predicts which word in vocabulary is next in sequence
  - add ReLU to half of the units in each layer
  - now apply layer-wise (RNN-like) weight sharing
- datasets
  - Penn Tree Bank
  - Text8
- Training details
  - for each mini-batch, measure l2 norm of the whole gradient of all parameters
  - if norm is larger than *L*=50, scale down to have norm *L*
  - early termination (if learning rate drops below 10^-5 or 50 epochs)
  - weights initialized by N(0, 0.05) and batch size=128
- Results
  - compared to RNN, LSTM, and Structurally Constrained Recurrent Nets (SCRN)
  - MemN2N gives lower perplexity on datasets
  - increasing the number of hops helps
    - some hops concentrate only on recent words, other hops have attention over all memory locations
    - two types of hops seem to alternate
 - doesn't decay exponentially like a traditional RNN, but has the same average activation across the entire memory


## Conclusion
- Contributions:
  1. New model: NN with explicit memory and recurrent attention that can be backprop-ed for LM and QA
  2. Tackling problem: no supervision of supporting facts compared to Memory Network, can be used in a wider range
  3. Performance:
    - same performance as Memory Networks
    - better than other baselines with same level of supervision
    - LM: slightly outperforms tuned RNNs and LSTMs of comparable complexity
    - increasing the hop number improves performance
  4. Proposed material:
- Future works:
  - currently unable to exactly match the performance of Memory Networks trained with strong supervision
  - smooth lookups may not scale well to cases where larger memory is required
  - planning to explore multiscale notions of attention or hashing
