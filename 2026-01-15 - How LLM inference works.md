---
tags:
  - llms
---
# How LLM inference works

*Note: This is an extremely high level treatment of the subject - many implementation details might be different from expected.*

1. Prompt broken down into tokens.
2. N tokens mapped to M dimensional vectors - M x N input embedding matrix.
3. Input embedding matrix + positional encoding matrix (encodes positional info for each token)
4. Input matrix fed through first layer - aka transformer block - made up of 2 sub layers - multi-head self attention + feed forward network (aka MLP)
5. Self attention adds meaning to each token's vector based on position and how it relates to other vectors. MLP enriches each token's vector on its own. Output -> "Difference matrix" encoding all changes in meaning from this layer.
6. Difference matrix added to original input matrix to produce intermediate M x N matrix. Each subsequent layer performs similar computations, resulting in a "residual stream" of matrices.
7. After the last layer, vector for final token is multiplied by a huge matrix representing the entire vocab, which projects it to a huge vector the size of the vocab. Each element is a logit, which is a real number representing confidence. Softmax is applied to turn logits into a probability distribution.
8. A sampling method is used to decide on one token. Top K = shrink the candidate pool to the top K most likely tokens and do weighted random sampling. Top P = same as top K but P = cumulative probability (e.g. top 90%).
9. Steps 1-8 are called the "prefill phase". Next phase is called the "decode phase", where each newly generated token gets fed through the model. Intermediate values are computed only for the new token, while values for previous tokens are drawn from the KV cache.

A note on KV caching: K and V are intermediate values computed by the attention mechanism at each layer. Each token has specific K and V values at each layer of the model, but these remain constant as new tokens are generated. Hence the model generates them once, stores them in a KV cache, and reuses the cached values when processing new input tokens.