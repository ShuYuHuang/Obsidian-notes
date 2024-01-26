# Qwen Intro
- Open sourced by Ali-Baba
- Model
    - 1.8B, 7B, 14B, 30B version
    - Multi-Lingual (Not so good in Arabic)
    - Self-defined chat function (https://huggingface.co/Qwen/Qwen-7B-Chat/blob/main/qwen_generation_utils.py)
    -  Qwen-14B from 2K to over 8K tokens, and Qwen-1.8B/7B from 8K to 32K tokens
- Engine
    - Support openai API hosting 
        - support for function calls
        - https://github.com/QwenLM/Qwen/tree/ab109ced9fe05781f5bbd2c3b41500bc58c17eba
    - Speed-ups for LLM inference:
        - Auto scaling for multiple GPU
        - float16, int8 support
        - flash-attention support
        - paged-attention
        - xformer support for training
    - Tool Usage

#alibaba #lang-en #lang-ch #1_8B #7B #14B #32K_token