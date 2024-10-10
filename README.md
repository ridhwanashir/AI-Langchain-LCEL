# AI-Langchain-LCEL
 Explanation on Langchain Expression Language (LCEL)

This project demonstrates the use of LangChain Execution Language (LCEL) for building AI pipelines using Azure OpenAI models. LCEL simplifies chaining multiple components together to create powerful AI workflows using LangChain's `Runnable` interface, vector stores, and prompt templates.

## Project Overview

In this project, we explore the following concepts using LCEL:
- **Pipe Operator (`|`)**: Chaining multiple components together to create AI workflows.
- **Runnable Interface**: Understanding methods like `invoke`, `stream`, `batch`, and their asynchronous counterparts.
- **Input & Output Schemas**: Exploring how input and output schemas work for chaining and defining prompts.
- **Adding Stop Sequences**: How to use stop sequences to control the model's responses.
- **Retrieval-Augmented Generation (RAG)**: Demonstrating RAG with a custom vector store using FAISS and Azure OpenAI embeddings.
- **Dynamic Routing and Custom Functions**: Implementing complex dynamic chains with branching logic and custom functions.

The project is split into two notebooks:
1. `LCEL-Notebook-Intro.ipynb`: An introduction to the basics of LCEL.
2. `LCEL-Notebook-Deep-Dive.ipynb`: A deeper exploration of advanced features including custom function routing, multi-step prompts, and parallel execution of runnables.

## Prerequisites

- **Python 3.9 or higher**
- **Azure OpenAI API keys** (Azure OpenAI subscription)
- **FAISS**: For vector store creation
- **Virtual Environment** (venv)

## Setup Instructions

### 1. Clone the repository

```bash
git clone <repository-url>
cd LCEL-LangChain-Notebooks
```

### 2. Create a virtual environment

Create a virtual environment to isolate your project's dependencies:

```bash
python -m venv .venv
source .venv/bin/activate # On Windows use: .venv\Scripts\activate
```

### 3. Install dependencies

Install the necessary Python packages from requirements.txt:

```bash
pip install -r requirements.txt
```

### 4. Set up your environment variables

Create a `.env` file in the root of your project directory with the following content:

```env
AZURE_OPENAI_KEY=your_azure_openai_key_here
AZURE_OPENAI_ENDPOINT=your_azure_openai_endpoint_here
AZURE_OPENAI_API_VERSION=2023-03-15-preview # or another version depending on your API
AZURE_OPENAI_DEPLOYMENT=your_azure_openai_deployment_name_here
AZURE_OPENAI_EMBEDDING_DEPLOYMENT=your_azure_openai_embedding_deployment_name_here
```

Make sure to replace the placeholders with your actual Azure OpenAI credentials.

### 5. Run the notebook

Launch Jupyter or other IDE that support Python Notebook (VSCODE) and open the provided notebooks:

Open either the `LCEL-Notebook-Intro.ipynb` or `LCEL-Notebook-Deep-Dive.ipynb` to start exploring LCEL with LangChain.

## Concepts Covered

### 1. Pipe Operator (|)

The pipe operator (|) allows you to chain different components together. For example, you can chain prompts, models, and output parsers into a single pipeline:

```python
chain = prompt | model | StrOutputParser()
```

### 2. Runnable Interface

LCEL uses a runnable interface with methods to manage and stream responses:

- `invoke`: Streams back chunks of the response.
- `stream`: Continuously streams the output.
- `batch`: Executes the chain on a list of inputs.

These also have asynchronous counterparts like `ainvoke`, `astream`, and `abatch`.

### 3. Input & Output Schemas

LCEL automatically handles input and output schemas for chaining different components. You can inspect the schema with:

```python
chain.input_schema.schema()
```

### 4. Azure OpenAI Model Integration

The project integrates Azure OpenAI's GPT model for natural language processing. The model is configured and used like this:

```python
model = AzureChatOpenAI(
    api_key=os.getenv("AZURE_OPENAI_KEY"),
    azure_endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
    api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
    azure_deployment=os.getenv("AZURE_OPENAI_DEPLOYMENT"),
    temperature=0
)
```

### 5. Retrieval-Augmented Generation (RAG)

We set up a vector store using FAISS and Azure OpenAI embeddings to demonstrate RAG. This allows for context-based question-answering:

```python
vectorstore = FAISS.from_texts(texts, embedding=embeddings)
retriever = vectorstore.as_retriever()
```

## Sample Usage

Here's a basic example of generating a prompt and querying the model:

```python
prompt = ChatPromptTemplate.from_template("Tell me a joke about {topic}")
chain = prompt | model | StrOutputParser()
chain.invoke({"topic": "Programming"})
```

For a batch of inputs:

```python
chain.batch([
    {"topic": "Data Analyst"},
    {"topic": "Data Science"}
])
```

Using a vector store for RAG:

```python
chain.invoke({"context": retriever, "question": "What kind of music does Ridhwan like?"})
```

## License

This project is licensed under the MIT License.
```

This markdown format provides a structured and readable representation of the setup instructions, concepts covered, and sample usage for the LCEL-LangChain-Notebooks project.
