# Key-Value Memory Networks for Directly Reading Documents
- Miller et al. [[paper]](https://arxiv.org/pdf/1606.03126v2.pdf)

* Motivation: 
  - Knowledge bases, though frequently used for question answering, are limited in amount and shape of data compared to documents
* Contribution: 
  1. Key-value memory network(KV-MemNN) that works with either knowledge source (KB or documents)
  2. WIKIMOVIES, a new analysis tool that measures the performance of a QA system when knowledge source is switched from a KB
to unstructured documents
* Method:
  - model learns to use keys to address relevant memories with respect to the question
  - corresponding values of the question are subsequently returned
  - model can encode prior knowledge for the task and leverage complex transforms between keys and values
  - while being trained using standard backprop via s.g.d.
* Results:
  - when tested on WIKIMOVIES, KV-MemNN outperforms the original Memory Network and reduces the gaps between answering from different
  domains
  - on WIKIQA, another benchmark without KB, KV-MemNN reaches state-of-the-art results and surpasses recent attention-based NN models
* Future work: 


## Related work
- Santos et al. 
[[paper]]() 
[[notes]]() 
: state-of-the-art on TRECQA / WIKIQA using CNN

- Yin and Schutze. 
[[paper]]() 
[[notes]]() 
: state-of-the-art on TRECQA / WIKIQA using CNN

- Wang et al. 
[[paper]]() 
[[notes]]() 
: state-of-the-art on TRECQA / WIKIQA using CNN

- Miao et al. 
[[paper]]() 
[[notes]]() 
: state-of-the-art on TRECQA / WIKIQA using RNN

- Hill et al,  
[[paper]](https://arxiv.org/pdf/1503.08895v5) 
[[notes]]
: uses memory networks for reading children's books and answering questions about them

- Weston et al (2016),  
[[paper]](https://arxiv.org/pdf/1503.08895v5) 
[[notes]]
: uses memory networks for complex reasoning over simulated stories

## Prerequisites
- Weston et al, Memory Networks
[[paper]](https://web.eecs.umich.edu/~honglak/naacl2016-dscnn.pdf)
[[notes]]()
: unlike memory networks, our model is trained end-to-end, requires less supervision during traing
  - this is a continuous form of the Memory Network
    - that model was not eas to train via backpropagation, and required supervision at each layer of the network

- Sukhbaatar et al, End-To-End Memory Networks 
[[paper]](https://arxiv.org/pdf/1503.08895v5) 
[[notes]]
: basic model

## Key-Value Memory Networks
- Concept
  - lookup (addressing) stage is based on the key memory
    - key: designed with features to help match it to the question
  - reading (return result) stage is based on the value memory
    - value: designed with features to help match it to the response (=answer)
  - effects
    1. greater flexibility for practitioner to encode prior knowledge about their task
    2. more effective power in the model via transforms between key and value
- Model description
  - At test time:
    - when given query(e.g. question in QA tasks), hops (=iterations of addressing & reading memory) are made
    - at each step, the collected information of memory is cumulatively added to original query to be used for next round
    - at last iteration, the final retrieved context and most recent query are combined as features to predict response
    - in other words, q0 -> (get i0 from memory) -> q1(=q0+i0) -> (get i1) -> q2(=q1+i1) -> (get i2) -> use q2+i2
  - Memory slots defined as **pair of vectors (k1,v1), (k2,v2), ... , (kM,vM)**
  - Addressing and reading of the memory
    - Key Hashing
      - from question, select small subsets that share at least one word with the question ignoring stop words
    - Key Addressing
      - each candidate memory get probability of relevance compared to question
    - Value Reading
      - at final reading step, the values of selected memories are read by taking a weighted sum (x relevance prob)

- Model structure
![alt tag](https://lh5.googleusercontent.com/ZJAEgKnmkK9wy17PX1YGQ7V1NNx6vFQuSI3-lp4xvWMPSDApq8ki6U0LswZsUaW9gVRfhb3nqyqxxoA=w958-h1050-rw)
- Weight tying scheme
  1. adjacent
  2. layer-wise (RNN-like)
    - model resembles an RNN where outputs are divided into "internal" and "external" outputs
    - internal output : considering memory
    - external output : predicting a label

## Experimental Setup

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
