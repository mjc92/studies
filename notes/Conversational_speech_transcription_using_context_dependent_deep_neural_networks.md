# Conversational speech transcription using context-dependent deep neural networks 

* Motivation: Trying to apply Context-Dependent Deep-Neural-Network HMM (CD-DNN-HMM) directly to speech-to-text transcription
* Contribution: 
⋅⋅1. CD-DNN-HMM can be used in large-scale tasks 
⋅⋅2. We can reduce error and model size by applying weight sparseness
⋅⋅3. look at MLP and DBN (deep belief network) to see what affects the accuracy of CD-DNN-HMMs 

* Method: 
* Results: 
* Future work: 

## Related work
- Denil et al. Predicting parameters in deep learning [[paper]](https://arxiv.org/pdf/1306.0543v2.pdf) [[notes]]() : Neural network can be reconstructed only using a small number of parameters
- Bucila et al. Model compression [[paper]](https://www.cs.cornell.edu/~caruana/compression.kdd06.pdf) [[notes]]() : A small network with few parameters can be trained to predict the output distribution of a larger network
- Hinton et al. Distilling the Knowledge in a Neural Network [[paper]](https://www.cs.toronto.edu/~hinton/absps/distillation.pdf) [[notes]]() : Introduced "distillation", a concept similar to model compression
- Chen et al. Compressing Neural Networks with the Hashing Trick [[paper]](https://arxiv.org/pdf/1504.04788v1.pdf) [[notes]]() : Introduced "HashedNet", a concept to tie similar parameters and reduce redundancy
- Seide et al. Conversational speech transcription using context-dependent deep neural networks [[paper]](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/CD-DNN-HMM-SWB-Interspeech2011-Pub.pdf) [[notes]]() : Set weights lower than a threshold to zero, up to 2/3 of weights of FFNN can be changed to 0 without accuracy loss
- Grezl et al. Optimizing bottle-neck features for LVCSR [[paper]](http://noel.feld.cvut.cz/speechlab/publications/068_icassp08.pdf) [[notes]]() : Change network architecture by introducing bottleneck layers
- Sainath et al. Low-rank matrix factorization for deep neural network training with high-dimensional output targets [[paper]](http://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=6638949) [[notes]]() : Change network architecture through a low-rank matrix factorization layer
- Wang et al. Small-footprint high-performance deep neural network-based speech recognition using Split-VQ [[paper]](http://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=7178919) [[notes]]() : Combine SVD and vector quantization to compress acoustic models

## Prerequisites
- using SVD to reduce the number of parameters in the network in the context of feedforard DNNs
- Xue et al. Restructuring of deep neural network acoustic models with single value decomposition [[paper]](https://www.microsoft.com/en-us/research/wp-content/uploads/2013/01/svd_v2.pdf) [[notes]]()
- Sak et al. Long short-term memory recurrent neural network architectures for large scale acoustic modeling [[paper]]() [[notes]]() : introduces a **shared recurrent projection matrix** used in this paper

## Methods
- 
![alt tag](https://github.com/mjc92/studies/blob/master/notes/rnn_lowrank.JPG)
