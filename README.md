# Agentic RAG with Azure AI Search

A sophisticated Retrieval-Augmented Generation (RAG) system using Azure AI Search's Knowledge Agent capabilities for intelligent document retrieval and question answering.

## Overview

This project demonstrates how to build an agentic RAG system that can intelligently answer questions about product reports using Azure AI Search's Knowledge Agent feature. The system combines document indexing, semantic search, and AI-powered response generation to provide accurate, context-aware answers.

## Features

- **Knowledge Source Management**: Create and manage search index-based knowledge sources
- **Intelligent Agents**: Deploy AI agents with custom instructions and behavior
- **Semantic Retrieval**: Leverage Azure OpenAI embeddings for semantic document search
- **Response Synthesis**: Generate contextual answers with source attribution
- **Configurable Reranking**: Fine-tune retrieval relevance with threshold controls

## Architecture

The system consists of three main components:

1. **Knowledge Source**: Defines the data source (Azure AI Search index) containing product documents
2. **Knowledge Agent**: AI agent configured with Azure OpenAI models for processing queries
3. **Retrieval Client**: Interface for submitting questions and receiving AI-generated responses

## Prerequisites

- Azure subscription with access to:
  - Azure AI Search service
  - Azure OpenAI service
- .NET environment (for C# implementation)
- Azure CLI or appropriate authentication setup

## Setup

### 1. Azure Services Configuration

Ensure you have the following Azure services deployed:

- **Azure AI Search**: For document indexing and semantic search
- **Azure OpenAI**: For embeddings and chat completions

### 2. Environment Variables

Update the following configuration in your notebook:

```csharp
string aoaiEndpoint = "https://your-openai-endpoint.openai.azure.com/";
string searchEndpoint = "https://your-search-service.search.windows.net";
string indexName = "your-index-name";
```

### 3. Model Deployments

Ensure these models are deployed in your Azure OpenAI service:

- **Embedding Model**: `text-embedding-3-large`
- **Chat Model**: `gpt-5-mini` (or your preferred GPT model)

## Usage

### Running the Notebook

1. Open `AgenticRag.ipynb` in VS Code or Jupyter
2. Install required NuGet packages:
   - Azure.Search.Documents (v11.7.0-beta.7)
   - Azure.Identity (v1.15.0)
   - dotenv.net (v4.0.0)

3. Execute cells sequentially to:
   - Set up authentication
   - Create knowledge source
   - Deploy knowledge agent
   - Test with sample queries

### Sample Query

The notebook includes an example query for generating marketing emails based on customer insights:

```csharp
string question = "Write marketing email for Product X based on the customer insights?";
```

### Agent Instructions

The system uses detailed instructions to ensure accurate responses:

- Answers are limited to information in the provided documents
- Returns "I don't know" when information isn't available
- No external knowledge or assumptions are used

## Configuration Options

### Knowledge Source

- **Data Selection**: Configure which fields to retrieve (`chunk_id`, `chunk`, `title`)
- **Index Integration**: Direct connection to Azure AI Search indexes

### Knowledge Agent

- **Model Configuration**: Specify Azure OpenAI deployment details
- **Output Modality**: Set to `AnswerSynthesis` for comprehensive responses
- **Reference Inclusion**: Enable source attribution and reference data
- **Reranking**: Configure relevance thresholds (default: 2.5)

### Retrieval Settings

- **Activity Tracking**: Monitor agent processing steps
- **Source References**: Include document sources in responses
- **Custom Instructions**: Define agent behavior and constraints

## Project Structure

```
AgenticRAGWithAISearch/
├── AgenticRag.ipynb          # Main implementation notebook
├── README.md                 # This file
└── [additional files]        # Data files, configurations, etc.
```

## Key Components Explained

### Knowledge Source Creation

Creates a searchable knowledge base from an existing Azure AI Search index:

```csharp
var indexKnowledgeSource = new SearchIndexKnowledgeSource(
    name: knowledgeSourceName,
    searchIndexParameters: new SearchIndexKnowledgeSourceParameters(searchIndexName: indexName)
);
```

### Agent Deployment

Configures an AI agent with specific models and behavior:

```csharp
var agent = new KnowledgeAgent(
    name: knowledgeAgentName,
    models: new[] { agentModel },
    knowledgeSources: new KnowledgeSourceReference[] { ... }
);
```

### Query Processing

Processes user questions through the configured agent:

```csharp
var retrievalResult = await agentClient.RetrieveAsync(
    retrievalRequest: new KnowledgeAgentRetrievalRequest(messages)
);
```

## Best Practices

1. **Index Design**: Ensure your search index is optimized for the types of queries you expect
2. **Chunk Strategy**: Use appropriate document chunking for better retrieval accuracy
3. **Model Selection**: Choose models that balance performance and cost for your use case
4. **Threshold Tuning**: Adjust reranking thresholds based on your quality requirements
5. **Error Handling**: Implement robust error handling for production deployments

## Troubleshooting

### Common Issues

- **Authentication Errors**: Ensure DefaultAzureCredential is properly configured
- **Model Not Found**: Verify model deployments in Azure OpenAI
- **Index Access**: Check permissions for the Azure AI Search service
- **Empty Responses**: Verify index contains relevant data for queries

### Performance Optimization

- Monitor retrieval latency and adjust timeout settings
- Consider caching for frequently asked questions
- Optimize index schema for your specific use case


## Resources

- [Azure AI Search Documentation](https://learn.microsoft.com/en-us/azure/search/search-agentic-retrieval-concept)
- [Reterival Agent Quick Start GitHub sample](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/main/quickstart-agentic-retrieval/quickstart.ipynb)


**Note**: This project uses preview features of Azure AI Search. Monitor the official documentation for updates and changes to the Knowledge Agent APIs.
