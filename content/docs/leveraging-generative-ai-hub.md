---
title: "Harnessing RAG with Generative AI Hub and Chroma Vector Store - Using Python SDK"
date: 2024-04-23
draft: false
---

In the rapidly evolving world of artificial intelligence, generative AI technologies have become a cornerstone for innovative applications across various industries. As demonstrated in SAP's recent tech bite, the Generative AI Hub on SAP Launchpad provides a robust platform for developers looking to explore and implement sophisticated AI-driven solutions. Here's how you can leverage this powerful tool to enhance your applications and drive innovation.

## Exploring the SAP Generative AI Hub

The Generative AI Hub is designed to facilitate easy interaction with AI models through REST API calls from any programming environment, be it JavaScript, Python, or others. This flexibility allows developers to integrate AI capabilities seamlessly into their existing systems. Visit the [SAP AI Core on the Discovery Center](https://discovery-center.cloud.sap/serviceCatalog/sap-ai-core?region=all).


### Utilizing SDKs for Enhanced Integration

For those looking to dive deeper, the Generative AI Hub offers specialized SDKs, including Python and JavaScript versions. These SDKs simplify the implementation of complex functionalities such as agent orchestration, retrieval augmented generation, and chat history management. By using these SDKs, developers can create more dynamic and responsive AI applications. Visit the [SAP generative AI hub SDK](https://pypi.org/project/generative-ai-hub-sdk/)

## Advanced Use Case: Retrieval Augmented Generation

One of the most exciting capabilities of the Generative AI Hub is its support for retrieval augmented generation. This technique involves grounding a large language model in specific context information, thereby improving the accuracy and relevance of its responses.

### Practical Implementation with Python SDK

Using the Python SDK provided by the Generative AI Hub, developers can implement retrieval augmented generation by integrating with SAP AI Core and Hana Vector Store. This process involves converting relevant data into embeddings, which are then stored and utilized to provide contextually aware responses.

#### Example: Educational Assistance

Consider the scenario where an AI helps students with literature assignments. By storing detailed summaries of required reading materials as embeddings in a vector database, the AI can access the most relevant information to accurately respond to queries about the text. This not only enhances learning but also ensures the integrity of the educational process by preventing the AI from generating incorrect or misleading information.

## Setting Up and Deploying Models

Setting up and deploying models within the AI Launchpad is straightforward. Developers can manage their AI deployments effectively, allowing for quick iterations and updates to their applications. This ease of use is crucial for maintaining the agility required in today's fast-paced technological landscape.

## Code Repo
[RAG with SAP generative AI hub](https://github.com/writefayaz/sap-tech-bytes/tree/2024-29-01-generative-ai-hub).

# Using Chroma Vector Store with OpenAI Embeddings for Efficient Information Retrieval

In this blog, we'll explore an alternative approach to store vectors and facilitate efficient retrieval of answers for natural language queries, specifically focusing on a scenario involving questions about time travel. This method utilizes the Chroma vector store from the `langchain_community` alongside OpenAI embeddings, serving as a cost-effective alternative to SAP HANA Vector Store.

## Implementation & Code Breakdown

Below is the Python code for setting up the system, complete with comments explaining each component's function:

```python
# Import the ChatOpenAI class for chat-based model interactions
from gen_ai_hub.proxy.langchain.openai import ChatOpenAI
# Import the OpenAIEmbeddings class to handle text embeddings
from gen_ai_hub.proxy.langchain.openai import OpenAIEmbeddings

# Import tools for splitting texts into manageable chunks
from langchain.text_splitter import CharacterTextSplitter
# Import a utility for loading text documents
from langchain_community.document_loaders import TextLoader
# Import Chroma for creating a vector store from text embeddings
from langchain_community.vectorstores import Chroma

# Import the RetrievalQA class for retrieval-based question answering
from langchain.chains import RetrievalQA

# Initialize a text loader for the document 'time_travel.txt'
loader = TextLoader('time_travel.txt')
# Load the document's content
documents = loader.load()
# Setup a splitter to divide text into 1000 character chunks with no overlap
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
# Apply the text splitter to the loaded documents
texts = text_splitter.split_documents(documents)

# Setup an embedding model using OpenAI's 'text-embedding-ada-002'
embedding_model = OpenAIEmbeddings(proxy_model_name='text-embedding-ada-002')
# Create a Chroma vector store using the generated text chunks and the embedding model
db = Chroma.from_documents(texts, embedding_model)
# Convert the vector store into a retriever for fetching relevant text chunks
retriever = db.as_retriever()

# Initialize a chat model using OpenAI's 'gpt-35-turbo'
chat_llm = ChatOpenAI(proxy_model_name='gpt-35-turbo')

# Configure the RetrievalQA system with the chat model and text retriever
qa = RetrievalQA.from_llm(
    llm=chat_llm, retriever=retriever)

# Define a complex query about time travel
query = ""
# Print a response header
print('\nRESPONSE:')
# Invoke the question-answering system with the query and print the output
print(qa.invoke(query)['result'])

```

In conclusion, the Generative AI Hub offers a powerful suite of tools for developers looking to advance their applications with AI. Whether you are implementing basic interactions or complex, contextually aware systems, the platform provides the necessary components to bring your innovative solutions to life. Harness these tools to not only enhance your applications but also to drive forward the future of AI in your industry.

