# Neural Networks with few multiplications
* [[paper]](https://arxiv.org/pdf/1510.03009v3.pdf)

* Motivation: Too much computation time spent on floating points
* Contribution: Reduce computation time in floating points
* Method: Binarize weights in hidden states, quantize representations at each layer to change multiplications to binary shifts
* Results: Faster results and even better performance compared to standard stochastic gradient descent
* Future work: Compression techniques can be applied to RNNs in other domains such as machine translation

## Related work
- Kim et al. Bitwise neural networks [[paper]](https://arxiv.org/pdf/1601.06071v1.pdf) [[notes]]() : totally boolean network, but only applies on pre-trained set
- Machado et al. Computational cost reduction in learned transform classifications [[paper]](https://arxiv.org/pdf/1504.06779v2.pdf) [[notes]]() : acceptable accuracy by replacing all floating-point multiplication by integer shifts
- Cheng et al. Training binary multilayer neural networks for image classification using expectation backpropagation [[paper]](https://arxiv.org/pdf/1503.03562v3.pdf) [[notes]]() : DNNs with binary weights can be trained for multi-classification with **expectation backpropagation**

## Prerequisites
- Courbariaux et al. BinaryConnect: Training Deep Neural Networks with binary weights during propagations [[paper]](https://arxiv.org/pdf/1511.00363v3.pdf) [[notes]]()
: eliminates multiplications in computing hidden representations by binarizing weights
- this method deals not only with hidden state computations but also **backward weight updates**

## Methods
- BinaryConnect: reduces multiplications by stochastically sampling weights to be -1 or 1.
- TernaryConnect: reduces multiplications by stochastically sampling weights to be -1, 0 or 1.
- Quantized backpropagation: quantize x(hidden vector) when calculating backprop 
