# COMP3610 Assignment 3: LLM-Powered Applications & Distributed Computing

**Student:** Denelle Mohammed (816039297)

## Overview

This project demonstrates an integrated system combining distributed data processing with Spark and retrieval-augmented generation (RAG) powered by large language models. The system analyzes NYC taxi trip data and transportation policy documents to provide intelligent analytics across structured data and unstructured documents.

## Project Structure

### Part 1: Distributed Data Processing with Spark

- **Data Source:** NYC Yellow Taxi trip data (January 2024)
- **Tasks:**
  - Spark environment setup and data loading
  - Data cleaning and feature engineering (7 derived features)
  - Spark SQL analytics with 5 analytical queries
  - Performance optimization via caching and partitioning

**Key Findings:**
- Taxi demand peaks during evening rush hours (3-6 PM)
- Weekend traffic moves faster than weekday traffic
- Short trips (<2 miles) have the highest tip percentage (18.81%)
- Cumulative trip distribution reaches 50% threshold at 3 PM

### Part 2: RAG Pipeline over Transportation Documents

- **Document Corpus:** 5 transportation policy and research documents (~516 pages):
  - NYC TLC Annual Report 2024
  - Analysis and Prediction of NYC Taxi and Uber Demand
  - Mobility Initiatives Report
  - Taxi and Transit Relationship Study
  - NYC DOT Infrastructure Report

- **Tasks:**
  - PDF document ingestion and quality assessment
  - Text chunking with multiple chunk size experiments (500, 1000, 2000 tokens)
  - Embedding generation using HuggingFace all-MiniLM-L6-v2
  - RAG pipeline implementation with Chroma vector store
  - Evaluation on 10 QA pairs

**Key Findings:**
- Optimal chunk size: 1000 tokens (balances context and precision)
- RAG accuracy: 70% on test set
- Primary failure mode (70%): generation failures despite correct retrieval
- Secondary failure mode (30%): document retrieval failures

### Part 3: Integrated Analytics Application

- **Query Router:** LLM-based classification into DATA, DOCUMENT, or HYBRID queries
- **Data Handler:** Generates Spark SQL from natural language questions
- **Document Handler:** RAG-based retrieval and answering
- **Hybrid Handler:** Combines data and document results into unified answers

**Router Accuracy:** 100% on 15-query test set

## Installation

### Requirements

- Python 3.8+
- Apache Spark 3.5.2
- 4GB+ available memory

### Setup

```bash
pip install openai tiktoken pandas matplotlib numpy pypdf chromadb sentence-transformers langchain langchain-community langchain-core langchain-text-splitters pyarrow requests pyspark
```

## File Organization

```
├── assignment3.ipynb          # Main Jupyter notebook with all analyses
├── README.md                  # This file
├── data/
│   ├── yellow_tripdata_2024-01.parquet    # Raw taxi data
│   └── cleaned_partitioned/               # Spark partitioned output (by pickup_hour)
├── docs/                      # Transportation policy documents (PDF files)
├── chroma_db/                 # Vector store (chunk size 1000)
├── chroma_500/                # Vector store (chunk size 500)
├── chroma_1000/               # Vector store (chunk size 1000)
└── chroma_2000/               # Vector store (chunk size 2000)
```

## Usage

1. **Run the Notebook:**
   ```bash
   jupyter notebook assignment3.ipynb
   ```

2. **Execute Cells in Sequence:**
   - Install dependencies
   - Configure Spark session
   - Run data processing pipeline
   - Execute RAG pipeline
   - Test integrated analytics system

## Key Results & Insights

### Data Analytics Highlights
- **Peak Hour:** Hour 17 (5 PM) with ~13% of daily trips
- **Best Day:** Thursdays have the highest average trip speed (11.88 mph)
- **Revenue Leaders:** LocationID 132 (Times Square) and 161 (Financial District)
- **Trip Categories:** Short trips dominate volume; long trips generate highest revenue

### RAG Pipeline Performance
- Chunk size 1000 optimal for transportation domain
- Strength: Clear, factual questions about policy (100% success rate)
- Weakness: Indirect or inferential questions (40% retrieval failure)

### Query Router Performance
- **Accuracy:** 100% (15/15 test queries correctly classified)
- Clear separation between DATA/DOCUMENT/HYBRID queries
- Robust handling of ambiguous queries via HYBRID fallback

## System Strengths & Limitations

### Strengths
✓ Robust Spark SQL query generation with error recovery  
✓ Accurate query classification and routing  
✓ Effective handling of clear, structured questions  
✓ Reliable document retrieval for policy-focused queries  

### Limitations
✗ Document retrieval struggles with implicit or indirect questions  
✗ Hybrid queries depend heavily on data pipeline performance  
✗ RAG accuracy limited by chunk granularity and retrieval strategy  
✗ LLM generation can hallucinate despite correct retrieval  

## Recommendations for Improvement

1. **Enhanced Document Retrieval:**
   - Implement hierarchical chunking (summary + detail levels)
   - Experiment with metadata filtering
   - Try multi-query retrieval strategies

2. **Better Answer Synthesis:**
   - Use few-shot examples in prompts
   - Implement constraint-based generation
   - Add fact verification step post-generation

3. **Hybrid Query Optimization:**
   - Weight data/document contributions based on query type
   - Implement co-ranking for merged results
   - Add multi-modal relevance scoring

## Technologies Used

- **Data Processing:** Apache Spark 3.5.2, PySpark SQL
- **LLM:** Llama 3.3 70B (via Synapse API)
- **Embeddings:** HuggingFace all-MiniLM-L6-v2
- **Vector Store:** Chroma DB
- **Text Processing:** LangChain, RecursiveCharacterTextSplitter
- **Analytics:** Pandas, Matplotlib, NumPy

## AI Tool Usage Policy 
- **Chatgpt** was used to:
    - Clarify concepts such as RAG architecture, query routing, and Spark SQL generation
    - Debug errors encountered during implementation (e.g., environment setup, PySpark issues)
    - Provide guidance on structuring code and interpreting outputs
- **Copilot** was used to:
    - Generate a README