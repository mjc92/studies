# TRACKING THE WORLD STATE WITH RECURRENT ENTITY NETWORKS
- Henaff et al. [[paper]](https://openreview.net/pdf?id=rJTKKKqeg)

* Motivation: 
  - a prediction agent should maintain a set of high-level concepts and update only relevant ones when given new information
* Contribution: 
  1. Recurrent Entity Network (EntNet): a new memory-augmented NN that uses a distributed memory and processor
* Results:
  - solves all 20 bAbI question-answering tasks
  - also solves a new reasning task that requires the use of a large number of supporting facts to answer a question
  - can generalize to sequences longer than those seen during training
  - obtains competitive results on the Childrens Book Test and performs best when reading the text in a single pass before
  receiving knowledge of the question (?)
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

## Model
- Overview
  - Three parts
    1. input encoder
    2. dynamic memory
    3. output layer
  - task : Question answering on short stories
    - input: word sequences

- Input encoder
- Dynamic memory
![alt tag](https://lh3.googleusercontent.com/N9n6z01r0DPx0_NkCtkvxgUYonbxUrh54t-8CxfYz36Xjl6iH63SNZvLdIae9YBzzCxYGypAHVPTc_A=w1920-h1045-rw)

- Output layer
![alt tag](https://lh4.googleusercontent.com/hiKTUaNz-T5z5NDgrKItyexVjoByIznl3PyGDsd0m9ZLwUuMNl7aG9ANbs6Sg1ElMvlmSJkjanZnYw4=w1920-h1045-rw)


## Motivating example of operation
- Suppose model is reading a story
  - inputs are natural language sentences
  - required to answer questions about the story it has just read
- model is free to learn the key vectors w_j for each memory j
  - associate a single memory via the key with each entity in the story
    - memory slot corresponding to a person encodes location / carrying objects / people they are with etc.
    - memory changes as new info indicating objects acquired / discarded, changed locations are taken
  - **can encode this choice of memories directly into model (type of prior knowledge)**
    - by tying the weights of key vectors with the embeddings of specific words (pre-define which words are important)
    - ex) given a list of named entities (selected by person), we have the model assign separate memory slots for each entity
  - Example : given 2 sentences
    1. Mary picked up the ball.
    2. Mary went to the garden.
  - 1st sentence: gates corresponding to both "Mary" and "ball" should activate
    - memory entry to Mary indicates she has the ball, and memory entry to 'ball' indicates it's being carried by Mary
  - 2nd sentence:
    - "Mary" entry updates location info that she is in garden, and "ball" is updated as well
      - although not in 2nd sentence, gate corresponding to "ball" can activate due to the content addressing term (s_t x h_j)
    - then the memory will be in a state where it can answer "Where is the ball?" or "Where is Mary?"

## Experiments



## Conclusion
- Contributions:
  1. New model: a model able to track the world state while reading text stories
  2. Tackling problem: to design an AI model which makes predictions of the complex world over long timescales
  3. Performance:
  4. Proposed material: https://github.com/jimfleming/recurrent-entity-networks by Jim Fleming
- Future works:
  - performance dropped when using only 1k bAbI samples instead of 10k
    - needs for a more efficient reasoning model
