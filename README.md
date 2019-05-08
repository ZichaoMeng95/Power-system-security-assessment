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

## Incremental learning by detecting the conception drift<br>
As the operating conditions or network topology keeps changing all the time, the data-driven PSSA model needs to be updated according to the given incremental data in offline mode. An incremental learning method is adopted by leveraging [conception drift](http://sweet.ua.pt/gladys/Papers/ADMA_GamaCastillo_06.pdf):<br>


![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/Scheme%20of%20the%20incremental%20learning..png) 

## Dataset<br>
Voltage angle and magnitude of buses are collected as time serirs samples. 10172 samples  are finally obtained as the imbalanced dataset from New England System with historical data of wind and PV generations, wherein the number of stable samples is 9867 and the number of unstable samples is 305. From the imbalanced dataset, 60% is randomly selected as the training samples, and the remaining 40% is used as the testing samples.<br>

Voltage angles of bus 1 under different minimum damping ratio are shown as:<br>

![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/Voltage%20angle%20of%20bus%201%20under%20different%20damping%20ratio.png) 

## Simulation results<br>
* Change of Data Distribution during AC-GAN Training. (a) training 50 epochs, (b) training 100 epochs, (c) training 200 epochs, (d) training 300 epochs:<br>

![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/Distribution%20of%20generated%20data%20during%20AC-GAN%20training%20process.png)

* Distribution of real data (the imbalanced dataset) is shown in (a); ED, K-LD and PCC during AC-GAN training process are shown in (b):<br>

![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/Distribution%20of%20real%20data%3B%20(b)%20ED%2C%20K-LD%20and%20PCC%20during%20AC-GAN%20training%20process.png)

ED, K-LD and PCC are indicators to show the quality of generated samples.

* visualization of synthesized data distribution with different algorithms. (a) data distribution synthesized by ROS, (b) data distribution synthesized by SMOTE, (c) data distribution synthesized by ADASYN, (d) data distribution synthesized by AC-GAN:<br>

![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/visualization%20of%20synthesized%20data%20distribution%20with%20different%20algorithms.png)

* Curves of various errors and F-measure with our without training strategy. (a) LD, (b) LG, (c) F-measure, (d) training errors of stability classifier:<br>

![](https://github.com/ZichaoMeng95/Power-system-stability-assessment/blob/master/image/Curves%20of%20various%20errors%20and%20F-measure%20with%20our%20without%20training%20strategy.png)

* Comparison of evaluation indexes of different algorithms:<br>

Algorithm  | TPrate/%  | TNrate/% | F-measure/%  | Ac/%
 ---- | ----- | ------ |  ---- | ----- 
 Original dataset  | 99.03|	87.50 |	92.91|	98.39
 ROS  | 96.92 |	97.99 |	97.45 |	97.35
 SMOTE  | 98.64 |	98.72	| 98.68 |	98.67 
 ADASYN	| 98.01 |	**99.81** |	98.91 |	98.75
 AC-GAN  | **99.73** |	99.15 |	**99.44** |	**99.61**



