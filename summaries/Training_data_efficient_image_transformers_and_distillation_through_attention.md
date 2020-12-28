[Training data-efficient image transformers
& distillation through attention](https://arxiv.org/pdf/2012.12877.pdf) <br />

neural networks purely based on attention were shown to address image understanding tasks such as image classification
such as visual transformer which has to be trained on hundreds of millions of images. This paper is based on
convolution free by transformers training on imagenet dataset only.also it introduces teacher-student token based 
distillation strategy specific to transformer only, where previous work used using convolutions. 

- key highlights:
- paper shows transformer based nn can achieve good result with no external data, with less parameters and comparable 
 memory usage.
-  image transformers learn more from a convnet than from a another transformer with distillation.

#### Related work
- hybrid architectures that combine convnets and transformers, including the self-attention mechanism, 
- Visual transformers without using convolution but in this case, pre-training phase on a large volume of curated data
 is required for the learned transformer to be effective. This paper paper contradict this point with training on only
 imagenet date.
 
Main key topics for this paper are <br />
 [The Transformer architecture](https://medium.com/inside-machine-learning/what-is-a-transformer-d07dd1fbec04) and <br />
 [Knowledge Distillation](https://towardsdatascience.com/knowledge-distillation-simplified-dd4973dbc764)
 
#### Visual transformer: overview

- Multi-head Self Attention layers (MSA): <br />
Attention(Q, K, V ) = Softmax(QK.T/√d)V <br />
The attention mechanism is based on a trainable associative memory with (key, value) vector pairs, which is matched
against query vector using inner product and scaled and normalize with softmax function to find weights.
MSA is h self attention head each consisting it's (Q, K, V) matrix and applies linear transformation 
to input X.

#### Transformer block for images
two FFN is added on top of MSA layer, which form Transformer block. Transformer block consists
MSA, Residual connection, BN layer and FFN.<br />
image feeding to network is done same as done in ViT feeding images as if they were a sequence of input tokens.
The fixed-size input RGB image(224x224) is decomposed into a batch of N patches of a fixed size of 16 × 16 pixels
(N = 14 × 14). Each patch is projected with a linear layer that conserves its overall dimension 3 × 16 × 16 = 768

#### Class token
trainable vector, appended to the patch tokens before the first layer, that goes through the transformer layers, and 
is then projected with a linear layer to predict the class. This architecture
forces the self-attention to spread information between the patch tokens and
the class token: at training time the supervision signal comes only from the
class embedding, while the patch tokens are the model’s only variable input.

it is desirable to use a lower training resolution and fine-tune the network at the larger resolution.
when high resolution is used with fine tuning, need to adapt the positional
embeddings, because there are different for different resolution, one for each patch.

####  Distillation through attention
distillation token is added same as class token with patches and it's  objective is to reproduce the (hard) label
predicted by the teacher, instead of true label.
Soft distillation(use label smoothing) minimizes the Kullback-Leibler divergence between the softmax of the teacher and the softmax 
of the student model. 
Hard distillation(doesn't use label smoothing). <br />
it is observed that learned class and distillation tokens converge in last layers with similarity around 0.93. 
