Mistral (Oct.2023) was a descendent of [[LLAMA2]], aims for commercial usage. It utilized:
- sliding window attention to maximize the token length
- grouped-query to raise the throughput
- rolling buffer cache to minimize the memory usage
**Organizations:** [MistralAI](https://mistral.ai)
## SPEC:
- Sub-versions: 7B, 7B-instruct
- Max tokens: 8K
- Ranking: 
	- HF Arena rank: 40 for Mistral 7B
	- HF Score: 68.22 NeuralHermes-2.5-Mistral-7B
	- Open Router: 9 for Mistral 7B-Instruct
## LICENSE: 
- 
- **Apache 2.0**

## Descendent:
- [NousResearch Yarn Mistral](https://huggingface.co/NousResearch/Yarn-Mistral-7b-128k) ([[YARN]])
- [[Mixtral]]
## Links:
- https://huggingface.co/mistralai
- https://www.sohu.com/a/728705390_121649381
- https://arxiv.org/pdf/2310.06825.pdf

#mistral_family #fastchat
2024-01-22