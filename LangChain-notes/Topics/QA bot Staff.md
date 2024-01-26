# RetrievalQA (Staff)
- https://python.langchain.com/docs/use_cases/question_answering/vector_db_qa

*Forms DB*
```python!
from langchain.document_loaders import CSVLoader
from langchain.vectorstores import DocArrayInMemorySearch
from langchain.embeddings import OpenAIEmbeddings

# get embedding function
embeddings = OpenAIEmbeddings()

# get document ready
loader = CSVLoader(file_path=filename)
docs = loader.load()

# create db object
db = DocArrayInMemorySearch.from_documents(
    docs, 
    embeddings
)
```

*Form RetrivalQA*
```python!
retriever = db.as_retriever(
    search_type="mmr",
    search_kwargs={
        'k': 6,
        'lambda_mult': 0.25
    }
)
## query and, combine list
# k: defines #docs to be returned, default=4
# score_threshold: min similarity for query, for "similarity_score_threshold" search type.
# fetch_k: # docs pass to the MMR; default=20
# lambda_mult: 1 - minimum diversity | 0 maximum; defaul=0.5.
# filter: filter on docs' metadata. must have metadata

qa_stuff = RetrievalQA.from_chain_type(
    llm=llm, 
    chain_type="stuff", 
    retriever=retriever, 
    verbose=True
)
```

*Usage*
```python!
query =  '''
Questions or user inputs
'''
response = qa_stuff.run(query)
```


*Integrated version*
create a loader for document -> adding qa retriever
```python!
from langchain.vectorstores import DocArrayInMemorySearch
from langchain.embeddings import OpenAIEmbeddings
embeddings = OpenAIEmbeddings()
loader = CSVLoader(file_path=filename)
index = VectorstoreIndexCreator(
    vectorstore_cls=DocArrayInMemorySearch,
    embedding=embeddings,
).from_loaders([loader])
```

# Methods for combining different query results

![[Pasted image 20240126101517.png]]
- send each chunk to LLM for summarization, LLM again for giving answer
- time: 1 + 1 answering time
- works: N + 1 needed
- mostly for summarizations, 2nd most commen

![[Pasted image 20240126101529.png]]
- send each chunk to LLM for summarize sequentially with concatenation
- time: N + 1 answering needed
- works: N + 1 needed
- mostly for combining, takes time

![[Pasted image 20240126101540.png]]
- send each chunk to LLM for summarize and ranking, select highest scored answer
- time: N + 1 answering needed
- works: N + 1 needed
- Only for N chose 1 questions

## 4.Stuff method
- Combine them all to get an answer