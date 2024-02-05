+++
title = "Experimentations With Neural Network Acceleration Techniques"
template = "page.html"
date = 2023-08-15T15:00:00Z
[taxonomies]
tags = ["neural network", "experimentation", "pruning", "quantization", "distillation"]
[extra]
summary = "An experimentation on various neural network compression and acceleration techniques to reduce latency while maintaining a high accuracy of models"
mathjax = "tex-mml"
+++

<!-- more -->

### Project Motivation
Large language models have shown remarkable effectiveness in understanding and classifying textual content to identify potential threats within the past few years. However, these models come with significant computational and storage overhead, and finding compression and fine-tuning techniques to reduce the scale and computational cost has become a key challenge for the company to overcome. The primary problem is compressing large language models without compromising their performance capabilities. In this project, we aim to:

* Reduce the latency of models by ~50% without compromising the accuracy of the models (<3% loss)
* Experiment with various techniques and identify methods that work best for BERT and other small LLMs (<=3B parameters)

### Methodology

Our experimentation scope is focused on a few network acceleration techniques, specifically in quantization, pruning, and distillation methods. These are techniques of our specific interest as they can compress the network without interfering with each other, ideally leading to higher compression rates as we try combinations of the methods.

1. **Quantization**<br>
Quantization is the process of lowering the precision of the model parameters to reduce their memory footprint and increase computation speed. Given its ease of implementation and minimal complexity, quantization is a rather popular technique applied to compressing deep learning models. However, the effect of quantization varies significantly depending on the hardware being used, and therefore care must be taken to ensure the right type of quantization is performed for the right hardware setup. In our experiments, the scope was limited to post-training quantization, which is the process of quantizing an already fine-tuned model.

2. **Pruning**<br>
Pruning in the context of neural networks is a crucial technique aimed at reducing the complexity of a model without significantly affecting its performance. This process involves eliminating certain elements of the network, such as weights or neurons, based on specific criteria. There are two primary types of pruning: structured and unstructured. Structured pruning involves removing entire channels or layers from the network, leading to a more compact architecture that can be more efficiently executed on hardware. Unstructured pruning, on the other hand, removes individual weights, resulting in a sparser network. One common method of unstructured pruning is magnitude pruning, where weights with the smallest absolute
values are pruned away, under the assumption that they contribute the least to the network's output. 
After pruning, it's essential to perform fine-tuning, which involves retraining the network on its original task. This step helps the network to recover from any performance loss incurred during pruning and to adapt its remaining weights for optimal performance. Fine-tuning ensures that the pruned network maintains its accuracy and efficiency, making pruning a valuable tool in optimizing neural networks for deployment.

3. **Knowledge Distillation**<br>
In knowledge distillation, information is transferred from a teacher model (often larger and pre-trained) to a student (distilled) model that approximates the original function learned by the teacher network. This is achieved by training the student model using the class probabilities produced by the teacher model as soft targets. When the soft targets have high entropy, they provide more information about the relationship
between the classes than hard targets (one-hot encoded), and the smooth probability distributions help prevent overfitting. The empirical research has shown that the student model can often be trained on less data and less complex architecture to perform similarly to the teacher model.
One of the key challenges in knowledge distillation is finding a suitable student model to train. Ideally, the architecture of the student model is substantially smaller in size than the teacher model while retaining a majority of its performance. For more renowned models like BERT and GPT, student models have been created (e.g. DistilBERT, TinyBERT, DistilGPT-2) using methods like transformer distillation. Several techniques can be used, including transformer distillation, neural architecture search, and pruning-based methods to discover apt student architectures for distillation purposes, but this typically requires a vast amount of computational resources.

### Experiment Results

#### Design Setup
We performed our network acceleration experimentations on two different models: BERT and Bloomz-3b. Distillation was only performed on BERT due to the computational limitations of finding a student model architecture on Bloomz. In addition to individually applying each technique to the models,
a combination of the techniques was also performed such as applying both pruning and quantization. This brought our total experiment count to 7 for BERT and 3 for Bloomz (as distillation was not performed).

The below metrics were used to evaluate the effect of these compression techniques.<br>
    i) Training time (seconds) - The time taken to fine-tune the model<br>
    ii) Inference Time (seconds) - The time taken for the model to generate an individual output<br>
    iii) Accuracy (percentage) - The percentage of the validation data correctly classified<br>
    iv) Model Size (Megabytes) - The size of the model when loaded into memory

#### Results
The experiment results on each variation of the network acceleration method are presented in Figure 1. The quantized model yields the highest accuracy (even higher than the baseline) of 28.55% compared to any other method on the BERT model. By reducing the precision of weights, we introduce a form of noise into the model, which could potentially prevent overfitting and thus improve the generalization model
leading to a higher accuracy. Compared to the original model, the quantized model is ~4.6x smaller. The inference time was significantly improved by using a combination of distillation and pruning to achieve a ~6.2x improvement in the prediction time.

![Figure 1 Experiement Results for BERT and Bloomz](/pictures/figure_3.png)

From the table, we can clearly see that Quantization has the highest model compression rate compared to
other techniques, with a minimal decrease in the accuracy for BERT. We are still working through the
experimentations with the LLM model, but so far it would appear that we are not yielding similar gains in
accuracy with quantization for LLMs. Other combinations of compression techniques did not show
significant gains in inference time or model size reduction.

### Links
[Github link](https://github.com/celisa/LLM-Experimentation-Capstone) - still a WIP
