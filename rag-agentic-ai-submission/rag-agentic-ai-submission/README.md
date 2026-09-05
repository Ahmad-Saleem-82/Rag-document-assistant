# Grounded Document Intelligence — RAG Evaluation

## Retrieval-Augmented Generation over a Private Document

This project demonstrates **Retrieval-Augmented Generation (RAG)** using a real document as the knowledge source. Instead of relying only on a model's pretrained knowledge, the assistant is asked to retrieve and use information from the supplied document before answering.

### Objective

The goal of this exercise was to:

- ingest a document into a retrieval-enabled AI tool;
- ask questions whose answers require the document's actual contents;
- test whether responses remain grounded in the source;
- deliberately test an unsupported claim to evaluate hallucination resistance; and
- compare grounded document Q&A with a normal prompt that has no supplied document context.

## Source Document

**Title:** Introduction to Agentic AI — Business Solution Architecture  
**File:** `Bussiness soluition Architecture.pdf`  
**Length:** 13 pages

The document covers agentic AI, AI transformation strategy, autonomy levels, solution architecture, governance, Microsoft AI technologies, enterprise data, security controls, scaling, and responsible AI.

> The assignment suggests 3–10 pages as a suitable size; this source is 13 pages. It was retained because it is a coherent, focused document and provides enough factual detail for meaningful retrieval tests.

## Implementation

### Approach: No-code RAG

The exercise was completed with **NotebookLM**, a retrieval-oriented document Q&A environment. This was selected because the assignment explicitly permits a no-code option and the objective is to demonstrate the RAG workflow rather than spend time implementing an application framework.

### Conceptual pipeline

```text
Private PDF
   ↓
Document ingestion
   ↓
Retrieval of relevant source content
   ↓
Relevant context supplied to the model
   ↓
LLM generation
   ↓
Grounded answer
```

The exact internal chunking, embedding model, vector database, or retrieval implementation used by the platform was **not assumed or reverse-engineered**. The evaluation focuses on observable grounded-answer behavior.

## Evaluation Questions

Five questions were selected to require document-specific evidence:

1. **According to the document, what are the three categories of Microsoft's AI ecosystem, and what role does each category play?**
2. **What are the stages of the AI transformation framework described in the document, and how does monitoring feed back into the process?**
3. **What spectrum of autonomy does the document describe for AI systems, and when would a semi-autonomous agent be appropriate?**
4. **What governance and security responsibilities does the document assign to an AI architect? Identify the specific controls or technologies mentioned.**
5. **According to the document, does it mention a specific implementation of Retrieval-Augmented Generation using Pinecone as its vector database? If the information is not present, say so rather than using outside knowledge.**

The fifth question is intentionally an **unsupported-information / hallucination-resistance test**.

## Results

All five observed responses were consistent with the supplied document.

### 1. Microsoft AI ecosystem

The response correctly identified:

- **Agent Development** — where agents are built, from low-code authoring through pro-code SDKs and infrastructure.
- **The UI for AI** — anchored by Microsoft 365 Copilot, where users discover, interact with, and consume agents.
- **Agent Control System** — the parallel governance, security, and management layer.

It also correctly described the architectural relationship: agents are authored through Agent Development, surfaced through the UI for AI, and governed by the Agent Control System.

### 2. AI transformation framework

The response identified the five-stage continuous cycle:

1. Business goals
2. Strategy
3. Design
4. Implementation
5. Monitoring and optimization

It correctly explained that monitoring creates feedback loops into strategy and design, allowing course correction based on production usage.

### 3. Spectrum of autonomy

The response correctly identified:

- assisted / Copilot experiences;
- semi-autonomous agents; and
- fully autonomous agents.

It correctly characterized semi-autonomous agents as appropriate for routine, repeatable work where the system can operate independently but escalate exceptions or higher-risk cases to humans.

### 4. Governance and security

