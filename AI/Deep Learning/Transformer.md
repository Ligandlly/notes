# Transformer

Seq2seq model with "Self-attention"

RNN 不容易被**并行化**，用CNN只能考虑有限的内容

Self-attention layer和双向RNN有一样的能力，而且可以并行计算

![image-20220609202458246](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609202458246.png)

![image-20220609202902974](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609202902974.png)

$W$、$W^q$……都是矩阵



Attention输入为两个向量，结果为两个向量的**匹配程度**

![image-20220609203324099](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609203324099.png)



$\alpha_{1,i}$是数值，$d$是$q$和$k$的维度

为什么要处以$\sqrt d$？

——因为$q$和$k$的维度越大$\alpha$就会越大。Paper里有一个注脚

![image-20220609203834011](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609203834011.png)

**Softmax**层

![image-20220609203955555](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609203955555.png)

如果要$b^1$不考虑$a^3$的信息，就让$\hat{\alpha}_{1,3}=0$。

![image-20220609204828666](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609204828666.png)

$Q=[q^1\ q^2\ q^3 \ q^4]$，$I=[\alpha^1\ \alpha^2 \ \alpha^3 \ \alpha^4 ]$，
$$
Q=W^q I \\
K = W^k I \\
V = W^v I
$$
![image-20220609205114948](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609205114948.png)

![image-20220609205215542](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609205215542.png)

![image-20220609205352133](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609205352133.png)



## Multi-head self-attention

![image-20220609205618535](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609205618535.png)

先让$q^i$乘上两个矩阵$W^{q,1}$、$W^{q,2}$变成$q^{i,1}$、$q^{i,2}$，kv也一样

$q^{i,1}$只会和$k^{i,1}$$k^{j,1}$运算

不同的head关注点不一样，有的head可以看邻居的信息，有的可以看全局的信息

## Positional Encoding

![image-20220609210222180](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220609210222180.png)

Self- attention中输入的顺序不重要，**没有位置信息**

加入一个向量$e^i$，是**手动设定**的，不是学习得到的

![image-20220611105541822](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220611105541822.png)

![image-20220611105637790](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220611105637790.png)

$W^p$是手设的

## Seq2Seq with Attention

![image-20220611105820337](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220611105820337.png)

原本输入序列x^i^要通过Bidirectional RNN到h^i^，现在可以通过Self Attention Layer取代RNN

灰色的方框是RNN

![image-20220611110407997](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220611110407997.png)

*   Nx代表重复N次

*   Add&Norm 代表把 Multi-head Attention 的输入和输出加起来，再做 **Layer Normalization**

*   Layer Normalization 会使得一个向量的各个元素$\mu=0, \sigma=1$，一般会搭配RNN使用

*   Decoder的输入是上一次Decoder产生的Output
*   Masked Multi-Head Attention会只注意到已经产生的部分

## Attention Visualization

![image-20220611111521998](/Users/lly/Desktop/notes/AI/Deep Learning/Transformer.assets/image-20220611111521998.png)

 

## 大致结构

![outlook](https://jalammar.github.io/images/t/The_transformer_encoder_decoder_stack.png)



## Encoder & Decoder

![encoder](https://jalammar.github.io/images/t/Transformer_encoder.png)

![](https://jalammar.github.io/images/t/Transformer_decoder.png)

>   The decoder has both those layers, but between them is an attention layer that helps the decoder focus on relevant parts of the input sentence (similar what attention does in [seq2seq models](https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/)).

## Tensor

![tensor](https://jalammar.github.io/images/t/embeddings.png)

使用[embedding algorithm](https://medium.com/deeper-learning/glossary-of-deep-learning-word-embedding-f90c3cec34ca)，将单词转换成向量。



![](https://jalammar.github.io/images/t/encoder_with_tensors_2.png)

<!--Here we begin to see one key property of the Transformer, which is that the word in each position flows through its own path in the encoder. There are dependencies between these paths in the self-attention layer. The feed-forward layer does not have those dependencies, however, and thus the various paths can be executed in parallel while flowing through the feed-forward layer.-->

在Transformer中，每一个位置上的词都按照自己的路径在encoder中流动。

在Self-Attention层中路径之间会有依赖关系，但是在Feed Forward层中没有依赖关系，可以**并行**运算。

## self-attention

《Attention is All You Need》中提出的概念。