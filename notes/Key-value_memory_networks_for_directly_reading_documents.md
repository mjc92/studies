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
![alt tag](https://lh4.googleusercontent.com/QW4GtaLuQ_HXpHSjv-O_xHYoqRzkEneyMKkLahQSDPRNlbQnRApxer9uvnlvVNhUPc14sjEqVR6Ifxk=w1484-h1043-rw)

- Key-Value Memories
  - Variations in defining transformation matrices for X, Y, K and V
    - During experiments they fixed X and Y as bag-of-words
  - KB Triple
    - knowledge bases have this "subject, relation, object" structure
    - for a standard MemNN the whole triple has to be encoded into the same memory slot
  - Sentence level
    - split a document into sentences, and each sentence is encoded into one memory slot
    - both key and value encode the entire sentence as a bag-of-words
    - this is identical to a standard MemNN
  - Window level
    - split a document into windows of W words (here, only include windows where center word is an entity)
    - windows are represented using bag-of-words
    - however, in Key-Value MemNNs we encode the key as the entire window, and the value as only the center word
      - entire window is more likely to be pertinent as a match for the question, whereas the center entity is a match for the answer
  - Window + Center encoding
    - to prevent mixing the window center with the rest of the window
    - double the size D of the dictionary and encode the center of the window and the value using the 2nd dictionary(?)
    - model can pick out the relevance of the window center (more related to answer) as compared to side words (more related to q)
  - Window + Title
    - the title of a document can represent an answer to a given question
    - here, the key is the word window, but the value is the document title
      - plus, we also keep all the standard [window,center] key-pair values from the window-level representation as well


## The WikiMovies Benchmark
- Overview
  - question-answer task dataset
- Knowledge representations
  1. Doc: raw Wikipedia documents
  2. KB: graph-based knowledge base created from the Open Movie Database
  3. IE: information extraction performed on Wikipedia to build a new KB
    - uses the Stanford NLP Toolkit & SENNA semantic role labeling tool to uncover grammatical structure
- Question-Answer pairs
  - based on SimpleQuestions, created question set by substituting entities in existing questions with those from our KB triples
  
## Experiments
- WikiMovies
  - Compared with 4 approaches
    1. KB-using QA system of Bordes et al.
    2. Supervised embeddings ???(learns question-to-answer embeddings directly)
    3. Memory Networks
    4. Key-Value Memory Networks
  - KV-MemNNs outperform all others on all three data source types (Doc/KB/IE)
  - Window-level + Center-encoding + Title (W=7 / H=2) performs best
  
- QA breakdown
  - nothing much

- KB vs Synthetic Document Analysis
  - to find out differences between performing with raw documents and KBs (KB is much better)
  - create a synthetic document based on KB triples
  - some of the loss is caused because in sentence form it is difficult to extract the subject/relationship/object forms
  - increasing the size of templates does not deteriorate performance

- WikiQA
  - task obj: given a question, select the sentence coming from a Wikipeaid document that best answers the question
    - KB methods cannot be implemented here; only done by reading raw documents
  - KV-MemNN outperform a large set of methods
    - L.D.C method of Wang et al are very similar

## Conclusion
- Contributions:
  1. New model: Memory network that have separate key / value memories
    - can encode prior knowledge about the task at hand in the key and value memories
  2. Tackling problem: it is difficult to solve QA tasks by reading directly from documents, therefore there is a huge
  gap between such direct methods and using KBs
  3. Performance:
    - best performances in WikiMovies and WikiQA
  4. Proposed material: https://github.com/siyuanzhao/key-value-memory-networks
- Future works:
  - application on different tasks
