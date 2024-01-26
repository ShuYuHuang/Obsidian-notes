# Tools

```python!
from langchain.agents import load_tools, initialize_agent
from langchain.agents import AgentType


tool_names=[
    "llm-math",
    "wikipedia",
    ... # all the tool types
]
tools = load_tools(tool_names, llm=llm) # return Tool objects
# with fields: 
#     'name',
#     'description',
#     'args_schema',
#     'return_direct',
#     'verbose',
#     'callbacks',
#     'callback_manager',
#     'func', not necessary
#     'coroutine', not necessary
#     'api_wrapper' , not necessary

agent= initialize_agent(
    tools, 
    llm, 
    agent=AgentType.CHAT_ZERO_SHOT_REACT_DESCRIPTION,
    handle_parsing_errors=True,
    verbose = True
)
```

# Python Agent

*Create agent*
```python!
from langchain.tools.python.tool import PythonREPLTool
from langchain.python import PythonREPL

agent = create_python_agent(
    llm,
    tool=PythonREPLTool(),
    verbose=True
)
```

*Usage*
```python!
# give some variables
e.g. 
customer_list = [["Harrison", "Chase"], 
                 ["Lang", "Chain"],
                 ["Dolly", "Too"],
                 ["Elle", "Elem"], 
                 ["Geoff","Fusion"], 
                 ["Trance","Former"],
                 ["Jen","Ayai"]
                ]

# ask it to do something
e.g.
agent.run(f"""Sort these customers by \
last name and then first name \
and print the output: {customer_list}""")

# Actions
# 1. determin what functions to use for the question and provided variables
# 2. give a script for that and run it
# 3. reply using that result from running
```
- stop seuqese:
- "stop": [
    "\nObservation:",
    "\n\tObservation:"
  ]
- prompt:
    ~~~
    Human: You are an agent designed to write and execute python code to answer questions.
    You have access to a python REPL, which you can use to execute python code.
    If you get an error, debug your code and try again.
    Only use the output of your code to answer the question. 
    You might know the answer without running any code, but you should still run the code to get the answer.
    If it does not seem like you can write code to answer the question, just return "I don't know" as the answer.


    Python REPL: A Python shell. Use this to execute python commands.
    Input should be a valid python command.
    If you want to see the output of a value, you should print it out with `print(...)`.

    Use the following format:

    Question: the input question you must answer
    Thought: you should always think about what to do
    Action: the action to take, should be one of [Python REPL]
    Action Input: the input to the action
    Observation: the result of the action
    ... (this Thought/Action/Action Input/Observation can repeat N times)
    Thought: I now know the final answer
    Final Answer: the final answer to the original input question

    Begin!

    Question: {inputs}
    Thought:
    ~~~

- return action:
    ~~~
    DISCRIPTIONS FOR ACTIONS
    MAY INCLUDE ACTION RESULTED FROM LAST RECURSION
    
    Action: Python REPL
    Action Input:
    ```
    CODES
    ```
    ~~~

# Customized tools

*Tool creation*
```python!
from langchian.agents import tool
from langchain.agents import initialize_agent
from langchain.agents import AgentType

@tool
def yourfunction(*args, **kwargs) -> OUTPUT_TYPE:
    """
    Doc string that define clearly
    - when to use this tool
    - how to use this tool
        - specify input specs
    - It is demanded!!!!!!
    """
    # your function
    return your_output

agent = initialize_agent(
    tools=others_tools+[yourtool] # can be combined together
    llm=llm,
    agent=AgehtType.CHAT_ZERO_SHOT_REACT_DESCRIPTION,
    handle_parsing_errors=True,
    verbose = True
)
```

*Usage*
```python
agent.run("YOUR QUESTIONS")
```