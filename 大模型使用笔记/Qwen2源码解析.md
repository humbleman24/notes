# Qwen2 源码解析 2

<img src="..\pic\Qwen2结构图.jpg" alt="Qwen2" style="zoom: 33%;" />

获取Qwen2的config，Qwen2.5的config是与2一致的

```
Qwen2Config {
  "attention_dropout": 0.0,
  "hidden_act": "silu",
  "hidden_size": 4096,
  "initializer_range": 0.02,           用于初始化所有权重矩阵的截断正态初始化器的标准差。
  "intermediate_size": 22016,          MLP表示的维度。
  "max_position_embeddings": 32768,    模型可能使用的最大序列长度。
  "max_window_layers": 28,        SWA（滑动窗口注意力）的层数。底部使用SWA，而顶部使用全部注意力。
  "model_type": "qwen2",
  "num_attention_heads": 32,
  "num_hidden_layers": 32,
  "num_key_value_heads": 32,
  "rms_norm_eps": 1e-06,               rms规范化层使用的epsilon
  "rope_scaling": null,
  "rope_theta": 10000.0,
  "sliding_window": null,
  "tie_word_embeddings": false,
  "transformers_version": "4.45.2",
  "use_cache": true,
  "use_sliding_window": false,        是否使用滑动窗口注意力
  "vocab_size": 151936                Qwen2模型的词汇表大小
}
```

### Qwen2Model forward

首先进来就是把inputs_ids进行向量化，然后拿到hidden_states。然后是存起来所有的hidden_states进入decoder_layer再拿一个hidden_states，作为下一轮decoder_layer的hidden_states输入，最后给hidden_states norm一下。

#### Qwen2Model embedding

**Qwen2RotaryEmbedding**（旋转位置编码）核心思想是对每个词嵌入向量的各个维度进行旋转操作。

​	（最简单的）：假设有一个词嵌入向量X，其前一半的维度和后一半的维度分别做旋转处理（通过余弦和正弦函数），从而编码位置信息。这种旋转操作使得每个词与相邻词语的相对位置编码自然嵌入在它们的关系中，不需要显示的位置编码。

​		Qwen的实现：对于token embeddings，根据其位置信息进行旋转变换。具体来说，对于每一个token的位置，生成一个旋转矩阵，然后将这个旋转矩阵应用于token embedding。这样不同位置的token就会有不同的旋转角度，从而实现了位置编码。

```python
import torch
from  torch import nn
class Qwen2RotaryEmbedding(nn.Module):
      def __init__(self,dim = 128 , base = 10000,max_max_position_embeddings = 2048, device = None):
          super().__init__()
          self.dim = dim
          self.base = base
          self.max_position_embeddings = max_position_embeddings    #取自config
          inv_freq = 1.0 / (self.base ** (torch.arange(0,self.dim, 2,dtype = torch.int64).float().to(device))  / self.dim )   
          self.register_buffer("inv_freq", inv_freq, persistent=False)
          self._set_cos_sin_cache(
                 seq_len=max_position_embeddings, device=self.inv_freq.device, dtype=torch.get_default_dtype()
                 )   
      def _set_cos_sin_cache(self, seq_len, device, dtype):
            self.max_seq_len_cached = seq_len
            t = torch.arange(self.max_seq_len_cached, device=device, dtype=torch.int64).type_as(self.inv_freq)
            freqs = torch.outer(t, self.inv_freq)
            # Different from paper, but it uses a different permutation in order to obtain the same calculation
            emb = torch.cat((freqs, freqs), dim=-1)
            self.register_buffer("cos_cached", emb.cos().to(dtype), persistent=False)
            self.register_buffer("sin_cached", emb.sin().to(dtype), persistent=False)   


```

而这个rotary position embedding很显然是要在每一次的self-attention之前引入，来添加位置信息。

#### Qwen2 Decoder layer

hidden_size: 就是输入输出的大小

self_attn: 用于选择使用什么注意力

mlp：一个全连接层，输入输出的大小都是hidden_size

input_layernorm & post_attention_layernorm：执行layernorm的操作

##### Forward function

```python
def forward(self, hidden_state):
    residual = hidden_state
    hidden_state = self.input_layernorm(hidden_states)
    
    # self_attention
    hidden_states, self_attn_weights, present_key_value = self.self_attn(
    	hidden_states = hidden_states,
        attention_mask = attention_mask,
        position_ids = position_ids,
        past_key_value = past_key_value,
        output_attentions = output_attentions,
        use_cache = use_cache,
        **kwargs
    )
    
    hidden_states = residual + hidden_states
    
    residual = hidden_states
    hidden_states = self.post_attention_layernorm(hidden_states)
    hidden_states = self.mlp(hidden_states)
    hidden_states = residual + hidden_states
    
    outputs = (hidden_states,)
    
```

#### Qwen2 Attention

num_attention_heads：多头注意力的头数

head_dim：多头注意力的维度 self.hidden_size // self.num_heads

num_key_value_heads：用于key和value的头数

形成注意力要使用的就是$q_{proj}, k_{proj}, v_{proj}, o_{proj}$ 四个全连接层

