# CSF Explorer

CSF Explorer is an AWS-hosted retrieval-augmented generation (RAG) application for exploring the National Institute of Standards and Technology Cybersecurity Framework (NIST CSF).

The application converts authoritative NIST CSF documents into a structured, searchable knowledge base and enables users to ask natural-language questions about the framework. Answers are grounded in approved source material and include citations that allow users to verify the supporting evidence.

> **Project status:** In development

---

## Purpose

NIST CSF documentation contains valuable guidance, but users often need to navigate multiple documents, framework versions, Functions, Categories, Subcategories, implementation examples, and supporting publications to find relevant information.

CSF Explorer is intended to make this information easier to discover and understand while preserving the authority, structure, version, and provenance of the original NIST documents.

The application will help users:

- Ask questions about the NIST Cybersecurity Framework in natural language.
- Find relevant Functions, Categories, Subcategories, and framework outcomes.
- Search using CSF identifiers such as `GV.OC-01`.
- Receive answers grounded in approved NIST source documents.
- View citations and supporting source passages.
- Distinguish between framework versions and document types.
- Identify when the available evidence does not support an answer.
- Explore the framework without replacing professional cybersecurity judgment.

---

## Intended Users

CSF Explorer is designed for:

- Cybersecurity professionals
- Governance, risk, and compliance teams
- Security architects
- Internal auditors
- Technology leaders
- Business and control owners
- Consultants and assessors
- Users learning or adopting the NIST Cybersecurity Framework

The application is an informational and research tool. It does not certify compliance, perform an audit, provide legal advice, or determine whether an organization’s controls are effective.

---

## Core Capabilities

### Document ingestion

- Register approved NIST CSF source documents.
- Preserve original PDF files as authoritative source artifacts.
- Record document versions, checksums, publication information, and processing status.
- Detect duplicate or superseded source documents.

### PDF preprocessing

- Extract text and document layout from PDF files.
- Convert extracted content into structured Markdown.
- Preserve headings, lists, tables, page boundaries, links, and CSF identifiers.
- Record low-confidence extraction and formatting exceptions for review.
- Maintain traceability from processed content back to the original PDF.

### Metadata enrichment

CSF Explorer will capture metadata such as:

- Source document identifier
- Source title and version
- Source authority
- Document or resource type
- CSF Function
- Category identifier and title
- Subcategory identifier
- Heading path
- Page range
- Publication date
- Lifecycle status
- Processing pipeline version
- Chunking version
- Embedding version
- Access classification

This metadata will support filtering, ranking, citation construction, version management, and evaluation.

### Structure-aware chunking

Rather than splitting documents only by character or token count, CSF Explorer will create chunks based on document meaning and structure.

Chunking rules will aim to:

- Keep CSF identifiers with their associated outcome text.
- Preserve Function, Category, and Subcategory relationships.
- Keep headings and essential context with each chunk.
- Preserve table headers and row meaning.
- Avoid separating qualifications, exceptions, or list introductions.
- Record the source version and page range for every chunk.
- Create stable, deterministic chunk identifiers.

### Search and retrieval

The retrieval pipeline is expected to combine:

- Lexical or keyword search
- Semantic vector search
- Exact CSF identifier matching
- Metadata filtering
- Authority and version filtering
- Hybrid result fusion
- Deduplication
- Reranking
- Minimum-evidence checks

The objective is to retrieve the smallest set of relevant and authoritative passages required to answer the user’s question.

### Answer generation

Answers will be generated using an approved Amazon Bedrock model and must:

- Be based on retrieved evidence.
- Clearly answer the user’s question.
- Include citations for substantive factual claims.
- Identify the relevant document or CSF version when necessary.
- Distinguish official framework text from explanatory material.
- Communicate uncertainty or ambiguity.
- Decline to answer when the governed corpus does not contain sufficient evidence.
- Avoid unsupported compliance or audit conclusions.

### Citations

User-visible citations will be constructed by the application from trusted metadata rather than relying solely on model-generated citation text.

A citation may include:

- Source title
- Source version
- CSF identifier
- Page or section
- Supporting passage
- Link to an authorized source view

### Guardrails and safety

CSF Explorer will use layered safeguards, including:

- Input validation
- Authentication and authorization
- Retrieval access filters
- Separation of instructions from retrieved document content
- Prompt-injection protections
- Citation validation
- Evidence-sufficiency checks
- Output validation
- Safe refusal behavior
- Amazon Bedrock Guardrails
- Security monitoring and red-team evaluations

---

## High-Level Architecture

CSF Explorer will be hosted on Amazon Web Services.

The proposed architecture includes:

| Capability | Proposed AWS service |
|---|---|
| Source and processed document storage | Amazon S3 |
| Processing orchestration | AWS Step Functions |
| Document processing | AWS Lambda or container-based workers |
| Complex PDF extraction fallback | Amazon Textract |
| Metadata and operational records | Amazon Aurora PostgreSQL |
| Keyword and vector retrieval | Amazon OpenSearch Service |
| Embedding generation | Amazon Bedrock |
| Answer generation | Amazon Bedrock |
| Model safeguards | Amazon Bedrock Guardrails |
| Application hosting | Amazon ECS with AWS Fargate or an approved serverless alternative |
| Content delivery | Amazon CloudFront |
| Web application protection | AWS WAF |
| Identity and access | Enterprise federation and AWS IAM |
| Encryption | AWS KMS |
| Secret storage | AWS Secrets Manager |
| Logging and monitoring | Amazon CloudWatch |
| Administrative audit | AWS CloudTrail |
| Backup and recovery | AWS Backup and service-specific recovery mechanisms |

The final AWS services and configurations will be selected through implementation experiments, evaluation results, security requirements, operational needs, and cost analysis.

---

## Processing and Question-Answering Flow

```text
NIST PDF
    |
    v
Source validation and registration
    |
    v
PDF text and layout extraction
    |
    v
Structured Markdown generation
    |
    v
CSF hierarchy and metadata enrichment
    |
    v
Structure-aware chunking
    |
    v
Embedding generation
    |
    v
OpenSearch indexing
    |
    v
Hybrid retrieval and reranking
    |
    v
Evidence selection
    |
    v
Bedrock answer generation
    |
    v
Citation and output validation
    |
    v
Grounded answer or safe refusal
