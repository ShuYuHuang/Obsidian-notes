(May.2023)


(31.May.2023 by EleutherAI, Yale Related)

- Official 2.8B: [https://huggingface.co/EleutherAI/pythia-2.8b](https://huggingface.co/EleutherAI/pythia-2.8b)

- 2.8B With instruction: [https://huggingface.co/lambdalabs/pythia-2.8b-deduped-synthetic-instruct/tree/main](https://huggingface.co/lambdalabs/pythia-2.8b-deduped-synthetic-instruct/tree/main)


DESCRIPTION_OF_MODEL
**Organizations:** 
## SPEC:
- Sub-versions: 14M, 70M, 160M, 410M, 1B, 1.4B, 2.8B, 6.9B, 12B
- Max tokens: 2048
- Ranking: 
	- HF score: 39.7
## LICENSE: 
- 
## Descendent:
- https://huggingface.co/lambdalabs/pythia-2.8b-deduped-synthetic-instruct/tree/main
## Links:

## System Prompt:
```python
# OpenAssistant Pythia default template
register_conv_template(
    Conversation(
        name="oasst_pythia",
        roles=("<|prompter|>", "<|assistant|>"),
        sep_style=SeparatorStyle.NO_COLON_SINGLE,
        sep="<|endoftext|>",
    )
)

```
[source](https://github.com/lm-sys/FastChat/blob/8163cb2719b3155fd5b83dd0bf4190f61a847a6d/fastchat/conversation.py#L318)

2024-01-22

 #meta #work