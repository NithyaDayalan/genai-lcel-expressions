## Design and Implementation of LangChain Expression Language (LCEL) Expressions

### AIM :
To design and implement a LangChain Expression Language (LCEL) expression that utilizes at least two prompt parameters and three key components (prompt, model, and output parser), and to evaluate its functionality by analyzing relevant examples of its application in real-world scenarios.

### PROBLEM STATEMENT :
Develop an LCEL-based system that takes dynamic inputs (topic and context) and generates structured outputs using a prompt template, a language model, and an output parser, providing concise summaries and applications in JSON format.

### DESIGN STEPS :

#### STEP 1 :
Define schema – specify output format (summary and applications) with a parser

#### STEP 2 :
Create prompt – insert topic and context, instruct LLM to respond in JSON.

#### STEP 3 :
Generate and parse – get LLM output and parse it; fallback to raw text if parsing fails.

### PROGRAM :
```
from langchain.prompts import ChatPromptTemplate
from langchain.output_parsers import StructuredOutputParser, ResponseSchema
from langchain.chat_models import ChatOpenAI

response_schemas = [
    ResponseSchema(name="summary", description="Short summary of the topic"),
    ResponseSchema(name="applications", description="Applications of the topic")
]

output_parser = StructuredOutputParser.from_response_schemas(response_schemas)
format_instructions = output_parser.get_format_instructions()

prompt = ChatPromptTemplate.from_template(
    "Topic: {topic}\nContext: {context}\n\n"
    "Respond ONLY in JSON format as follows:\n"
    "{format_instructions}"
)

llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)

def evaluate_expression(topic, context):
    formatted_prompt = prompt.format_messages(
        topic=topic, context=context, format_instructions=format_instructions
    )
    raw_output = llm(formatted_prompt)
    try:
        parsed_output = output_parser.parse(raw_output.content)
    except:
        parsed_output = {"summary": raw_output.content, "applications": "Parsing failed"}
    return parsed_output

topic = "Generative AI"
context = (
    "Generative AI refers to models capable of creating new content such as text, images, and music. "
    "It uses advanced machine learning techniques like transformers and diffusion models. "
    "Applications include chatbots, content creation, drug discovery, and personalized recommendations. "
    "However, challenges include bias, misinformation, and ethical risks."
)

result = evaluate_expression(topic, context)
print(result)
```
### OUTPUT :
<img width="414" height="176" alt="image" src="https://github.com/user-attachments/assets/e779fe54-21fd-4041-a8f4-e112b42edee6" />

### RESULT :
Hence, the LCEL program using two prompt parameters and three key components is successfully written and executed, producing structured JSON output of the topic’s summary and applications.
