# Deep residual learning for image recognition
- He et al.
[[paper]](https://arxiv.org/abs/1512.03385)
- 2015
- overview: the origin of ResNet, where is the conclusion?
* Motivation: deeper networks suffer from the 'degradation problem'; learning residual functions is easier to optimize
* Contribution: 
  1. deep residual nets are easy to optimize with lower training errors than plain models
  2. increased depth leads to better accuracy; better than previous networks
* Method:
* Results:
  1. best performance in ILSVRC 2015 classification
* Future work: 

## Introduction
- network depth is important
  - vanishing / exploding gradients with deeper layers? ->> these are solvable by batch/layer normalization
- degradation problem
  - accuracy saturates and degrades at deeper networks (20 --> 56)
  - in theory, the 56-layer net should learn at least identity mapping, but in reality performs worse than the 20-layer net
- ResNet
  - H(x) : desired output
  - F(x) := H(x)-x    <----- x : identity, F : residuals
  - we hypothesize that it is easier to optimize the residual mapping F(x) than H(x)

## Related work
- Srivastava et al. Highway networks, Training very deep networks
[[paper]](https://arxiv.org/abs/1505.00387, https://arxiv.org/abs/1507.06228) 
[[notes]]() 
: highway networks have similar structures in that they are shortcuts, but
  - these have gating functions that are data-dependent and have parameters
  - closed gate (approaching 0) == layers in highway networks are non-residual functions

## Model
- Residual learning
  - degradation problem : solver might have difficulty obtaining identity mappings from nonlinear functions
  - identity mappings are unlikely, but if it is easier to obtain identity mappings then we can get better results
- Identity mapping by shortcuts
  - y = W2*(ReLU(W1*x)) + x
    - plain / residual nets have same number of parameters / depth / width / computational cost
    - [identity] : dim (x) = dim (F), y = F(x,Wi) + x
    - [projection] : dim (x) != dim (F), y = F(x,Wi) + W_s*x
- Network architectures
  - plain network : VGG-based
  - Residual network : 
- Implementation
  - image resized, shorter side random sampled in [256,480]
  - 224 x 224 crop sampled from image or horizontal flip + per-pixel mean subtracted
  - resized with 256,480
  - 224*224 crop on image or horizontal flip
  - batch normalization right after each conv & before activation
  - sgd, mini-batch of 256
  - lr : 0.1 and divided by 10 when error plateaus
  - trained for 60*10^4 iters ㄷㄷㄷㄷㄷㄷ
  - weight decay of 0.0001, momentum of 0.9
  - no use of dropout
  - for testing, adopt 10-crop testing

## Datasets and Experimental Setup
	- Experiments
		○ ImageNet classification
			§ plain networks: 34-layer has higher training error than 18-layer
				□ degradation problem sighted
				□ unlikely to be caused by vanishing gradients : trained with BN
					® forward propagated signals have non-zero variances
					® backprop-ed gradients have healthy norms with BN
				□ so deep plain nets have low convergence rates, which impact reducing of the training error
			§ residual networks: 34-layer resnet has lower training error than 18-layer resnet
				□ 34-layer resnet has better performance than 34-layer plain network
				□ 18-resnet is faster than 18-layer plain network
			§ identity vs projection shortcuts:
				□ identity : y = F(x,Wi) + x
				□ projection : y = F(x,Wi) + W_s*x
				□ while projection is better than plain settings, it does not improve performance that much
				□ identity shortcuts are important for not increasing the complexity of bottleneck architectures
			§ deeper bottleneck architectures
				□ stack of 3 layers instead of 2 layers
				□ identity shortcuts are more efficient than projection shortcuts
		○ CIFAR-10 and analysis
			§ CIFAR-10 dataset settings
				□ inputs: 32x32 images, per-pixel mean subtracted
				□ 1st layer : 3x3 conv
				□ 6n layers stack with 3x3 conv
					® feature maps of sizes {32,16,8}
				□ 2n layers for each feature map size
					® filter sizes {16,32,64}
				□ conv with stride of 2
				□ 10-way fully-connected layer -> softmax
				□ total of 6n+2 stacked layers


## Results and Discussion
- Baseline model(CNN-rand) performs poorly
- Even a simple model (CNN-static) performs as well as complex models from other papers

### Multichannel vs Single Channel Models
- Multichannel architecture might prevent overfitting?
  - mixed results...

### Static vs Non-static Representations
- both single and multichannel non-static models can fine-tune its channel to task-at-hand

### Further Observations
- dropout is a good regularizer (2~4% performance boost)

## Conclusion
- Contributions:
  1. New model: a simple CNN with one layer of convolution performs well
  2. Tackling problem: 
  3. Performance: 
- Future works:
