# Summarization of document

*Text Splitting*
``` python
from langchain.text splitter import CharacterTextSplitter
## prepare
text_splitter = CharacterTextSplitter(...)
## execute
texts = text_splitter.split(doc) # doc: str
docs = [Document(page_content=t) for t in texts]
```
## Summary mehtods
*Map Reduce*
``` python
# from langchain.chains.mapreduce import MapReduceChain
from langchain.chains.summarize import load_summarize_chain
## prepare
chain = load_summarize_chain(llm, chain_type="map_reduce") # llm: some llm object
## execute
output_summary = chain.run(docs)
```
- Iteratively summarize {summary_of_last_docs} {current_doc}
- Perform better with long docs but need to summarize many times
- prompt template for summary:
    ```
    Write a concise summary of the following:
     "{text}"In
    CONCISE SUMMARY:
    ```

*Stuffing*
```python
## prepare
BULLET_POINT_PROMPT = PromptTemplate(
    template=prompt_template,
    input_variables=["text"]
) # prompt_template: str that has {text} inside for summarization
chain = load_summarize_chain(
    llm,
    chain_type="stuffing",
    prompt=BULLET_POINT_PROMPT)
) # llm: some llm object
## execute
output_summary = chain.run(docs)
```
- summarize at once
- Perform worse in long docs, but more efficient in small docs
- prompt template for summary:
```
prompt template = """write a concise bullet point summary of the following:{text}
CONSCISE SUMMARY IN BULLET POINTS: """
```
