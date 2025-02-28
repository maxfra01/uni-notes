Extra techniques for LLMs will be described in this file.

# Mixture of experts

This technique is used to **increase the size** of a model, without increasing the computational cost. We simply **add** many "experts" which are different version of one layer.
Not all experts are activated, only one (or few) are made for prediction: we talk about "sparse mixture of experts"

In transformer architecture, we apply mixture of experts into the Fully connected network. Each expert is weighted and at each step for each token we need to decide which is the expert that we need to activate: this is called **gating mechanism**.

![[Pasted image 20241104085039.png]]

Each expert has a "gating vector":
1. Each token is scored against the gating vector (via dot product)
2. The top k experts are selected and scaled with softmax
3. The output is the weighted sum of the outputs of the selected experts

![[Pasted image 20241104085250.png]]

One example for this technique is **Mixtral**: it's a sparse mixture of experts model: there are 8 experts for each FF network layer. There are 47 B parameters, but at inference time we only use 13 B of them.

# Vision-Language Model (VLM)

VLM are multi-modal models that can use **both images and text** as inputs and/or outputs.
The key parts are:
- A **Visual encoder**: used to converts images to vectors
- A **Language encoder**
- A **Multi-modal fusion** that combine the two type of vectors

**CLIP** is one of the earlier visual-language models based on transformers. Text and images are aligned in the same vector space (a picture of a dog is close to the text "dog").
To encode images, it uses a Vision Transformer (ViT).
To encode text

CLIP uses **contrastive learning**: the training set is composed of 400 M image/text pairs.

![[Pasted image 20241104090606.png]]

In this way, texts are placed close to the corresponding images, and far away from different images.
Note that CLIP is **not a generative model**, the main application is use text queries to retrieve relevant images.

# LLaVA

LLaVA is an **instruction-tuned multi-modal LLM**. It uses a vision transformer to get **visual tokens**. The visual tokens are projected (aligned) with a learned matrix then are prepended to the language instructions (question). An instruction-tuned LLM (Vicuna) is tuned to answer questions

![[Pasted image 20241104091351.png]]

# Exercises

Calculate the number of parameters of embedding layer:
- No bias term
- V = 30000
- Embedding model d = 768
- Positional embedding for 512 positions
- Number of layers 12
- Attention heads H = 12
- FF NN projection dimension: 3072
- Head dimension 64

>Embedding size = V * d + Positional_Embedding * d =  Token embedding size + positional embedding size

Now calculate the total size of the transformers (excluded embedding):

>Transformer size = 12*(Attention + FF)
>Wq, Wv, Wk =( 768 * 64 * 12 ) * 3
>Wo = 64 * 12 * 768
>Attention = wq+wv+wk+wo = 4 x (768x64x12)
>Layer norm: 768 * 2 (one for mean and one for std)
>1st 768 = > 3072, then 3072 = > 768
>FF= (768x3072)x2
>
>Total = 12(attention + layer norm + ff)


Consider inputs $[2,0], [0,2]$
$Wq= [[0,1,2],[0,0,2]]$
$Wk= [[1,2,2],[1,0,1]]$
$Wv= [[1,2,0],[0,0,1]]$
Calculate attention for the fist token

>Q = t1 * wq (we only care about the first token)
>
>Q = $[[0,2,4], [0,0,4]]$
>K = $[[2,4,4],[2,0,2]]$
>V = $[[2,4,0], [0,0,2]]$
>$$
\text{Attention}(Q,K,V) = \text{softmax}\left( \frac{QK^T}{\sqrt{ d_{k} }} \right)V
$$>$QK^T$ = $[24, 8]$
> First we divide by sqrt(3) (dk) 
> $\left[ \frac{24}{\sqrt{ 3 }}, \frac{8}{\sqrt{ 3 }} \right]$
> We compute softmax as $\frac{e^x_{i}}{\sum_{j}e^{x_{j}}}$
> 


Considering pairs of tokens , apply BPE to "abababab", starting from token a,b

>freq  ab = 4
>freq ba = 3
>(extract ab as X)
>
>XXXX
>
>freq XX (abab): 3
>
>(replace XX with Y)
>
>YY
>
>freq YY(abababab): 1
>
>(replace YY with Z)
>
>Z