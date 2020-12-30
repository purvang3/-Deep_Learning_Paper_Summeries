[A Style-Based Generator Architecture for Generative Adversarial Network](https://arxiv.org/pdf/1812.04948.pdf)

Authors proposed new generator architecture, same as style transfer literature. Proposed architecture
leads  to an automatically learned, unsupervised separation of high-level attributes  and stochastic variation 
and it enables intuitive, scale-specific control of the synthesis. new generator leads to better interpolation 
properties and better disentangles the latent factors of variation.

#### Introduction
despite recent efforts, the understanding of various aspects of the image synthesis process is still lacking.
also understanding latent space property by latent space interpolations is also not comparable.
proposed generator starts from a learned constant input and adjusts the “style” of
the image at each convolution layer based on the latent code and added noise, therefore directly controlling the 
strength of high-level attributes from stochastic variation at different scales.

generator embeds the input latent code into an intermediate latent space, which has a profound effect on how
the factors of variation are represented in the network. to remove unavoidable entanglement, generator
provided latent space is free from disentangled.

#### Style-based generator
Instead of proving latent vector from feed forward layer, latent code first made to pass through a non-linear mapping
network to get latent space w, which then controls the generator through adaptive instance normalization (AdaIN) and then
Gaussian noise is added after each convolution before evaluating the non-linearity.
In AdaIN operation, each feature map xi is normalized separately, and
then scaled and biased using the corresponding scalar components from style y, which makes 
dimensionality of y double.

In last, Generation of stochastic details is done by  introducing explicit noise
inputs.These are single-channel images consisting of uncorrelated Gaussian noise feed to
each layer of the synthesis network. it is broadcasted to all feature maps using learned perfeature scaling factors and then added to the output of the
corresponding convolution.