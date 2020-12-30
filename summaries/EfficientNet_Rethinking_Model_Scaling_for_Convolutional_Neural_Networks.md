[EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks
](https://arxiv.org/abs/1905.11946)

#### Abstract
paper propose that carefully balancing network depth, width, and resolution can lead to better performance.
paper proposes new compound scaling method which uniformly scales all dimensions of
depth/width/resolution. authors proposed  neural architecture search to design EfficientNets.

#### Introduction
Process of scaling up ConvNets has never been well understood and there are currently many
ways to do it. The most common way is to scale up ConvNets by their depth and widths.
balance all dimensions of network width/depth/resolution can be achieved by simply scaling each
of them with constant ratio rather than previous work which focuses on only one dimension.
For example, if we want to use 2^N times more computational resources, then we can simply increase the network depth by
α^N , width by β^N , and image size by γ^N , where α, β, γ are constant coefficients determined by a small grid search on
the original small model.

#### Related Work
Due to over parameterization of Deep ConvNets,model compression  is a common way to reduce model size by trading
accuracy for efficiency. due to popularity of using NAS  in designing efficient mobile-size ConvNets, same method is
followed for larger models in this paper. 

There are many ways to scale like depth and width scaling.It is also well-recognized that bigger
input image size will help accuracy with the overhead of more FLOPS.

#### Problem 

Unlike regular ConvNet designs that mostly focus on finding the best layer architecture Fi
, model scaling tries to expand the network length (Li), width (Ci), and/or resolution
(Hi, Wi) without changing.

instead of scaling in one direction, paper tries to  maximize the model accuracy
for any given resource constraint as optimization problem by finding coefficients.

since all this coefficients depend on each other and the values change under different
resource constrains, conventional methods mainly focus on one direction:

Depth: Scaling network depth is the most common way used by many ConvNets.
However, scaling a baseline model with different depth coefficient d, further suggesting the diminishing 
accuracy return for very deep ConvNets.
Width: Scaling network width is commonly used for small size models.
Wider networks tend to be able to capture more fine-grained features and are easier to train.
However, extremely wide but shallow networks tend to have difficulties in capturing higher level features.

#### Compound Scaling
With increasing resolution, network height and width should be increased to capture higher receptive field.
Therefore, paper proposes a new compound scaling method, which use a compound coefficient φ to uniformly scales
network width, depth, and resolution in a principled way where coefficients are searched
by grid search as per available resource.

####  EfficientNet Architectur

Since model scaling does not change layer operator, strong baseline is required and hence 
EfficientNet, a multi-objective neural architecture search that optimizes both accuracy and FLOPS is used.
after finiding baseline B0, two approaches in which 
- first fix  φ = 1 and searching for optimum  α, β, y 
- fix α, β, y and scale up with φ.
