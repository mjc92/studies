# Residual Networks Behave Like Ensembles of Relatively Shallow Networks
- Veit et al. [[paper]](https://arxiv.org/abs/1605.06431)

* Motivation: 
  - residual networks can be seen as a collection of many paths, and enable deep networks by leveraging short paths during training
* Contribution: 
  1. rewrite ResNet as an explicit collection of paths
  2. lesion study to reveal that these paths show ensemble-like behavior in that they do not strongly depend on each other
  3. most paths are shorter than expected, and only short paths are needed during training as longer ones don't contribute any gradient
* Results:
  - ResNets avoid vanishing gradient by introducing short paths which carry gradient throughout the extent of very deep networks
* Future work: 

## Introduction
- Characteristics of ResNet
  1. they use identity skip-connections to bypass residual layers
  2. skip connections lead to much deeper networks
  3. removing single layers does not affect performance, unlike VGGnet
- This work considers ResNet as a collection of many paths instead of a single deep network
  - paths seem independent to each other
  - conduct lesion study to find out



## Related work
- Highway networks

- Weston et al (2016),  
[[paper]](https://arxiv.org/pdf/1503.08895v5) 
[[notes]]
: uses memory networks for complex reasoning over simulated stories

## Prerequisites
- Residual Networks
- Ensembling

- Weston et al, Memory Networks
[[paper]](https://web.eecs.umich.edu/~honglak/naacl2016-dscnn.pdf)
[[notes]]()
: unlike memory networks, our model is trained end-to-end, requires less supervision during traing
  - this is a continuous form of the Memory Network
    - that model was not eas to train via backpropagation, and required supervision at each layer of the network

## The unraveled view of residual networks
- Overview
  - Three parts
    1. input encoder
    2. dynamic memory
    3. output layer
  - task : Question answering on short stories
    - input: word sequences

- Input encoder
- Dynamic memory
![alt tag](https://lh3.googleusercontent.com/Shd-DcpEOAIjXaonz-HEY60vk698X_6D-PpeJutG3etqJPJRikpoGNPi15BvCeEM3cWCfvznbjPOz6Y=w976-h1074-rw)

- Output layer
![alt tag](https://lh5.googleusercontent.com/bAhXvKhgEJDCHG0UiES5IEhlsIRK1w1Ih2MhK5Ef6woDZU-NaW6ikCMFDPVkEEbNOFWgFrNh5ahVbls=w976-h1074-rw)


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
