---
{"dg-publish":true,"permalink":"/retrieval-augmented-generation-rag/"}
---

### What?
Retrieval Augmented Generation or RAG is fundamentally nothing more than giving a LLM a tool to query a vector database. Using this, the LLM which we may now call an [[Agent\|agent]] since it has a tool, is able to based on context query the vector database for vector similarity in chunks, and then using that is able to give a more context-aware reply to whatever the user may have asked.

It is the base of a more powerful approach called [[GraphRAG\|GraphRAG]] which combines the powerful entity mapping abilities of a graph, to the semantic search enabled by vector databases to make the retrieved documents much more relevant.

### Why?
It's one strong way to [[Grounding\|ground]] the LLM's answers. Essentially, by giving the LLM the ability to retrieve from a pre-defined knowledge base (and usually adding to its prompt that it shouldn't claim anything that cannot be verified from said knowledge base), we can be more confident that the LLM's reply will be inline with knowledge that we know to be true, greatly simplifying research and when properly tuned reducing the risks of hallucination to near zero.

More importantly, it allows an LLM to have on-hand a knowledge base that could never fit in its context window constantly available for retrieval. Arguably, even with models with huge context windows like Gemini 2.5 Pro which when it was announced papers claimed could have context window reaching 10M tokens, a RAG would still be thoroughly useful there as it would allow the LLM to navigate their knowledge more efficiently and not get bogged down by irrelevant details.

The concept of RAG is a deceptively powerful one, and there's no reason to use it solely as a dumb knowledge base. In fact, the analogy to the natural memory process is right there.
As a human, our brains contain petabytes of information, but generally we retrieve that information based on triggers in our environment which makes the given information relevant, a RAG gives the same ability to an LLM and (in my estimation) would be a crucial component of any system claiming to be [[Artificial General Intelligence (AGI)\|AGI]].

### How?
Building a RAG is generally broken down in two phases:
- Offline Phase: Where the documents to be stored in the RAG are chunked (split into smaller documents), processed, augmented with relevant metadata (to aid querying and document the information) before being embedded (turned to vectors of numbers) and stored in an aptly-named vector database.
- Online Phase: Much simpler, it's really just a LLM chatting with you with the tool needed to query the vector database prepared in the previous step. Usually, the flow looks like:
	- **Retrieve**: Get information from the vector database
	- **Augment**: Add the retrieved information to the context window of the LLM
	- **Generate**: Now that the LLM has this context, you let them generate the context-aware reply

Common choices of vector databases include:
- ChromaDB: Easy to implement, but might struggle under big load
- FAISS: Harder to implement, meant for BIG data, written by Facebook
- Qdrant: Written in **RUST**, fast, highly performant, higher learning curve

### Summary
A RAG is a core component of agentic systems that plays a huge role in [[Context Engineering\|context engineering]], it can also be used to give an agent short and long-term memory in more advanced systems.

Pros:
- Gives knowledge base to agent much bigger than its context window could contain 
- Gives precise context to the agent to help with proper replying to questions

Cons:
- Can struggle with retrieving relevant documents (vector similarity may realistically bring a vector which is semantically similar but originates from an irrelevant document). This can be solved by adding Ranker agent which arranges and scores the retrieved documents and either only return the most relevant, or returns them all with scores that the LLM may use to prioritize. (The first option is likely better in line with the precept of "minimal sufficient context")
- Can struggle with multi-step questions since its only tool is vector similarity
- The vector embedding during the online phase will add some overhead and there's no real way to avoid it (besides not querying the DB.) A [[ReAct Agent\|ReAct]] agent may assess the query, and based on the history and the already retrieved documents decide to skip querying the vector store.