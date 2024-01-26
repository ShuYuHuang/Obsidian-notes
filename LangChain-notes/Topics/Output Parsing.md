# Pydantic
- Used to define framework of some entity, like a man with property hight, welth, address, ...
- Structurize the output of LLM

*Example*
```python
from langchain.output_parsers import PydanticOutputParser
from pydantic import BaseModel, Field


class EntityClass(BaseModel):
    property1: str = Field(description="description1")
    property2: str = Field(description="description2")

parser = PydanticOutputParser(pydantic_object=EntityClass)

prompt = PromptTemplate(
    template="Answer the user query.\n{format_instructions}\n{query}\n",
    input_variables=["query"],
    partial_variables={"format_instructions": parser.get_format_instructions()},
)

joke_query = "..."
formatted_prompt = prompt.format_prompt(query=joke_query)
```

- Prompt:
    ```
    Answer the user query.
    The output should be formatted as a JSON instance 
    that conforms to the JSON schema below.

    As an example, for the schema
    {
        "properties": {
            "foo": {
                "title": "Foo",
                "description": "a list of strings",
                "type": "array",
                "items": {
                    "type": "string"
                }
            }
        },
        "required": [
            "foo"
        ]
    } 
    the object {"foo": ["bar", "baz"]} is a well-formatted 
    instance of the schema. 
    The object {"properties": {"foo": ["bar", "baz"]}} is 
    not well-formatted.

    Here is the output schema:
    {     
    "properties": {        
     "setup": {            
     "title": "Property1",            
     "description": "description1",            
     "type": "string"         },         
    "punchline": {             
    "title": "Property2",            
     "description": "description2",            
     "type": "string"         }     },    
     "required": [         
    "property1",         
    "property2"     ] }

    {User's Query Here}
    """
    ```

## Ref
- https://juejin.cn/post/7156965302633562149
- https://juejin.cn/post/7156965302633562149