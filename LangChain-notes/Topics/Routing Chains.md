# LLM Chains
Most simple
1. Pass input to prompt format
2. Send to LLM
3. get output
```python
... # Prepared llm and prompt template
from langchain.chains import LLMChain
chain = LLMChain(llm=llm, prompt=prompt)
```

# Sequential Chains
## SimpleSequentialChain
Execute sequentially the provided chains
1. Execute chain 1, get output w/ format: {arg1: value, arg2: value,...}
2. Execute chain 2 with input arg1, arg2, get output w/ format: {arg3:value3, arg4:value4}
3. ...
```python
... # Prepared multiple chains
chain1 = ...
chain2 = ...
chains = [
    chain1,
    chain2,
    ...
]
from langchain.chains import SimpleSequentialChain
overall_simple_chain = SimpleSequentialChain(chains=chains)
```

*Usage*
```python
return_string = overall_simple_chain.run(input_string)
# return string
```

## SequentialChain
1. Execute chain 1, get output w/ format: {arg1: value, arg2: value,...}
2. Execute chain 2-1 with some of the args, get output w/ format: {arg3:value3, arg4:value4}
3. Execute chain 2-2 with some of the args, get output w/ format: {arg5:value5, arg6:value6}
4. ...
5. Output specific args
```python
... # Prepared llm and prompt templates
promp1 = ???Template.from_template("...{input1}...{input2}...")
chain1 = ???Chain(llm=llm, promp=prompt1, output_key="key1")
promp2 = ???Template.from_template("...{key1}...")
chain2 = ???Chain(llm=llm, promp=prompt2, output_key="key2")
promp3 = ???Template.from_template("...{key2}...")
...
chains = [
    chain1,
    chain2,
    ...
]
from langchain.chains import SequentialChain
overall_chain = SequentialChain(
    chains=chains,
    input_variables=["input1", "input2", ...],
    output_variables=["key?", "key?", ...] # Chose which output you wand from output_keys
)
```

*Usage*
```python
return_msg = overall_chain(input_string)
# return dictionary
```

# Router Chain
LLMRouterChain + Output Parser + MultiPromptChain
![[Pasted image 20240126101049.png]]
## 1. Prepare Destination Chains
*Destination Prompt Templates*
- define different bots for different usage
```python
template1 = ChatPromptTemplate.from_template(
    template="""
    Function discription1

    Input discription1
    {inupt}"""
)
template2 = ChatPromptTemplate.from_template(
    template="""
    Function discription2

    Input discription2
    {inupt}"""
)
...
```
*Destination Chains*
- dictionary of chains for different functions
```python
destination_chains = {
    "chain1": LLMChain(..., prompt=template1),
    "chain2": LLMChain(..., prompt=template2),
    ...
}

destinations_str="""
'chain1': 'Description1 for entering chain1'
'chain2': 'Description2 for entering chain2'
'chain3': 'Description3 for entering chain3'
...
"""
```

## 2. Prepare RouterChain
- Demand the LLM to deside the chains to execute

*Multi-Prompt Router Prompt*
~~~python
MULTI_PROMPT_ROUTER_TEMPLATE = """
Describe the main task

<< FORMATTING >>
Return a markdown code with JSON format look like
```json
{{{{
    "destination": string \ name of the destination prompt or "DEFAULT"
    "next_inputs": string \ a potentially modified version of the original input
}}}}
```

REMEMBER: "destination" MUST be one of the candidate prompt names specified below OR it can be "DEFAULT" if the input is not REMEMBER: "next_inputs" can just be the original input if you don't think any modifications are needed

<< CANDIDATE PROMPTS >>
{destinations}

<< INPUT >>
{{input}}

<< OUTPUT (remember to include the ```json)>>
"""

router_template = MULTI_PROMPT_ROUTER_TEMPLATE.format(
    destinations=destinations_str
)
~~~

*RouterChain with RouterOutputParser*
```python
from langchain.chains.router.llm_router import LLMRouterChain,RouterOutputParser

router_prompt = PromptTemplate(
    template=router_template,
    input_variables=["input"],
    output_parser=RouterOutputParser()
)

router_chain = LLMRouterChain.from_llm(llm, router_prompt)
```


## 3. Chain Router and Destinations together
- Let hte input go through router, and put to one of the desination chain
```python
from langchain.chains.router import MultiPromptChain

default_prompt = ChatPromptTemplate.from_template("{input}")
default_chain = LLMChain(llm=llm, prompt=default_prompt)

chain = MultiPromptChain(
    router_chain=router_chain,
    destination_chains=destination_chains,
    default_chain=default_chain,
)
```

*Usage*
```python
chain.run("INPUTS")
```