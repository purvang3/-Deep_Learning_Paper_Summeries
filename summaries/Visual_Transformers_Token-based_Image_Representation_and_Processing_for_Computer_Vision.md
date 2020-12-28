[paper](https://arxiv.org/pdf/2006.03677.pdf)

Convolutions treat all image pixels equally regardless of importance; explicitly model all concepts across all images,
regardless of content; and struggle to relate spatially-distant concepts. This paper uses
- representing images as semantic visual tokens
- running transformers to densely model token relationships.<br />
contrast to pixel-space transformers Visual Transformer operates in a semantic token space.
Normally convolutions uniformly process all
image patches regardless of importance. This leads to spatial inefficiency in both computation and representation.
Finding low level and high level features in all types of images results in rarely-used, inapplicable filters
expending a significant amount of compute. Each convolutional filter is constrained to operate on a small region,
but long-range interactions between semantic concepts is vital and to mitigate this problem,
dilated convolutions, global pooling, and non-local attention layers methods are used at pixel level.

This paper shows pixel_covolution as root cause of some problem and presenting new paradigm to represent and process 
high-level concepts in images with Transformers. instead of using fixed pixel-array, proposed the spatial attention
to convert the feature map into a compact set of semantic tokens and feed to transformer.

Unlike convolutions, our VT can better handle the three challenges above
- judiciously allocating computation by attending to important regions only.
- encoding semantic concepts in a few visual tokens relevant to the image.
- relating spatially distant concepts through self-attention in token-space.

#### Relationship to previous work
Transformers in vision models:  dividing an image into 16 × 16 patches and feeding to transformers requires 
learn dense, repeatable patterns (e.g., textures), which convolutions are
drastically more efficient at learning. but proposed method first use convolutions for extracting low-level features
and transformers for relating high-level concepts. Another network DETR, adopts transformers to
simplify the hand-crafted anchor matching procedure in object detection training.

#### Visual Transformer
first process input with several convolution blocks, then feed the output feature map to VTs.
- early in the network, use convolutions to learn densely-distributed, low-level patterns
-  later in the network, use VTs to learn and relate more sparsely-distributed, higher-order semantic concepts.
At the end of the network, use visual tokens for image level prediction tasks and use the augmented feature map
for pixel-level prediction tasks.

VT module first , group pixels into semantic concepts, to produce a compact set of visual
tokens. then,  to model relationships between semantic concepts, apply a transformer to these visual tokens.
and in the last, project these visual tokens back to pixel-space to obtain an augmented feature map.

#### Tokenizer
As contrasts with convolutions, which use hundreds of filters, and graph convolutions, which use hundreds of
“latent nodes” to detect all possible concepts regardless of image content, paper uses few handfuls of words to 
summarize the words. tokenizer converts feature maps from convolution into visual tokens. It can be also used 
Filter-based Tokenizer where we first apply point wise convolutions, where we map each pixel
to l semantic group, and within each group, we spatially pool pixels to obtain tokens T.
since fixed set of weights models all such high-level concepts at once, Recurrent Tokenizer 
is proposed which uses weights that are dependent on previous layer’s visual tokens.

#### Transformer
interaction between Visual tokens is done with transformers, which use input-dependent weights by design
instead of graph convolution,which uses fixed weights.