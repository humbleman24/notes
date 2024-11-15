# ResNet

## 论文学习

### Abstract

Can ease the training of networks that are substantially deeper than those used previously.

explicitly reformulate the layers as **learning residual functions** with reference to the **layer inputs**.

the depth of representations is of central importance for many visual recognition tasks.

### Introduction

problem of vanishing/exploding gradients has been largely addressed by normalised initialization and intermediate normalization layers.

When deeper networks are able to **start converging**, a degradation problem has been exposed: with the network depth increasing, **accuracy gets saturated**, and then **degrades rapidly**. Unexpectedly, such degradation is **not caused by overfitting**, and **adding more layers to a suitably deep model leads to higher training error.**

Addressing the degradation problem by introducing a deep residual learning framework.

### Related Work

#### Residual representations

VLAD and Fisher Vector are both powerful shallow representations for image tasks.

For vector quantization, encoding residual vectors is shown to be more effective than encoding original vectors.

good reformulation or preconditioning can simplicity the optimization.

#### Shortcut Connections

add a linear layer connected from the network input to the output. 

a few intermediate layers are directly connected to auxiliary classifiers for addressing vanishing/exploding gradients.

present shortcut connections with gating functions which is data-dependent and have parameters.

### Deep residual Learning

the degradation problem suggests that the solvers might have difficulties in approximating identity mappings by multiple nonlinear layers.

with the residual learning reformulation, if identity mappings are optimal, the solvers may simply drive the weights of the multiple non-linear layers **toward zero to** approach identity mappings.

learned residual functions in general have small responses, suggesting that identity mappings provide **reasonable preconditioning**.

two options when the dimension changed in non-linear layers:

- the short cut still performs identity mapping, with extra zero entries padded for increasing dimensions
- the projection shortcut is used to match dimensions
