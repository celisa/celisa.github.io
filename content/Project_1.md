+++
title = "Neural Network Attack With Adversarial Patches"
template = "page.html"
date = 2023-12-15T15:00:00Z
[taxonomies]
tags = ["neural network", "experimentation", "adversarial attack"]
[extra]
summary = "An experimentation on the effect of adversarial patches on different neural networks"
mathjax = "tex-mml"
+++

<!-- more -->

### Project Motivation
As recent developments in machine learning and deep learning have paved the way for many technological advancements, these models have also become increasingly susceptible to potential adversarial attacks. Adversarial attacks are deliberate and malicious attempts to deceive these models by making subtle alterations to the input data to mislead the neural network into generating a different results.

Adversarial patches are a type of adversarial attacks that involve placing a carefully crafted patch on an image to fool the model in making incorrect classifications. Drawing from <cite>Brown et al.[^1]</cite>'s work, we trained an adversarial patch on the CIFAR-10 dataset with the following goals:

* Measure the effectiveness of untargeted and targeted adversarial patches trained on ResNet-20.
* Explore the sensitivity of the model to different patch sizes
* Assess the transferability of the targeted and untargeted adversarial patches on DenseNet and VGG-16.

[^1]: [Adversarial Patches Brown et al.](https://arxiv.org/pdf/1712.09665.pdf)

## Methodology

Our overall approach is to train both targeted and untargeted adversarial patches of various sizes and test their transferability properties to ResNet and VGG models. The training of adversarial patches is as follows:

1. Initialize a patch that can be applied to any random location on the image
2. Update the patch
* **Untargeted Approach**: the patch is updated through stochastic gradient ascent
$$
\displaylines{\hat{p} = \underset{p}{\mathrm{argmax}}\mathbb{E}_{x \sim X, r \sim R, l \sim L} \left[ CE(\hat{y} | A(p, x, l, t), y) \right]
}
$$

* **Targeted Approach**: labels of all images are assigned to the target class and the patch is updated through stochastic gradient descent

$$
\displaylines{\hat{p} = \underset{p}{\mathrm{argmin}}\mathbb{E}_{x \sim X, r \sim R, l \sim L} \left[ CE(\hat{y} | A(p, x, l, t), y) \right]
}
$$

where $A(p, x, l, t)$ is the patch application operator that takes in the patch $p$, the target image $x$, the location $l$, and transformations (e.g scale, rotations) $t$ and applies a transformed patch to the image at the given location. 

## Experiment Results
We used ResNet-20 to train our untargeted patches. Due to the
modest dimensions of our training images and patches, discernible patterns were not readily apparent
in the patches. However, a consistent color palette emerged across all patches, with shades of purple,
pink, green, and blue appearing in all the patches. To establish a baseline for our ASR, we fine-tuned all
models using the CIFAR-10 dataset and calculated the baseline error rate, representing the validation
error when no patches were applied (see red line in Figure 1). Notably, all patches demonstrated a
substantial improvement over the baseline error rate, underscoring their efficacy in compromising the
neural network with a certain degree of success.

![Figure 1 Untargeted Validation ASR](/pictures/figure_1.png)

Furthermore, our experiment results revealed a positive correlation between patch size and ASR.
This aligns with our intuitive understanding of larger patches covering more extensive areas of the
image, making it easier to deceive the neural network by incorrectly predicting a class. Interestingly,
in all instances, the validation ASR equaled or surpassed the training ASR. This phenomenon may be
attributed to the model’s robustness to perturbations during training, leading to more accurate predictions.
Perhaps, during validation, the model encounters a more diverse set of examples, potentially
making it more susceptible to the adversarial patches.

Using the patches trained on ResNet-20, we also evaluated the ASR on alternative architectures,
namely DenseNet-121 and VGG-16, to measure the generalization capability of the adversarial patches.
The findings, presented in figure 2 for DenseNet and VGG, respectively, indicate a notable degree
of transferability of the patches to these distinct models. With the exception of 7 x 7 patch, the
validation ASRs for DenseNet and VGG were found to be comparable to those achieved in ResNet
suggesting successful transferability of patches to other architectures. One plausible explanation for the
comparatively diminished ASR of the 7 x 7 patch size in both DenseNet and VGG could be attributed
to potential interference with specific network structures. The 7 x 7 patch size might overlap with
critical components of the network, such as convolutional filters or pooling operations, in a way that
disrupts the model’s internal representations. This interference could result in less effective adversarial
attacks.

![Figure 2 Untargeted Validation ASR with DenseNet121 and VGG16](/pictures/figure_2.png)

Similar to untargeted ASR, we used ResNet-20 to train our targeted patches. Given the substantial
number of classes, encompassing a total of 10 categories, we opt for a more focused analysis by
only using results from target class Bird (representing a favorable outcome) and target class Horse
(representing an unfavorable outcome) to illustrate our findings. A comprehensive compilation of all
results is provided in the Supplementary Materials section for curious readers.
Just as we observed with untargeted attacks, a positive correlation is generally discerned between
patch size and ASR, although the strength of this correlation is not as pronounced as encountered in
untargeted attack scenarios. Notably, a relatively linear increase in ASR can be noticed in table 2
when we increase the patch size for target class ‘Bird’. However, in the case of the target class ‘Horse’,
the validation ASR remains consistently low for patch sizes 3x3, 5x5, and 7x7. 
This suggests potential challenges in the model’s ability to effectively capture meaningful patterns
associated with the ‘Horse’ class. Perhaps, the representation of certain classes like horse can possess
more complex patterns or variations that are hard to separate from the rest of the classes. As an
example, distinguishing classes such as ‘dog’, ‘cat’, and ‘deer’ become more intricate in comparison
to horses as they share similar anatomical features in contrast to birds. When comparing the patch
patterns of the ‘Bird’ class and the ‘Horse’ class in figure 3, we notice that the ‘Bird’ class contains a
lot of shades of blue whereas the ‘Horse’ class contains a mix of yellow, blue and red shades. Perhaps,
these might be less effective patterns to adversarially attack the neural network.
When evaluating the ASR on DenseNet and VGG for transferability properties, we observed significant
disparities in the generalization capabilities of targeted patches as opposed to untargeted patches.
For target class ‘Bird’, we obtain ASR values surpassing the baseline for all patch sizes, albeit with
diminishing gains as the patch size increases. Interestingly, the ‘Bird’ patch exhibits a higher ASR on
DenseNet compared to VGG, potentially attributable to the more analogous architectural structures
shared between DenseNet and ResNet. Conversely, the ‘Horse’ patch demonstrates even more inferior
transferability, with validation ASR consistently below 10% for both models across varying patch sizes,
indicative of suboptimal transferability properties. Comparatively, in relation to untargeted attacks,
targeted attacks yield lower ASR and transferability capabilities.

## Conclusion
In this study, we utilized the CIFAR-10 dataset to train adversarial patches, aiming to examine both
the impact of various patch types and the degree to which these trained patches could transfer across
different Deep Neural Network architectures. Our experimentations with untargeted and targeted
attacks were able to surpass baseline error rates highlighting their efficacy. Generally, we observed
a positive correlation between patch size and attack success rate demonstrating that larger patches
relative to the size of the input image have a better ability to deceive the neural network. Evaluations
across DenseNet-121 and VGG-16 showcased notable transferability of adversarial patches for untargeted
attacks. However, we observed relatively inferior transferability properties with targeted attacks
using ‘Bird’ patch and ‘Horse’ patch as examples emphasizing nuanced challenges with targeted attacks
in carrying over adversarial patterns to alternative architectures.

### Links
[Github link](https://github.com/celisa/Adversarial-Patches-Experimentation/tree/main) <br>
[Detailed report](https://drive.google.com/file/d/1IdAYKJg_QPY_8gR_U5qU2TC5uvCPJRJ0/view?usp=sharing) 