---
title: Architecture
description: A brief overview of the Tableu Migration SDK architecture.
layout: docs
---

The Migration SDK is designed as a local application that primarily downloads content from Tableau Server to a local machine and then publishes it to Tableau Server. The Migration SDK exclusively uses the Tableau REST API to migrate content. (HTTP reference?)

```mermaid
graph LR
A <--> B
C <--> B
B <--> D
subgraph Tableau Server
A[APIs]
end

subgraph User Application
B[Migration SDK]
end

subgraph Tableau Cloud
C[APIs]
end

D[(Temporary storage)]
```
