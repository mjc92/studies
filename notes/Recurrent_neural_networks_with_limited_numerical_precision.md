# Recurrent neural networks with limited numerical precision
[[paper]](https://arxiv.org/pdf/1608.06902v1.pdf)

* Motivation: RNNs are heavy to be run due to memory and computational power limitations
* Contribution: tried to reduce weight precision during training by limiting numerical precision of the network weights and biases
* Method: weight binarization, ternarization, pow2-ternarization
* Results: Weight binarization doesn't work on RNNs, but stochastic/deterministic ternarization & pow2-ternarization methods work on
low-precision RNNs and produce even higher accuracy
* Future work: 

## Related work
- Rastegari et al. XNOR-Net: ImageNet classification using binary convolutional neural networks 
[[paper]](https://arxiv.org/pdf/1603.05279v4.pdf) 
[[notes]]() : ternarization of weights in CNNs
- Courbariaux and Bengio. BinaryNet: Training deep neural networks with weights and activations constrained to +1 or -1
[[paper]](https://arxiv.org/pdf/1602.02830v3.pdf) 
[[notes]]() : quantization of their **activations**, during training and run-time

## Prerequisites
- Courbariaux et al. BinaryConnect: Training Deep Neural Networks with binary weights during propagations 
[[paper]](https://arxiv.org/pdf/1511.00363v3.pdf) 
[[notes]]() : eliminates multiplications in computing hidden representations by binarizing weights
- Lin et al. Neural Networks with Few Connections 
[[paper]](https://arxiv.org/pdf/1510.03009v3.pdf) 
[[notes]](https://github.com/mjc92/studies/blob/master/notes/Neural_networks_with_few_multiplications.md)
: **introduces TernaryConnect**
- Stromatias et al. Robustness of spiking Deep Belief Networks to noise and reduced bit precision of neuro-inspired hardware platforms
 [[paper]](https://arxiv.org/pdf/1608.06902v1.pdf) 
 [[notes]]() : introduces **pow2-ternarization** used in this paper
- plus their own weight quantization method called **Exponential Quantization**

## Methods
- Basic representations of a convolutional layer, a fully-connected layer, and vector quantization
![alt tag](https://github.com/mjc92/studies/blob/master/notes/vector_quantization_cnn.JPG)

### Quantizing the Fully-connected Layer
![alt tag](https://github.com/mjc92/studies/blob/master/notes/vector_quantization_cnn_fc.JPG)

### Quantizing the convolution Layer
![alt tag](https://github.com/mjc92/studies/blob/master/notes/vector_quantization_cnn_conv.JPG)

### Quantization with Error Correction
- Drawbacks
  1. Minimizing quantization error of model parameters does not necessarily lead to optimal classification accuracy
  (better to minimize estimation error of each layer's output)
  2. Quantization of one error is independent of others and may lead to large error when quantizing multiple layers
- So, we use error correction into quantization of network parameters 
  1. to minimize estimation error of the response at each layer
  2. to compensate the error introduced by previous layers

### Error Correction for the Fully-connected Layer
- use block coordinate descent approach
![alt tag](https://github.com/mjc92/studies/blob/master/notes/error_correction.JPG)