```python
 	self.q_proj = nn.Linear(self.hidden_size, self.num_heads * self.head_dim, bias=config.attention_bias)
    self.k_proj = nn.Linear(self.hidden_size, self.num_key_value_heads * self.head_dim, bias=config.attention_bias)
    self.v_proj = nn.Linear(self.hidden_size, self.num_key_value_heads * self.head_dim, bias=config.attention_bias)
    self.o_proj = nn.Linear(self.num_heads * self.head_dim, self.hidden_size, bias=config.attention_bias)
    self.rotary_emb = Qwen2RotaryEmbedding(
        self.head_dim,
        max_position_embeddings=self.max_position_embeddings,
        base=self.rope_theta)
```

##### Attention Forward

首先先对输入做一些初始化操作，比如获取输入维度信息，映射为q,k,v矩阵

```python
# 获取 batch_size 和 seq_len
bsz, q_len, _ = hidden_states.size()
 
# 把 hidden_states 丢入 q_proj、k_proj、v_proj
query_states = self.q_proj(hidden_states)
key_states = self.k_proj(hidden_states)
value_states = self.v_proj(hidden_states)
 
# 把 q_proj、k_proj、v_proj 的输出 reshape 为下一步计算做准备
query_states = query_states.view(bsz, q_len, self.num_heads, self.head_dim).transpose(1, 2)
key_states = key_states.view(bsz, q_len, self.num_key_value_heads, self.head_dim).transpose(1, 2)
value_states = value_states.view(bsz, q_len, self.num_key_value_heads, self.head_dim).transpose(1, 2)
```

然后对q,k添加位置信息，也就是rotray embedding

```python
# 将旋转位置嵌入应用于查询和键张量。使用了旋转位置嵌入的余弦和正弦部分，将它们与查询和键张量相乘，并将结果相加，从而实现旋转位置嵌入的效果
cos, sin = self.rotary_emb(value_states, seq_len=kv_seq_len)
query_states, key_states = apply_rotary_pos_emb(query_states, key_states, cos, sin, position_ids)   # 这里完成位置信息的添加

```

由于设置的num_key_value_head的个数可能与num_head不一致，所以要对key_value进行分组。

为什么要这么设置？

- 被认为可以提高模型的表达能力、增加模型的结构化信息，降低计算复杂度

- 在大规模模型或长序列输入时能显著提高效率。
- 在Efficient Attention 或 Long range attention模型中，通常使用这种方式来优化注意力计算。

```python
 # 首先，它将key_states和value_states重复self.num_key_value_groups次。
key_states = repeat_kv(key_states, self.num_key_value_groups)
value_states = repeat_kv(value_states, self.num_key_value_groups)
```

然后就可以进行在每组之间进行标准的自注意力操作，计算注意力分数

```python
# 然后，使用torch.matmul()函数计算query_states和转置后的key_states之间的矩阵乘法。最后，将结果除以math.sqrt(self.head_dim)进行归一化
attn_weights = torch.matmul(query_states, key_states.transpose(2, 3)) / math.sqrt(self.head_dim)
 
# 然后 attn_weights 加上 attention_mask
attn_weights = attn_weights + attention_mask
 
# softmax + dropout
attn_weights = nn.functional.softmax(attn_weights, dim=-1, dtype=torch.float32).to(query_states.dtype)
attn_weights = nn.functional.dropout(attn_weights, p=self.attention_dropout, training=self.training)
 
# 然后 attn_weights 和 value_states 相乘
attn_output = torch.matmul(attn_weights, value_states)
 
# 然后把 attn_output reshape 为下一步计算做准备
attn_output = attn_output.transpose(1, 2).contiguous()
attn_output = attn_output.reshape(bsz, q_len, self.hidden_size)
 
# 最后把 attn_output 丢入 o_proj
attn_output = self.o_proj(attn_output)
 
# 返回 attn_output、attn_weights、present_key_value
return attn_output, attn_weights, past_key_value
```

#### Qwen2MLP

进来进行一系列的线性变换，然后激活。一般先升维再降维

这里的gate类似于**门控机制**的结构，具体是通过gate_proj和up_proj两个全连接层输出相乘来实现。

`self.gate_proj` 生成的是类似门控信号的向量。（对信息流进行有选择的通过或抑制）

`self.up_proj` 生成的是传递信息的内容向量。（信息流）

```python
self.gate_proj = nn.Linear(self.hidden_size, self.intermediate_size, bias=False)
self.up_proj = nn.Linear(self.hidden_size, self.intermediate_size, bias=False)
self.down_proj = nn.Linear(self.intermediate_size, self.hidden_size, bias=False)
self.act_fn = ACT2FN[config.hidden_act]
    
    def forward(self, x):
        down_proj = self.down_proj(self.act_fn(self.gate_proj(x)) * self.up_proj(x))
        return down_proj
```

#### Qwen2RMSNorm

核心是通过均方根进行归一化：归一化有助于通过确保权重的规模不会变得过大或过小来稳定学习过程。对训练很有用

它不计算均值，仅依赖于每个向量的均方根值（RMS）来归一化输入，简化了计算过程。

```python
self.weight = nn.Parameter(torch.ones(hidden_size))
self.variance_epsilon = eps     #为了防止分母为0
	def forward(self, hidden_states):
        input_dtype = hidden_states.dtype
        hidden_states = hidden_states.to(torch.float32)
        variance = hidden_states.pow(2).mean(-1, keepdim=True)
        # rsqrt求的是平方根的倒数
        hidden_states = hidden_states * torch.rsqrt(variance + self.variance_epsilon)
        return self.weight * hidden_states.to(input_dtype)
```































































