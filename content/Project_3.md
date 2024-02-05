+++
title = "Image-To-Prompt Model Evaluation"
template = "page.html"
date = 2023-05-01T15:00:00Z
[taxonomies]
tags = ["neural network", "LLM", "Images"]
[extra]
summary = "A study that explores the possibility of reversing the relationship between text and images using advanced Vision-Language Pre-training (VLP) frameworks"
mathjax = "tex-mml"
+++

<!-- more -->

## Project Motivation
This study explores the possibility of reversing the relationship between text and image using advanced Vision-Language Pre-training (VLP) frameworks, such as the BLIP and ViT models. We examined a dataset of 7,000 images, randomly selected from DiffusionDB, which features a wide range of prompt-image associations. By leveraging transfer learning and pre-training techniques, we fine-tuned the models and assessed their performance using cosine similarity as the evaluation metric. Our findings demonstrate that the fine-tuned BLIP model significantly surpasses the zero-shot baseline, showing a 25% improvement in average cosine similarity scores on the test set. While the model effectively describes image content, it faces challenges in understanding context, object relationships, and interpreting idioms or metaphors. We also acknowledge the limitations of cosine similarity as an evaluation metric, as it does not consider the sensibility of phrases. Future research should investigate alternative metrics and additional fine-tuning methods to enhance performance on complex and abstract image-text relationships.

## Methodology
In our quest for optimal model performance, we assessed various architectures and fine-tuned hyperparameters to maximize cosine similarity between generated and reference texts. We are using BERT-based embeddings to encode our predicted and actual prompts to derive semantically meaningful sentence embeddings that can then be compared using cosine-similarity. We experimented with two state-of-the-art architectures, leveraging pre-trained models as a solid foundation. We investigated the impact of learning rate, epochs, and batch size on performance. Given computational constraints, the scope of the search space and default hyperparameter values were motived by prior research conducted by <cite>Li et al.[^1]</cite> See below table for hyperparameter search space:

![Table 2 Hyperparameter Search Space](/pictures/figure_4.png)

After fine-tuning hyperparameters on the validation set, we evaluated generalizability on the test data using cosine similarity as the success metric.

[^1]: [Li et al.](https://arxiv.org/pdf/2301.12597.pdf)

## Experiment Results
Our study compared the performance of two state-of-the-art image-to-text models, namely the Bootstrapping Language-Image Pre-Training (BLIP) model and the Visual Transformer (ViT) model combined with a GPT-2 decoder. We evaluated their performance on the task of generating text prompts for images using cosine similarity as the success metric. 

### BLIP Model
Based on our findings, the BLIP model performed the best with an epoch rate of 15, learning
rate of 1e-4 and a batch size of 6 achieving an average validation cosine similarity of ~0.39. While our
search space is not exhaustive, it would appear that the learning rate was the most influential in
determining the validation cosine similarity for the model. Please refer to Table 6 in Appendix for the
results of hyperparameter tuning using random search. After finding the optimal hyperparameters, we
compared the cosine similarity of the zero-shot model (baseline performance) and the fine-tuned model
at each epoch and observed an ~18% higher cosine similarity score for the test set using the fine-tuned
model as illustrated in Figure 15. However, it is worth mentioning that the current model could
potentially suffer from overfitting: the lack of significant improvement in validation data loss (especially
during later epochs) signals that the model isn’t generalizing well to unseen data, and the divergence in
cosine similarity scores between train and validation sets signal that the model could be overfitting.
Please refer to Figure 15 in Appendix for more details about training and validation loss as well as the
cosine similarity score at each epoch. To account for this shortcoming, the batch size, training set size,
learning rate, number of epochs, and other hyperparameters such as dropout rate can be modified as
part of the fine-tuning process in a future study. In Table 3, a number of example predictions with varying cosine similarity scores are presented
that showcase the nature of predicted prompts with BLIP.

![Table 3 Example prompt predictions using fine-tuned BLIP model](/pictures/figure_5.png)

### ViT Model
The ViT Model performed the best with a learning rate of around 1e-4, 10 epochs, and 8
samples per patch, achieving a final average cosine similarity of 0.46. For the ViT model, as with the BLIP model, the learning rate is an important variable, along with the number of samples per training batch. In Figure 16 in appendix, we compare the cosine similarity of
the zero-shot model (baseline performance) and the fine-tuned model at each epoch. In tuning
hyperparameters for the ViT-GPT model, we used a Weights&Biases sweep to visualize the distribution
of validation cosine similarity. In Figure 10, we show the distribution of validation cosine similarity
scores as a function of each model’s learning rate, number of training epochs, and batch size:
demonstrating that the formula to a high cosine similarity score is enough epochs, a high batch size
and a moderate level of learning rate. In Table 4, a number of example predictions with varying cosine similarity scores are presented
that showcase the nature of predicted prompts with ViT.

![Table 3 Example prompt predictions using fine-tuned BLIP model](/pictures/figure_6.png)

## Conclusion
In conclusion, this study explored reversing text-image relationships using
state-of-the-art VLP frameworks, specifically BLIP and ViT-GPT models. The fine-tuned BLIP
model outperformed the zero-shot baseline in image content description but struggled with contextual understanding and object relationships. The ViT-GPT model excelled in recognizing
stylistic elements and generating semantically coherent phrases but faced challenges with
abstract concepts and complex relationships.
The BLIP model identified concrete objects and literal meanings but had difficulties with
context, style, and relationships within images. The average prompt length was shorter than
target prompts, suggesting the model's struggle with complex and abstract images.
Additionally, the model failed to predict idioms or metaphors. Using cosine similarity as a
performance metric had limitations, as it could yield reasonable scores for semantically
nonsensical sentences.
The ViT-GPT model performed better in understanding stylistic elements and producing
semantically complete and grammatically sound responses. However, the predicted prompts
were still shorter on average, and the model struggled with artistic inspirations and complex
abstract notions.
Further research should focus on refining model architectures, optimizing
hyperparameters, employing data augmentation, and exploring alternative metrics to better
capture semantic relevance. Incorporating longer tokenizers and investigating dimensionality
reduction techniques may also prove beneficial.
This study provides valuable insights into VLP frameworks' capabilities and limitations
for reversibility tasks, contributing to computer vision and natural language processing
research. Addressing identified challenges and exploring novel techniques will be essential for
enhancing text-image reversibility and developing more sophisticated models for various
real-world applications.

### Links
[Github link](https://github.com/qu-genesis/image-to-prompt-project/tree/main) <br>
[Detailed report](https://github.com/qu-genesis/image-to-prompt-project/blob/main/30_docs/IDS705_Final_Project_Report_Team1.pdf) 