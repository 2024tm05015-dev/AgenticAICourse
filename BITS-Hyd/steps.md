# Local RAG on Kubernetes with Qwen3-8B-Instruct (vLLM) & Qdrant

This guide walks you through setting up a complete Retrieval-Augmented Generation (RAG) pipeline on a bare Ubuntu workstation equipped with an **NVIDIA RTX 4090 (24 GB)**. 

The stack deploys a production-ready infrastructure inside a local Kubernetes environment (`minikube`), using `vLLM` to serve the LLM, `Qdrant` as the vector database, and a custom `FastAPI` service to orchestrate the RAG logic.

## Architecture Overview

```text
                 +----------------------+
                 |       Ingress        |  (optional)
                 +----------+-----------+
                            |
                    rag-api-service
                            |
                     RAG API (FastAPI)
                     - embeds queries (sentence-transformers, CPU)
                     - retrieves context from Qdrant
                     - calls vLLM for generation
          +-----------------+-----------------+
          |                                   |
      vllm-service                       qdrant-service
   Qwen3-8B-Instruct (vLLM)                Qdrant
          |                                   |
      RTX 4090 GPU node                Persistent Volume
