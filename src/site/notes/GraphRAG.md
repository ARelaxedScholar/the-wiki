---
{"dg-publish":true,"permalink":"/graph-rag/"}
---

### What?
Is a type of [[Agentic Systems\|agentic system]] where an agent is given access to a graph database containing entity mappings and other relevant information on the relations of different entities, this information then augments the information that can be queried from a vector database in a standard [[Retrieval Augmented Generation (RAG)\|RAG]] system.

### Why?
RAG is a cool system, why would we go out of our way to add graphs to them? Graphs are cool don't get me wrong, but what problem are we solving? The issue is that traditional RAG is unable to capture relationships. That is since the semantic search is done through vector similarity, it is quite possible and in fact likely that the vectors retrieved will have nothing to do with our context once we take in account the entities involved. You'd then need some kind of ranker agent, but this involves two LLM calls instead of one which already introduces some latency.

LLMs are pretty good at extracting entities and checking for relationships, but if we do not give them the right documents it is pointless. As a result, traditional RAG often refers with retrieving that sort of data.

Traditional RAG also often stuggle when it needs to connect information from multiple documents.

When using Graphs, we enable the system to laser in on a subset of relevant documents based on the search, and only then do a semantic search on these documents. This allows for laser-sharp retrieval of information, that is also entity-aware.

An example of a complex query that would be extremely hard for a traditional RAG system to solve is anything involving multiple steps of reasoning:
"Can you give me the name, of a Canadian prime minister who lost an election, after a re-election while stemming from a majority party?"

The system would first have to find the entity in the dataset who are prime ministers (which is hard to do without entity related tools that graphs provide), then they would have to prune the list to those that lost an election WHILE keeping the rest of the information in its context. 

While possible for a human, since we naturally juggle these relations (and arguably even a human might get too eager and forget something), a LLM without the right tools would find themselves utterly unable to find the information even if that information was given in the dataset unless the fact was just stated plainly in their dataset (in which case vector similarity would suffice.)

### How?
If we take the example we gave before, a GraphRAG (given the right tools) could solve it as follows:
0. Ensure that you have some way to encode that the entities talked about are prime ministers, a natural way would be to have a specialist graph (generally called a [[Knowledge Graph\|knowledge graph]]) with entities as prime ministers and other political personalities, events, party names, edges between entities as affiliations, etc.
1. The agent can then query the graph and filter for all the nodes which represent prime ministers
2. Then from those nodes, we can traverse and see if they are connected to a re-election event (or maybe, we just have a greater than 1 election event)
3. Then we see if their party was a majority one at the time of reelection (either through the graph itself, or through querying the DB for relevant metadata likely date of an event or something like that)
4. Finally, we check if they are tied to a lost event.
5. From there, if any prime minister passed the full gauntlet, we can retrieve the relevant metadata and then query against the vector database. 

Such a process is often called "Multi-Hop Reasoning" which is one solid way to solve multi-step questions.

We could then give the agents these documents to the `knowledge_base_agent` or the main agent depending on the architecture, who could then confirm that based on  the documents it has, indeed these prime ministers fit the bill and give the reply to the user, or if nothing was retrieved they can confidently say that (based on their knowledge base) no such prime minister exists.

If they fail to give an accurate information, it could be a failure in planning (which is easily debuggable by just printing the plan), or an error in the graph itself which would be our fault and deterministic since we'd write the code to create the graphs from the documents.

Note that this is just one, and arguably the most advanced way of using a GraphRAG system.
A much easier approach is simply to query both the semantic database and the graph in parallel based on the query, and give back all the context to the agent, much simpler to implement, but increases the risk of the agent just messing up with the data. It relies much more on the ability of the agent to figure out what to do with the data (without hallucinating the problem away.)

### Summary
In conclusion, GraphRAG is a nice extension to the concept of [[Retrieval Augmented Generation (RAG)\|Retrieval Augmented Generation (RAG)]] which solves the biggest problem how to answer relational and multi-step questions.

Its biggest features:
- Renders the relationships between concepts explicit for the agent so that it may query and traverse it
- Enables the agent to solve multi-step questions consistently and accurately
- Enables the agent to filter the relevant documents, before a semantic search based on metadata from the graphs

Cons?
- I mean, it's a more complex architecture, and you need to maintain a graph database and a vector database. (Note that some people forgo the vector database part entirely, and just use `JSON` graphs to encompass both the information and make the relationships, this renders traversal impossible however, and requires reading the entire document to get a clear picture of the relations relevant to an entity.)