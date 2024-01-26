# Chaining processes

use | to chain processes, including model, pre-/post-process function or other chains
```python 
from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.schema.output_parser import StrOutputParser

prompt = ChatPromptTemplate.from_template(
    "tell me a short joke about {topic}"
)
model = ChatOpenAI()
output_parser = StrOutputParser()
chain = prompt | model | output_parser
chain.invoke({"topic": "bears"})
```

# Fallbacks
Use backup chain when the chain failed
```python 
final_chain = chainA.with_fallbacks([chainB])
```

# Interfaces

Invoke: input Dict, output str
```python 
chain.invoke({"topic": "bears"})
```
Batch: input List[Dict], output List[str]
```python 
chain.batch([{"topic": "bears"}, {"topic": "frogs"}])
```
Stream: input Dict, output iterable str
- [Streamable list](https://python.langchain.com/docs/integrations/chat/)
```python 
for t in chain.stream({"topic": "bears"})
    print(t)
```