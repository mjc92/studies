# Network compression

- Categorization based on work of lifeiteng (https://github.com/lifeiteng/FasterDNN)
- Inspired by work of Yunjey Choi (https://github.com/yunjey/deeplearning-papers#deeplearning-papers-2016119-)




| Paper         | Year  | Matrix Factorization | Parameter sharing | Pruning | Parameter quantization | S/W | H/W | DNN type |
| :-----------: |:-----:|:--------------------:|:-----------------:|:-------:|:----------------------:|:---:|:---:|:--------:|
| Predicting parameters in deep learning | 2013 | | | | | | | **CNN** | 
| On the compression of recurrent neural networks with an application to LVSCR acoustic modeling for embedded speech recognition [[paper]](https://arxiv.org/pdf/1603.08042.pdf) [[notes]](https://github.com/mjc92/studies/blob/master/notes/On_the_compression_of_recurrent_neural_networks_with_an_application_to_lvcsr_acoustic_modeling_for_embedded_speech_recognition.md) | 2016 | O | O | | | | | **RNN** |
| Neural networks with few multiplications [[paper]](https://arxiv.org/pdf/1510.03009v3.pdf) [[notes]](https://github.com/mjc92/studies/blob/master/notes/Neural_networks_with_few_multiplications.md) | 2016 |  | | | O | | | **CNN** |
| Recurrent neural networks with limited numerical precision [[paper]](https://arxiv.org/pdf/1608.06902v1.pdf) [[notes]](https://github.com/mjc92/studies/blob/master/notes/Recurrent_neural_networks_with_limited_numerical_precision.md) | 2016 |  | | |O| | | **RNN** |
| Quantized convolutional neural networks for mobile devices [[paper]](https://arxiv.org/pdf/1512.06473v3.pdf) [[notes]](https://github.com/mjc92/studies/blob/master/notes/Quantized_convolutional_neural_networks_for_mobile_devices.md) | 2016 |  | | |O| | | **CNN** |
| New paper | 2016 | | | | | | | **CNN** |

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

##Theory
- Predicting parameters in deep learning. Denil et al. **NIPS 2013**
- Exploiting linear structure within convolutional networks for efficient evaluation. Denton et al (Y.LeCun). **NIPS 2014**
- Speeding up convolutional neural networks with low rank expansions. Jaderberg et al. **BMVC 2014**
- Compressing neural networks with the hashing trick. Chen et al. **ICML 2015**
- Deep Compression: compressing deep neural networks with pruning, trained quantization and Huffman coding. Han et al. **ICLR 2016**
- Neural networks with few multiplications. Lin et al (Y.Bengio) **ICLR 2016**
- EIE: efficient inference engine on compressed deep neural network. Han et al. **ISCA 2016**
- Recurrent neural networks with limited numerical precision. Ott et al (Y.Bengio). **NIPS 2016 EMDNN**

##Applications
- Quantized convolutional neural networks for mobile devices. Wu et al. 2015.
- On the compression of recurrent neural networks with an application to LVCSR acoustic modeling for embedded speech recognition.
Prabhavalkar et al. 2016

##To be seen soon
- SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size. Iandola et al (S.Han). 2016.
- Trained ternary quantization. Zhu et al (S.Han). 2017
