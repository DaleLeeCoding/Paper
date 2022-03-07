# A Simple Framework for Contrastive Learning of Visual Representations

[A Simple Framework for Contrastive Learning of Visual Representations](https://arxiv.org/abs/2002.05709)

# Contrastive Learning

Contrastive methods aim to learn representations by enforcing **similar elements to be equal** and **dissimilar elements to be different**.
![Untitled](https://user-images.githubusercontent.com/69357689/156972258-0eb63be0-d2ed-4ef6-8abe-56e2d7c37084.png)


## Representations
![Untitled 1](https://user-images.githubusercontent.com/69357689/156972348-9e44ebef-c017-4ca2-adb9-e5d4e218ca46.png)


## Similarity
![Untitled 2](https://user-images.githubusercontent.com/69357689/156972358-04bd1d66-8f37-4ebb-908e-ecb1e61b9a78.png)


## Loss
![Untitled 3](https://user-images.githubusercontent.com/69357689/156972364-9b09936d-cfd0-4715-99c1-abf4e36fe47c.png)


x+ is a positive example and x- is a negative example

sim(.) is a similarity function

Note that each positive pair (x,x+) we have a set of K negatives
![Untitled 4](https://user-images.githubusercontent.com/69357689/156972395-5508c5ae-5406-4b3c-9bad-b6bbc6749a61.png)
![Untitled 5](https://user-images.githubusercontent.com/69357689/156972400-b6661226-7f63-4680-8930-29a552d3e56b.png)


# Overview

A **stochastic data augmentation** module that transforms any given data example randomly resulting in two correlated views of the same example.

- Random crop and resize(with random flip), color distortions, and Gaussian blur

**No explicit negative sampling.** 2(N-1) augmented examples within a minibatch are used for negative samples. (N is a batch size)

Normalized temperature-scaled cross entropy(NT-Xent) loss is used.

LARS optimizer is used.

To evaluate the learned representations, **linear evaluation protocol(Freeze + one linear layer)** is
used.

Many existing approaches define contrastive prediction tasks **by changing architecture.** 

**But** the authors use only simple data augmentation methods, this simple design choice conveniently decouples the predictive task from other components such as the NN architecture.

## Augmentation
![Untitled 6](https://user-images.githubusercontent.com/69357689/156972419-8211e53a-9381-4f69-b1a3-65ba0aaa0d4b.png)


**No single transformation suffices to learn good representations**
![Untitled 7](https://user-images.githubusercontent.com/69357689/156972431-428a8f62-e7b8-4529-8a3b-a84d388d69c8.png)

**Random cropping and random color distortion stands out**


most patches from an image share a similar color distortion
![Untitled 8](https://user-images.githubusercontent.com/69357689/156972443-b43681b0-b7a0-41b7-a4bb-8f47b3e04ab0.png)


**Stronger color augmentation** substantially improves the linear evaluation of the learned unsupervised models.

A sophisticated augmentation policy(such as AutoAugment) does not work better than simple cropping + (stronger) color distortion

Data augmentation that does not yield accuracy benefits for supervised learning can still help considerably with contrastive learning.

### Nonlinear Projection Head

Nonlinear projection is better than a linear projection(+3%) and much better than no projection(>10%)

![Untitled 9](https://user-images.githubusercontent.com/69357689/156972477-1a924596-993d-4f8a-a2c1-1bfd4229fc5b.png)

The hidden layer before the projection head is a better representation than the layer after.

![Untitled 10](https://user-images.githubusercontent.com/69357689/156972486-810dc369-6c04-4132-92b6-e173866e77b2.png)

The importance of using the representation before the nonlinear projection is due to loss of information induced by the contrastive loss. In particular z = g(h) is trained to be invariant to data
transformation.

**t-SNE**
![Untitled 11](https://user-images.githubusercontent.com/69357689/156972496-0616cee3-0352-4ed0-ae5a-67bae33213af.png)


### Loss function

#TOSEARCH

l2 normalization along with temperature effectively weights different examples, and **an appropriate temperature can help the model learn from hard negatives.**

Unlike cross-entropy, **other objective functions do not weigh the negatives** by their relative hardness.

![Untitled 12](https://user-images.githubusercontent.com/69357689/156972506-08832bf4-3db5-4d7d-bfd3-a994e12050da.png)

### Batch size

When the training epochs is small, larger batch size have a significant advantage.

Larger batch sizes provide more negative examples, facilitating convergence, and training longer also provides more negative examples, improving the results.

# Illustrated Example
![Untitled 13](https://user-images.githubusercontent.com/69357689/156972521-db5d4816-599e-4045-b36f-e32c268687fa.png)


# Conclusion

SimCLR differs from standard supervised learning on ImageNet only **in the choice of data augmentation, the use of nonlinear head at the end of the network, and the loss function.**

Composition of data augmentations plays a critical role in defining effective predictive tasks.

**Nonlinear transformation between the representation** and the contrastive loss substantially improves the quality of the learned representations. #TOQUESTION

Contrastive learning benefits from **larger batch sizes and more training steps** compared to supervised learning.

---

ref)

[https://www.youtube.com/watch?v=FWhM3juUM6s&ab_channel=JinWonLee](https://www.youtube.com/watch?v=FWhM3juUM6s&ab_channel=JinWonLee)

[https://www.slideshare.net/JinwonLee9/pr231-a-simple-framework-for-contrastive-learning-of-visual-representations](https://www.slideshare.net/JinwonLee9/pr231-a-simple-framework-for-contrastive-learning-of-visual-representations)
