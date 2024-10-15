## Attention is all you need

[paper address](https://proceedings.neurips.cc/paper/2017/hash/3f5ee243547dee91fbd053c1c4a845aa-Abstract.html)

#### important concept

use stacked self-attention and point-wise, fully connected layers for both the encoder and decoder

<img src="D:/notes/pic/transformer架构图.png" alt="transformer structure" style="zoom:75%;" />



##### encoder

stack of N = 6 identical layers. Each layer has two sub-layer. 

* The first is a multi-head self-attention mechanism

* second is a simple, position-wise fully connected feed-forward network

employ **residual connection** around each of the two sub-layers, followed by **layer normalization**

To facilitate the connections, all the outputs dimension are the same: $d_(embedding) = d_(model) = 512$

##### decoder

also stack of N = 6 identical layers.

* insert a sub-layer between the two layers, which performs multi-head attention over the output of the encoder stack.
* self-attention is masked to prevent positions from attending to subsequent positions.
* the masking ensure that the predictions for position $i$  can depend only on the known outputs at positions less than $i$



#### Attention

Scaled Dot-Product Attention

There are three parameterized matrix that are intend to learn the attention from the text: $M_Q, M_K, M_V$

As the name, embedding or sequence are multiplied with the above three matrix to generate middle feature:
$$
E_Q = Embedding * M_Q\\
E_K = Embedding * M_K\\
E_V = Embedding * M_V
$$


The $Q , K$ are multiplied to generate the attention matrix and masked
$$
Mask(Q K^T)
$$
 then it would scaled by the $1/\sqrt{d_k}$, applied the softmax to generate the probability, then multiply with V to get final score
$$
Attention(Q, K, V)= Softmax(\frac{QK^T}{\sqrt{d_k}}) V
$$
why scaled? For large values of $d_k$, the dot product attention grow large in magnitude, pushing the softmax function into regions where it has extremely small gradients (same effect as the gradient vanish). So the scaled one can mitigate this effect.

#### multi-head attention

instead of performing a single attention function, linearly project the queries, keys and values $h$ times with different, learned linear projections to $d_k, d_k, d_v$ dimensions respectively. And then perform the attention function in parallel, yielding $d_V$ dimension output values. They would be concatenated and once again projected, resulting in the final values.

employ $h = 8$ parallel attention layers or heads. for each $d_k = d_v = d_(model) / h = 64$ 

#### Implementation details

* encoder-decoder attention layers: $Q$ come from the previous **decoder layer**, $K, V$ come from output of the **encoder**
* encoder layers, the self-attention would let the each position to attend all positions in the previous layer
* decoder layers, the masked attention is required

#### Position-wise Feed-Forward Networks

consists of two linear transformations with a ReLU activation function in between.

the dimensionality of input and output is $d_(model) = 512$,  and the inner-layer has dimensionality $d_(ff) = 2048$

**share** the same weight matrix between two embedding layers and the pre-softmax linear transformation.





 



































