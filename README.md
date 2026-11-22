# Document-Based RAG Search Assistant

This notebook builds a retrieval-augmented question-answering assistant that lets users ask questions about a collection of documents. Rather than relying only on the language model's general knowledge, the workflow loads documents, splits them into manageable chunks, indexes their content, retrieves relevant passages, and uses those passages as context for an answer.


## How the pipeline works

1. **Configuration:** loads settings, provider selection, paths, and credentials.
2. **Document loading:** reads supported PDF documents from the configured source directory.
3. **Chunking:** splits document text with a recursive text splitter so passages can be retrieved individually.
4. **Embedding:** creates vector representations using the selected embedding backend.
5. **Indexing:** stores the vectors and associated text in Chroma.
6. **Retrieval:** searches the index for passages relevant to the user's question.
7. **Generation:** sends the question and retrieved context to the selected chat model to produce a grounded response.


## Main components

- `DocumentProcessor` handles loading and splitting documents and previewing the resulting chunks.
- `VectorStoreManager` manages the vector database and retrieval operations.
- `RAGEngine` combines retrieval with the language model to answer queries.
- Provider setup supports OpenAI, Google Gemini, and Groq chat models through the notebook's configured selection.
- An interactive command-line loop is included for asking multiple questions in one session.


## Technology stack

- Python
- LangChain components
- Chroma vector database
- Hugging Face embedding integration
- PDF loading through `PyPDFLoader`
- Chat model integrations for OpenAI, Gemini, and Groq
- Pydantic Settings and dotenv for configuration


## Setup

1. Open `Document_Based_RAG_Search_Assistant.ipynb` in Google Colab or Jupyter.
2. Run the dependency installation cell.
3. Configure the desired model provider and the corresponding API key in the environment or `.env` file. The notebook supports provider-specific configuration; only the credentials for the provider you use are required.
4. Place the PDFs to be indexed in the notebook's configured document directory.
5. Run the notebook cells in order to initialize settings, load and preview documents, and build the vector index.


## Use it

After the index is prepared, run the query cell to ask a question. The notebook also includes an interactive mode that accepts multiple questions until the exit command is entered. You can rerun ingestion when the document collection changes; review the ingestion setting before doing so, since the notebook supports cleaning the existing index or merging/appending data.


## Configuration and data

Keep API keys out of source code and version control. Store them in environment variables or a local `.env` file that is excluded from commits. The document collection and vector database are local project data; back them up if the indexed content is important. Check the configured storage path before deleting or rebuilding the index.


## Limitations

Answers depend on the quality and coverage of the indexed documents, chunking, embeddings, and retrieval results. If the relevant information is missing from the documents, the assistant may not be able to answer accurately. Treat generated answers as summaries of retrieved context and verify critical information against the original source documents.
