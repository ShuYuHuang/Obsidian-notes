# LLM Engines
- fastchat
- huggingface inference engine
- vLLM
- Qwen:
    - https://github.com/QwenLM/Qwen
    - https://arxiv.org/pdf/2309.16609.pdf
- ray

# Flash attention
Engines that needs flash attention:
- huggingface inference engine

Engines that could support flash attention:
- fastchat (through vLLM)
- vLLM
- Qwen

# Requirement for flash-attention library:
https://github.com/Dao-AILab/flash-attention
FlashAttention-2 currently supports:
```
- Ampere, Ada, or Hopper GPUs (e.g., A100, RTX 3090, RTX 4090, H100). 
- Support for Turing GPUs (T4, RTX 2080) is coming soon, please use FlashAttention 1.x for Turing GPUs for now.
- Datatype fp16 and bf16 (bf16 requires Ampere, Ada, or Hopper GPUs).
- All head dimensions up to 256. Head dim > 192 backward requires A100/A800 or H100/H800.
```

# Buiding with V100?
- Hard core alternatives
https://github.com/openai/triton/issues/1567
- Suggestion to V100 https://github.com/Dao-AILab/flash-attention/issues/68