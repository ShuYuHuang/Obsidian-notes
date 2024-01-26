# Memories in LangChain

*Creation:*
```python
from langchain.chains.conversation.memory import ???Memory[](https://)
memory = ???Memory()
conversation = ConversationChain(
    ...,
    memory=memory
)
``` 
*Calling*
```
conversation.memory.buffer
```

# Pblm and Method
- Cannot retrieve things according to coreference resolution

## ConversationBufferMemory
- store all conversation lines

## ConversationSummaryMemory
- Use extra conversation to summarize and recreate a new line

## ConversationBufferWindowMemory
- store conversations in a window of ```k``` interactions

## ConversationSummaryBufferMemory
- summarize and keep the new few conversations, keep it ```max_token_limit``` tokens

## Conversation Knowledge Graph Memory(ConversationKGMemory)
- 