---
title: payment 1
deprecated: false
hidden: false
metadata:
  robots: index
---
```mermaid
sequenceDiagram
    participant Alice
    participant Bob

    box rgb(249, 249, 249) Test Group
        participant Service
    end

    Alice->>Service: Test request
    Service-->>Alice: Test response
```
