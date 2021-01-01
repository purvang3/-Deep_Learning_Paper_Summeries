[Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps
](https://arxiv.org/abs/1312.6034)

Paper addresses the visualisation of image classification models, learnt using deep Convolutional Networks.
Paper proposes two visualization methods:
- first one, which  which maximises the class score, thus visualising the notion of the class.
- Constructing a class saliency map, specific to a given image and class.

#### Introduction
Previous work was to visualised deep models by finding an input image which maximises the neuron activity of interest by carrying out an optimisation using gradient ascent in the
image space.For convolutional layer visualisation, they proposed the Deconvolutional Network (DeconvNet) architecture, which aims to approximately
reconstruct the input of each layer from its output.

#### Class Model Visualisation
Given a learnt classification ConvNet and a class of interest, the visualisation
method consists in numerically generating an image, which is representative of the class in terms
of the ConvNet class scoring model. Here e the optimisation is performed
with respect to the input image, while the weights are fixed to those found during the training stage.
Initialization of Optimization is done with zero image and then calculated training set mean is added and 
unnormalized score is used for back-propagation.

#### Image-Specific Class Saliency Visualisation
Given an image, a class, and a classification ConvNet with the class score function, pixels 
are ranked based on their influence on score. by having all parameters, magnitude
of elements of w defines the importance of the corresponding pixels of I for the class c.
as Sc(score) is composed on highly non-linear operation, for finding w, derivation of Sc with respect
to Image is taken nearby image pixels.

#### Class Saliency Extraction
Given an image and class, class saliency match is computed by first taking e derivative w by back-propagation
and then rearranging w for gray scale image, where number of weights in w is equal to image pixels and then for
each pixel, value of w is choose.

#### Weakly Supervised Object Localisation
This method encode the location of the object of the given class in the given image, and thus can be 
used for object localisation.Foreground and background colour models were set to be the Gaussian Mixture Models. 
The foreground model was estimated from the pixels with the saliency higher than a threshold, set to the 95% quantile 
of the saliency distribution in the image; the background model was estimated from the pixels with the saliency smaller than the 30% quantile.