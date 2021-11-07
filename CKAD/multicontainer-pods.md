# Multicontainer Pods

There are 3 design patterns associated with multi container pods.

- Sidecar Pattern
- Adapter Pattern
- Ambassador Pattern

Sidecar: Deploying a logging application along with a web application to forward the logs to the central log server.

Adapter: If multiple applications generate different formats of logs, it requires us to convert them to a common format.
Then we can use an adapter container to convert the logs to a common format.

Ambassador: Imagine we have three dbs for dev, stage and prod. Then if we need to always refer to the localhost(dev) db
and proxy it to the correct db, we can use the ambassador design pattern.

However, implementation is always identical for above design patterns.

---
[Replication Controllers & Replica Sets](replication.md)

[Back](index.md)
