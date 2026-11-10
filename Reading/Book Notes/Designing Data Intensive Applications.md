### Common functionalities of data-intensive applications-

- Store for future retrieval by self or an application (Database)
- Store the result of an expensive operation for faster frequent reads (caches)
- Speed up data search by keywords or filters (indexes)
- Send a message to another process to be handled in async (stream processing)
- Periodically process a large chunk of data (batch processing)

Even for the same functionality, there exists various types of approaches and properties.
E.g- Different types of Database storages- relational, object based, KV, Graph, etc.
Different types of caching mechanims

The tools and techniques to be used are dependent on the problem to be solved, the priorities and the requirements.
Often, multiple tools and techniques are combined to achieve what a single tool cannot

Objective-
Make data systems **reliable**, **scalable** and **maintainable**
