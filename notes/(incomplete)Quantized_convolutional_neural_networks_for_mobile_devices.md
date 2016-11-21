# Quantized convolutional neural networks for mobile devices
* [[paper]](https://arxiv.org/pdf/1512.06473v3.pdf)
* [[github]](https://github.com/jiaxiang-wu/quantized-cnn)

* Motivation: Limited computation ability and memory space make it difficult to run DNN on mobile devices
* Contribution: Q-CNN model that enables fast test-phase computation while reducing storage and memory consumption
, effective training scheme to suppress the accumulative error while quantizing the whole CNN
* Method: 
* Results: 4~6x speedup and 15~20x compression with less thant 1% accuracy loss, classify an image on mobile device within 1 second
* Future work: 

## Related work
- Kim et al. Bitwise neural networks [[paper]](https://arxiv.org/pdf/1601.06071v1.pdf) [[notes]]() : totally boolean network, but only applies on pre-trained set
- Machado et al. Computational cost reduction in learned transform classifications [[paper]](https://arxiv.org/pdf/1504.06779v2.pdf) [[notes]]() : acceptable accuracy by replacing all floating-point multiplication by integer shifts
- Cheng et al. Training binary multilayer neural networks for image classification using expectation backpropagation [[paper]](https://arxiv.org/pdf/1503.03562v3.pdf) [[notes]]() : DNNs with binary weights can be trained for multi-classification with **expectation backpropagation**

## Prerequisites
- Product quantization works better in approximate nearest neighbor search than hashing-based methods
- Jegou et al. Product quantization for nearest neighbor search [[paper]](https://arxiv.org/pdf/1511.00363v3.pdf) [[notes]]()
: use of product quantization in nearest neighbor search
- Most quantization-based methods focues on fullly-connected layers, and do not improve test-phase computation
- **Our work accelerates and compresses both convolutional and fully-connected layers, and dramatically reduces run-time memory consumption**

## Methods

1. Quantizing the fully-connected layer
2. Quantizing the convolutional layer
3. Quantizing with error correction
 1. Error correction for the fully-connected layer
 2. Error correction for the convolutional layer
 3. Error correction for multiple layers
