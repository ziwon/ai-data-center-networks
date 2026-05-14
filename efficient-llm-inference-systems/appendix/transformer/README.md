# Transformer

## Decoder Self-Attention Flow

![Transformer Decoder Self-Attention](./transformer-decoder-self-attention.png)

This diagram summarizes how decoder-only self-attention turns input tokens into context-aware representations:

1. Tokenize the input and look up token embeddings.
2. Add positional encoding to preserve token order.
3. Project input embeddings into `Q`, `K`, and `V`.
4. Compute attention scores with `QK^T`.
5. Apply the causal mask so each token cannot attend to future tokens.
6. Scale, softmax, and combine attention probabilities with `V`.
7. Run multi-head attention output through the Transformer block with residual connections, LayerNorm, and the FFN.

## References

- [Transformer Explainer: LLM Transformer Model Visually Explained](https://poloclub.github.io/transformer-explainer/) - interactive GPT-2 small visualization for tokenization, embeddings, masked self-attention, output probabilities, and sampling controls.
