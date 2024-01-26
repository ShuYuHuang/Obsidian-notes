# Prepare function and function schema
``` python 

import json
def function_name(property_1, ...):
    """Function Discriptions"""
    # Write your function here
    ...
    return json.dumps(...)
functions = [
    {
      "name": "function_name",
      "description": "Function Discriptions",
      "parameters": {
        "type": "object",
        "properties": {
          "property_1": {
            "type": "type of property 1",
            "description": "Discriptions of property 1"
          },...
        },
        "required": ["property_1", ...]
      }
    },...
  ]
```
# Assign function in openai call
``` python
import openai
response = openai.ChatCompletion.create(
    ...
    functions=functions
)
```

# Bind function in langchain with ChatOpenAI
```python 
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI().bind(functions=functions)
```

# Pydentic objects
```pyhton 
from typing import List, Dict, ...
from pydantic import BaseModel, Field
# Define Schema with function class

# simple pydentic object
class YourFunctionClass(BaseModel):
    """Function discription"""
    property_1: Type_1= Field(description="description of the property")
    property_2: Type_2= Field(description="description of the property"),
    ...
    
# Type_x could be any type:
#    str, int, float, Any, 
#    List, Dict, List[Dict], Dict[List], ...

# nested pydentic objct
class YourChildClass(BaseModel):
    """Discription"""
    property_1: YourFunctionClass= Field(description="description of the property")
    property_2: List[YourFunctionClass]= Field(description="description of the property")
    ...
```
(description are optional)

# Pydantic to OpenAI function definition
```pyhton 
from langchain.utils.openai_functions import convert_pydantic_to_openai_function
your_function = convert_pydantic_to_openai_function(YourFunctionClass)
```
Output
```jsx 
{
    'name': 'YourFunctionClass',
    'description': 'Function discription',
    'parameters': {
        'title': 'YourFunctionClass',
        'description': 'Function discription',
        'type': 'object',
        'properties': {
            'property_1': {
                'title': 'Property 1',
                'description': 'description of the property',
                'type': 'Type_1'
            },
            ...
        },
        'required': ['property_1', ...]
    }
}
```
Use it with bind (Autometically used when needed)
```python 
model = ChatOpenAI()
model_with_function = model.bind(functions=[your_function, ...])
model_with_function.invoke(...)
model_with_function.batch(...)
model_with_function.stream(...)
```

Use it when running model (Autometically used when needed)
```python 
model = ChatOpenAI()
model.invoke("your queries", functions=[your_function])
```

Output without function
```jsx
AIMessage(
    content='.....'
)
```
Output with function
```jsx
AIMessage(
    content='.....',
    additional_kwargs={
        'function_call': {
            'name': 'YourFunctionClass',
            'arguments': '{
                "property_1": "value_of_property_1"
            }'
        }
    }
)
```

## Forcing it to use a function

```python 
model_with_forced_function = model.bind(
    functions=[weather_function],
    function_call={"name":"YourFunctionClass"} # default is "auto"
)

```

# Function in chain
e.g.
```python 
from langchain.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant"),
    ("user", "{input}")
])
chain = prompt | model_with_function
chain.invoke({"input": "input questions"})
```

# Multiple functions
```python 
functions = [
    convert_pydantic_to_openai_function(YourFunctionClass1),
    convert_pydantic_to_openai_function(YourFunctionClass2),
    ...
]
model_with_functions = model.bind(functions=functions)
```
Chain should decide itself which function to use.