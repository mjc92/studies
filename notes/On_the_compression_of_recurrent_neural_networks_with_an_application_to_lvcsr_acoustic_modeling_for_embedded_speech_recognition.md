# On the Compression of Recurrent Neural Networks with and Application to LVCSR Acoustic Modeling for Embedded Speech Recognition

* Motivation: Speech recognition algorithms are too heavy to directly be put "on-device"
* Contribution: Study techniques that can be used to compress RNN acoustic models
* Method: 
* Results: Reduce size of LSTM acoustic model to 1/3 of original size while negligible loss in accuracy
* Future work: Compression techniques can be applied to RNNs in other domains such as machine translation

## Prerequisites
- Denil et al. Predicting parameters in deep learning [[paper]](https://arxiv.org/pdf/1306.0543v2.pdf) [[notes]]() : Neural network can be reconstructed only using a small number of parameters
- Bucila et al. Model compression [[paper]](https://www.cs.cornell.edu/~caruana/compression.kdd06.pdf) [[notes]]() : A small network with few parameters can be trained to predict the output distribution of a larger network
- Hinton et al. Distilling the Knowledge in a Neural Network [[paper]](https://www.cs.toronto.edu/~hinton/absps/distillation.pdf) [[notes]]() : Introduced "distillation", a concept similar to model compression
- Chen et al. Compressing Neural Networks with the Hashing Trick [[paper]](https://arxiv.org/pdf/1504.04788v1.pdf) [[notes]]() : Introduced "HashedNet", a concept to tie similar parameters and reduce redundancy

