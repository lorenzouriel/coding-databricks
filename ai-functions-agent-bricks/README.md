# AI Functions Agent Bricks

## Overview
This project shows how to build an **end-to-end RAG (Retrieval-Augmented Generation)** workflow using **Databricks SQL**, **AI Functions (`ai_query`)**, and **Vector Search (`VECTOR_SEARCH`)**.

It uses Uber Eats–style data (searches, items, and orders) to demonstrate:
- Data ingestion from JSON files.
- AI-powered enrichment of search intents, menu descriptions, and order statuses with AI-Functions.
- Semantic and hybrid search using Databricks vector indexes.
- Querying and using Playgroung with the indexed vector data.

## Structure
```bash
ai-functions-agent-bricks/
├── data/
│   ├── json-files/
│   └── text-files/
└── gen-ai/
├── create/
│   └── table-creation.sql
└── script/
├── batch_inference.sql
├── vector_search.sql
└── rag_vector_search_queries.sql

```

## Setup
1. **Clone the repo**
```bash
git clone C:\WAVES\portfolio\coding-databricks\ai-functions-agent-bricks
cd ai-functions-agent-bricks
```

2. **Create tables**
```sql
%sql
RUN './gen-ai/create/table-creation.sql';
```

3. **Run AI enrichment**
```sql
%sql
RUN './gen-ai/script/batch_inference.sql';
```

4. **Build and test vector search**
```sql
%sql
RUN './gen-ai/script/vector_search.sql';
RUN './gen-ai/script/rag_vector_search_queries.sql';
```

## Example Queries
**Semantic Search**
```sql
SELECT i.product_name, vs.search_score
FROM VECTOR_SEARCH(
  index => 'ubereats.default.idxitemspt',
  query_text => 'cheap gluten-free vegetarian pizza',
  num_results => 5
) AS vs
JOIN items_for_vs i ON vs.id = i.id;
```

**Hybrid Search**
```sql
SELECT i.product_name, vs.search_score
FROM VECTOR_SEARCH(
  index => 'ubereats.default.idxitemspt',
  query_text => 'napolitan gluten-free pizza',
  query_type => 'HYBRID',
  num_results => 5
) AS vs
JOIN items_for_vs i ON vs.id = i.id;
```

## Tech Stack
* **Databricks SQL**
* **AI Functions:** `databricks-gpt-oss-120b`
* **Vector Search**
* **Data Source:** JSON (Kafka, MongoDB)