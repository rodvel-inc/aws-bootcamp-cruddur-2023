# Week 2 — Distributed Tracing

For software, there is debugging. For everything else, there is `Observability`.

We have logs, log streams, but monitoring and observability go beyond that.

We give software instuctions to tell us what is going on under the hood.

**`Distributed tracing`** is the standard in **`Modern observability`**, but it is not about different logs from different components in a non-contextual or non-related structure. 

**`Modern observability`** is about telling a story. The so-called story is better known as the **`trace`**. 

For instance, as a request starts from the backend, it goes through different systems, applications, services, or components. The "marks" left on these elements is what the trace is made up of.

