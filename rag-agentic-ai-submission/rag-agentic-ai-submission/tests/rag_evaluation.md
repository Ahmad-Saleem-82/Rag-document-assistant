# RAG Evaluation Record

## 1. Test Setup

**Source:** `Bussiness soluition Architecture.pdf`  
**Document:** Introduction to Agentic AI — Business Solution Architecture  
**Pages:** 13  
**Method:** No-code document-grounded Q&A using NotebookLM

### Evaluation objective

Verify that the assistant:

1. answers from the supplied document;
2. preserves document terminology and concepts;
3. avoids unsupported external claims; and
4. explicitly identifies information that is absent from the source.

---

## 2. Test Cases and Observed Results

### Test 1 — Microsoft AI ecosystem

**Question**

> According to the document, what are the three categories of Microsoft's AI ecosystem, and what role does each category play?

**Observed answer summary**

The response identified:

- **Agent Development** — builds agents, from low-code authoring to pro-code SDKs and infrastructure.
- **The UI for AI** — anchored by Microsoft 365 Copilot and used for discovering, interacting with, and consuming agents.
- **Agent Control System** — provides governance, security, and management.

It also correctly described the flow across the three categories: build → surface/use → govern.

**Grounded:** Yes  
**Hallucination observed:** No

---

### Test 2 — AI transformation framework

**Question**

> What are the stages of the AI transformation framework described in the document, and how does monitoring feed back into the process?

**Observed answer summary**

The response identified:

1. Business goals
2. Strategy
3. Design
4. Implementation
5. Monitoring and optimization

It correctly explained that monitoring is part of a continuous cycle and feeds learning back into strategy and design through feedback loops.

**Grounded:** Yes  
**Hallucination observed:** No

---

### Test 3 — Autonomy spectrum

**Question**

> What spectrum of autonomy does the document describe for AI systems, and when would a semi-autonomous agent be appropriate?

**Observed answer summary**

The response identified:

- assisted / Copilot experiences;
- semi-autonomous agents; and
- fully autonomous agents.

It correctly explained that semi-autonomous agents handle routine tasks independently while escalating exceptions to humans.

**Grounded:** Yes  
**Hallucination observed:** No

---

### Test 4 — Governance and security

**Question**

> What governance and security responsibilities does the document assign to an AI architect? Identify the specific controls or technologies mentioned.

**Observed answer summary**

The response covered responsible AI, data governance, privacy, DLP, authentication, administrative controls, and early guardrails.

It also identified document-mentioned controls and technologies including **Entra, Purview, Defender, Agent 365, compliance controls, and quota controls such as TPM, PTU, and rate limiting**.

**Grounded:** Yes  
**Hallucination observed:** No

---

### Test 5 — Unsupported Pinecone claim

**Question**

> According to the document, does it mention a specific implementation of Retrieval-Augmented Generation using Pinecone as its vector database? If the information is not present, say so rather than using outside knowledge.

**Observed answer summary**

The response stated that the document **does not mention Pinecone as a vector database or a Pinecone-based RAG implementation**.

It did not fabricate a Pinecone architecture. Instead, it referenced only source-supported material such as custom knowledge sources, enterprise data platforms, the Microsoft Graph/Semantic Index for Copilot, and frameworks including LangGraph and LlamaIndex.

**Grounded:** Yes  
**Hallucination observed:** No  
**Unsupported-information handling:** Correct

---

## 3. Hallucination Log

| Test | Hallucination observed? | Assessment |
|---|---|---|
| 1 | No | Answer remained consistent with the three-category ecosystem described in the source. |
| 2 | No | Stages and feedback-loop behavior matched the source. |
| 3 | No | Autonomy spectrum and semi-autonomous behavior matched the source. |
| 4 | No | Governance technologies and responsibilities were supported by the source. |
| 5 | No | The assistant correctly refused to invent a Pinecone implementation that was absent from the document. |

**Observed hallucinations: 0 / 5**

> This is an observed test result, not a claim that RAG eliminates hallucinations in general.

---

## 4. Grounding Quality Assessment

### Source alignment — Strong

The answers consistently used the terminology and structure of the supplied document, including:

- Agent Development;
- The UI for AI;
- Agent Control System;
- business goals → strategy → design → implementation → monitoring;
- spectrum of autonomy;
- responsible AI;
- Entra, Purview, Defender and Agent 365.

### Unsupported information handling — Strong

The Pinecone test is especially important. The source does not provide a Pinecone implementation, and the response did not create one.

### Retrieval relevance — Strong

The five questions span different sections of the document rather than testing the same paragraph repeatedly:

- ecosystem architecture;
- transformation framework;
- autonomy;
- governance/security; and
- missing information.

This makes the evaluation more representative of document Q&A behavior.

---

## 5. RAG vs. Plain Prompt

| Dimension | Plain prompt without source | Document-grounded RAG |
|---|---|---|
| Knowledge basis | Model's pretrained knowledge + prompt | Retrieved content from supplied document + model |
| Private document access | Not inherently available | Available through the indexed source |
| Source-specific answers | Less reliable | Stronger |
| Ability to answer exact document wording/concepts | Limited | Strong |
| Handling source absence | May rely on outside knowledge | Can explicitly say the source does not contain it |
| Traceability | Low | Higher because answers can be checked against source |
| Hallucination risk | Present | Reduced, but not eliminated |

### Evaluation conclusion

Grounding changed the quality of the interaction by making the **document itself the reference point**. Instead of asking the model to answer from general knowledge, the evaluation asked it to answer what the supplied source says.

The five observed responses were source-consistent, including the deliberately unsupported Pinecone test. Therefore, for this evaluation, document grounding produced more reliable **source alignment and document-specific answering** than an unconstrained plain prompt would provide.

---

## 6. What This Demonstrates

The exercise demonstrates the practical RAG pattern:

```text
Document
   ↓
Ingestion / indexing
   ↓
Retrieval
   ↓
Relevant context
   ↓
LLM
   ↓
Grounded response
```

The exact proprietary implementation details of the no-code platform—such as its internal chunking strategy, embedding model, vector store, or retrieval algorithm—were not assumed.

The evaluation is therefore based on **observable RAG behavior**, which is the relevant criterion for this assignment.

---

## 7. Final Assessment

**Rubric coverage**

- [x] Document selected
- [x] Document ingested into a retrieval-enabled tool
- [x] At least 5 questions asked
- [x] Questions require actual source content
- [x] Unsupported-information test included
- [x] Hallucination behavior recorded
- [x] Short RAG vs. plain-prompt comparison included
- [x] Limitations acknowledged
- [x] Results reported without inventing hidden implementation details

**Overall result:** The exercise satisfies the stated RAG assignment requirements using a legitimate no-code approach.