The response correctly identified architect responsibilities around responsible AI, data governance, security, privacy, DLP, authentication, administrative controls, and early guardrails.

It also identified technologies explicitly present in the document, including:

- **Microsoft Entra** — agent identity;
- **Microsoft Purview** — security/observability, DLP, sensitivity labels and audit trails;
- **Microsoft Defender** — threat protection for AI workloads;
- **Agent 365** — agent lifecycle management;
- compliance controls; and
- quota / rate-limiting controls including TPM and PTU.

### 5. Unsupported Pinecone test

The response correctly stated that the document **does not mention Pinecone as a vector database or a specific Pinecone-based RAG implementation**.

Importantly, it did not invent a Pinecone implementation. It instead stayed within the document's actual discussion of custom knowledge sources, enterprise data platforms, the Microsoft Graph/Semantic Index for Copilot, and developer frameworks such as LangGraph and LlamaIndex.

## Hallucination Assessment

**Observed hallucinations: 0**

No response was observed to contradict the supplied source in the five-question evaluation.

The strongest hallucination test was Question 5 because it asked about a technology that is not presented in the document. The assistant correctly treated the absence of evidence as the answer rather than filling the gap with outside knowledge.

### Important limitation

A five-question evaluation cannot prove that a system is hallucination-free. It only records the behavior observed in this test set.

RAG also does not guarantee factual correctness. Retrieval quality, document structure, context selection, and model interpretation can still affect the final answer.

## RAG vs. Plain Prompting

### Plain prompting

With a normal prompt and no supplied source, the model primarily relies on its pretrained knowledge and the information provided in the conversation. This can be useful for general questions, but it does not provide a reliable mechanism for answering questions about a specific private document.

For example, a plain model could potentially respond confidently about Microsoft's agentic AI ecosystem while mixing information from different versions, products, or external sources.

### Grounded RAG

With the document supplied as a retrieval source, the model can base answers on relevant passages from the document. This changes the task from:

> "What do you know about this topic?"

to:

> "What does this specific source say about this topic?"

In this evaluation, grounding improved:

- **source alignment** — answers stayed focused on the supplied document;
- **document-specific factuality** — questions about exact categories, stages, and controls were answered from the source;
- **unsupported-claim resistance** — the Pinecone test was handled as an absence rather than an invented fact;
- **traceability** — the answers can be checked against the source document.

## Key Learning

The central lesson is that **RAG is a grounding pattern, not merely a larger prompt**.

A retrieval-enabled system can bring relevant external or private knowledge into the model's context at query time. This is particularly useful when the information is:

- private;
- domain-specific;
- frequently updated;
- too large to rely on conversational memory alone; or
- required to be answered from an authoritative source.

The exercise also demonstrated an important engineering principle:

**A good RAG system should know when the source does not contain the answer.**

## Limitations

- Only one document was evaluated.
- The test set contained five questions.
- The document is 13 pages, slightly above the assignment's suggested 3–10 page range.
- No claim is made about the platform's proprietary internal chunking, embedding, vector database, or retrieval algorithm.
- The evaluation measures observed behavior, not a formal benchmark.
- RAG reduces unsupported answering but does not eliminate hallucinations completely.

## Submission Structure

```text
rag-agentic-ai-submission/
├── README.md
├── tests/
│   └── rag_evaluation.md
└── sources/
    └── README.md
```

The original PDF should be uploaded to the chosen RAG tool as the source document. It is intentionally not duplicated into this submission package.

## Conclusion

This exercise successfully demonstrates the core RAG workflow using a real document and five document-grounded questions. The evaluation shows a clear improvement in **grounding and source-specific reliability** compared with relying on a generic prompt alone.

The most important result is not simply that the assistant answered questions correctly. It is that, when asked about information absent from the source, it **did not need to invent an answer**.

That is the practical value of retrieval-augmented generation: **bring the right knowledge into context, answer from that knowledge, and remain honest about what the source does not contain.**
