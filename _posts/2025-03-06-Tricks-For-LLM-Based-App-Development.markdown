{% include analytics.html %}
## Introduction
Want to develop a GenAI application? Here are a few techniques to help you out.

## Prompt Engineering Techniques
Prompt engineering is the art of designing inputs to effectively communicate with AI models. Here are five key techniques:

### 1. Few-shot Learning
Provide a few examples to demonstrate the task.
```
Prompt:
"I want you to classify the sentiment of these sentences as positive, negative, or neutral. Here are some examples:
'I love this product!' → Positive
'This is the worst day ever.' → Negative
'The weather is okay today.' → Neutral
Now classify: 'The movie was pretty good.'"
Output:
Positive
```

### 2. Zero-shot Learning
Perform a task without prior examples.
```
Prompt:
"Translate the following sentence from English to Spanish without any examples: 'The cat sleeps on the windowsill.'"
Output:
"El gato duerme en el alféizar de la ventana."
```

### 3. Chain of Thought (CoT)
Break down a problem into logical steps.
```
Prompt:
"Solve this problem step-by-step: A store sells apples for $0.50 each and oranges for $0.75 each. If I buy 3 apples and 2 oranges, how much do I spend? Please show your reasoning."
Output:
Let’s solve this step-by-step:
Cost of apples = 3 apples × $0.50 per apple = $1.50
Cost of oranges = 2 oranges × $0.75 per orange = $1.50
Total cost = Cost of apples + Cost of oranges = $1.50 + $1.50 = $3.00
```

### 4. Role-playing
Assign the model a specific persona.
```
Prompt:
"Act as a pirate and tell me how ye’d find treasure on a deserted island."
Output:
"Arr, matey! First, I’d hoist me trusty spyglass and scour the horizon fer any glint o’ gold..."
```

### 5. Output Formatting
Specify the desired structure of the response.
```
Prompt:
"List three benefits of drinking water in JSON format."
Output:
{ "benefits": [ "Improves hydration and energy levels", "Supports digestion and metabolism", "Helps maintain healthy skin" ] }
```

## Structured Outputs in OpenAI
OpenAI's Structured Outputs ensure responses adhere to predefined JSON schemas, making it invaluable for applications requiring reliable formatting.

### Example 1: Solving a Math Problem
```python
from pydantic import BaseModel
from openai import OpenAI

client = OpenAI(api_key="your-api-key")

class Step(BaseModel):
	explanation: str
	output: str

class MathSolution(BaseModel):
	steps: list[Step]
	final_answer: str

response = client.beta.chat.completions.parse(
	model="gpt-4o-2024-08-06",
	messages=[
    	{"role": "system", "content": "You are a math tutor. Provide a step-by-step solution."},
    	{"role": "user", "content": "Solve 2x + 3 = 7"}
	],
	response_format=MathSolution
)

solution = response.choices[0].message.parsed
for step in solution.steps:
	print(f"Step: {step.explanation} -> {step.output}")
print(f"Final Answer: {solution.final_answer}")
```

### Example 2: Extracting Event Details
```python
from pydantic import BaseModel
from openai import OpenAI

client = OpenAI(api_key="your-api-key")

class Event(BaseModel):
	name: str
	date: str
	participants: list[str]

response = client.beta.chat.completions.parse(
	model="gpt-4o-2024-08-06",
	messages=[
    	{"role": "system", "content": "Extract event details from the text."},
    	{"role": "user", "content": "Alice and Bob are attending a meeting on March 10."}
	],
	response_format=Event
)

event = response.choices[0].message.parsed
print(f"Event: {event.name}, Date: {event.date}, Participants: {', '.join(event.participants)}")
```

## Data Retrieval: Embedding Models and Databases
Data retrieval is crucial for applications like semantic search and recommendation systems. Embedding models transform data into numerical vectors, and embedding databases store and retrieve these vectors efficiently.

### Top Embedding Models (March 2025)
#### Open-Source
- **NV-Embed-v2 (NVIDIA):** Top MTEB score, ideal for RAG systems.
- **BGE-M3 (BAAI):** Supports 100+ languages, handles long-context data.
- **E5-Mistral-7B-Instruct (Microsoft):** Multilingual, high semantic understanding.
- **Stella (400M and 1.5B):** Cost-effective, commercial use allowed.

#### Closed-Source
- **OpenAI text-embedding-3-large:** Robust semantic understanding, API access.
- **Voyage-3-large (Voyage AI):** High retrieval relevance, API access.
- **Cohere embed-english-v3.0:** Strong for asymmetric semantic search, API access.

### Best Embedding Databases
- **Weaviate:** Open-source, robust semantic search.
- **Pinecone:** Managed, real-time similarity search.
- **ChromaDB:** Lightweight, integrates with Python AI stacks.
- **PGVector:** PostgreSQL extension, hybrid data support.
- **Vespa:** Full-featured, advanced retrieval and ranking.

### Choosing the Right Tools
- **Research/Experimentation:** NV-Embed-v2 or BGE-M3 with ChromaDB or Weaviate.
- **Commercial Applications:** Stella or Voyage-3-large with Pinecone or Vespa.
- **Budget-Conscious:** PGVector with BGE-M3 or Stella.
- **Multilingual Needs:** BGE-M3 or E5-Mistral-7B with Weaviate.

### Practical Tips
- **Test on Your Data:** Evaluate models on your dataset.
- **Balance Cost and Performance:** Open-source alternatives can match closed-source models.
- **Scalability Matters:** Choose a database that grows with your data volume.

Data retrieval is transforming how we interact with information. Whether you opt for NV-Embed-v2, OpenAI text-embedding-3-large, or a robust database like Weaviate or Pinecone, the tools available in March 2025 empower developers to build smarter, more responsive AI systems.


