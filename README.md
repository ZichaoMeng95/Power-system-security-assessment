# Power-system-stability-assessment

## Research motivation and contributions<br>
Data-driven methods are faced with serious data imbalance problem when applied to Power system stability assessment (PSSA). Due to the high reliability of the power system, there are only a few unstable samples in the historical data, so it imposes ***big challenge*** on traditional PSSA classification methods:<br>
* Traditional PSSA classification methods, such as decision tree, SVM, generally perform poorly on imbalanced datasets, as they are designed to generalize from majority training dataset, which would pay less attention to rare cases. Consequently, test samples belonging to the minority category are misclassified more often than those belonging to the majority category.<br>

Aiming at the problem of data imbalance caused by the scarcity of small signal unstable samples in power system integrated with renewable energy, a novel PSSA approach is presented. The ***main contributions*** can be summarized as follow:<br>
*	**Auxiliary classifier generative adversarial networks** ([AC-GAN](https://arxiv.org/pdf/1610.09585.pdf)) is proposed here which can effectively learn the distribution characteristics of real data and generate high quality small signal unstable samples in line with the actual situation, thus balancing the dataset. Training strategy including pre-training discriminator, parameter transplantation and batch normalization  is performed to stable and accelerate the training process of PSSA model. <br>

If you are not familiar with **GAN**, there is the papper you [need](https://arxiv.org/pdf/1406.2661.pdf). The structure of GAN is shown as:<br>
![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/ac-gan%20arcitecture.png) 

## My proposed methodology<br>
The architecture of PSSA framework:<br>

![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/Complete%20model%20for%20stability%20assessment.png) 

The model can be divided in two parts:

* imbalanced data augmentation  (constructed by AC-GAN)
* stability classifier (constructed by CNN)

Firstly, the imbalanced dataset is used as the real data for AC-GAN training. Then, the augmentation dataset is generated accordance with the specified category C using the already trained AC-GAN generator, and the balanced dataset is obtained by combining imbalanced dataset and augmentation dataset. Finally, the balanced dataset is used as training samples to train the stability classifier.<br>

## Model training strategy with transfer learning<br>
A parameter fine-tuning method based on steady-state data pre-training is leveraged to solve the over-fitting problem and speed up the training of the overall model. The breif steps are as follows:<br>
* Pre-train the AC-GAN discriminator
* Parameter transplantation in tability classifier.
* Training process optimization through Batch Normalization

If you are not familiar with **transfer learning**, there is the papper you [need](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=5288526). 

## Incremental learning by detecting the [conception drift](http://sweet.ua.pt/gladys/Papers/ADMA_GamaCastillo_06.pdf):<br>
![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/Scheme%20of%20the%20incremental%20learning..png) 

## Dataset<br>
* A standard IEEE 33 bus model is built in PSCAD/EMTDC for data acquisition:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/33-bus%20multi-source%20distribution%20network.png) 

* The second harmonic signals of negative sequence voltage and current measured at bus 22 when two-phase short circuit occurs at bus 6 and transition resistance is 0.01Ohm during simulation time of 0.4s~0.7s:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/Second%20harmonic%20signals%20of%20negative%20sequence%20voltage%20and%20current%20measured%20at%20bus%2022.png) 

In this way, fault data and steady-state data under various operation conditions can be collected, and ensure there is **no intersection** between training samples and testing samples.<br>

## Simulation results<br>
* Fault data estimation results:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/Comparison%20of%20%20CTN%20fault%20data%20estimation%20results.png)

* Fault data augmentation results under different training epochs:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/AC-GAN%20data%20generation%20results%20when%20C%3D1%20during%20training%20process.png)

* Visualization of AC-GAN discriminator output features:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/Visualization%20of%20discriminator%20output%20features%20of%20AC-GAN.png)

* Curves of various errors and fault location accuracy with our without training strategy:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/training%20strategy.png)

* The influence of convolution structure change on fault location accuracy:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/structure%20change.png)

* The influence of data normalization on fault location accuracy:<br>
![](https://github.com/ZichaoMeng95/Fault-location-in-distribution-network/blob/master/images/normalization.png)

