## Introduction

Researchers are often faced with the challenge of retrieving domain-relevant data 
from disparate systems each exposing its own API specification. To reduce the burden
facing researchers, LLMs have been proposed to address the challenge of query translation.
In this paper we explore approaches to leverage LLMs to reliably perform query translations with
the goal of minimizing the burden on users and propose future areas of research to
further understanding in this domain.

## Methods

We implemented a pipeline that read annotated SPARQL queries and enriched them with 
instructions designed to improve LLM query translation accuracy. The generated
queries were then assessed for syntactic validity against the sparql.org query validator (http://sparql.org/query-validator.html).
In order to assess semantic accuracy, queries were executed against https://rdfportal.org/sib/sparql
and the number of results recorded. In the future, we intend to persist result sets
and calculate precision/recall. The prompt was manually updated based on the performance
of the LLM in order to improve translation accuracy.

## Results

Translation of a basic prompt directing the LLM to produce valid SPARQL for a given annotation
yielded syntactically valid queries [70%] of the time and semantically valid queries [0%] of the time. No
query yielded results when executed against the RDFPortal SPARQL endpoint. In order to improve results
the LLM prompt was refined as follows:

* We included the relevant parts of the Uniprot schema as part of the context - this resulted in a syntactically valid but semantically invalid SPARQL query.
* We supplemented the context with a related annotated query example - the LLM returned the example rather than formulating a new SPARQL query.
* We supplemented the context with three related annotated query examples - the LLM generated a syntactically and semantically valid query that returned valid results when executed.
* Lastly, we explored an alternative to providing example annotated queries. In this approach we explained how to generate valid triples from a ShEx schema. The LLM produced a syntactically valid but semantically invalid query. With a slight modification, the generated query rendered valid.

## Discussion

LLMs show great promise in the area of natural-to-computable query translation but require
the use of a number of strategies to improve translation accuracy. Prompt engineering and fine-tuning
are two approaches that are used to guide LLMs towards more accurate translations. In this project,
we investigated the use of prompt engineering using both manually curated instructions in the context and 
the use of RAG. The provision of relevant annotated query examples offered an easy way to improve LLM performance. However,
this approach requires a sufficiently broad corpus of examples that can be retrieved based on relevance
to the user query. While the provision of the underlying source's schema was not sufficient to improve query 
translated results, provision of the schema along with instructions on how the schema can be
leveraged to generate valid basic graph patterns appear promising. We intend to conduct further investigations
in this area. The advantage of schema-based instructions is that (1) they do not require a large corpus
of annotated examples and (2) could potentially apply to any schema.

We developed a pipeline that supports both the generation of computable queries from human language prompts
and the syntactic and semantic validation of these queries. While the initial evaluation pipeline
is quite simple, it can be extended to (1) type check SPARQL queries, (2) assess the similarity of
their result sets against the same underlying substrate, and (3) assess their complexity of design. We leave such evaluation
to future work in this area. The pipeline is currently built using python and did not fully leverage
LLM frameworks such as LlamaIndex. Adaptation to the LlamaIndex framework is planned for future work.

## Conclusion

LLMs offer the promise achieving human language query translation. However, for such approaches to have
wide applicability in scientific research, it requires that LLM pipelines be configured such that users
require minimal knowledge of the underlying repository APIs and technologies. Thus, prompt engineering
techniques that do not require user refinement or LLM model fine-tuning are strongly encouraged. In this project,
we explored potential approaches for both enhancing LLM performance and evaluating their results.