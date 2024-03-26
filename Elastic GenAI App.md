Search, but not just search, analytics.
Semantic meaning for support.  Labelbox training LLMs. Metoffice searching weather data.

"Unsupervised" anomaly detection.

LLMs don't "know what happened in the news this morning".
If you want an LLM to work with your private data, Elastic can become the bridge to allow access into your data, whether it's a cloud LLM or something self hosted.

Elasticsearch will deliver the most relevant content to GenAI so it can answer the query.
Redact private information, apply access controls.
Reduce compute costs etc by reducing data sent.

Your business data -> Search box(es) - "what you're looking for" + "what you want to be done with the data", send that to the LLM -> Output.
Example question: Build a team out of people with different skillsets.

Text, vector search, vector data base, choice of embedding models

Vectors are search terms **stored as numbers**.  Semantic meaning.  Unstructed data, natural language.  Not just text.

Bring-your-own transformer model.

BM25 "typical search architecture" can be used with LLMs too.

3 choices for embeddings:
- Pyrotch, 3rd party model ML support
- Proprietary models
- Elastic learned sparse encoder

What needs to happen for kNN search?
- Convert search into
- get document from s3, redis?
- Aggregations - "left hand side filter"
- pagination - page 1 page 2
- geospatial / temporal filters - think of a map search, best thai food in your area... thailand?
- blending results

Demo: RAG - retrieval augmented generation
A spin on RAG .... "GAR"... what does that mean? (he never went back and answered this lol)

- Look for data
- query
- get results
- pass it to an LLM

MVP minimum viable product
- Python app
- a bunch of documents loaded into elastic search
- python queries the documents, then talks to chatgpt
- prompt engineering

ChatGPT 4 responds that Oct 2021 was the latest update to its data, old version of Elasticsearch.

"What's a redact processor" 

GPT4 can read the documentation based on what Elastic passed to it.

Apache Lucene vector database.
Elasticsearch most comprehensive blah blah blah

elastic.co/search-labs
elasticsearch-labs on github