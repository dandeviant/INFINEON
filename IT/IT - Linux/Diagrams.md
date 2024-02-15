---
cssclasses:
  - mermaid
---


```mermaid
pie title My Everyday Life
"Cybersecurity": 45
"Networks": 45
"Sleep": 10
```


#### TCP Handshake
```mermaid
sequenceDiagram

Sender -->> Receiver: SYN
Receiver -->> Sender: SYN-ACK
Sender -->> Receiver: ACK
```

#### UDP Handshake
```mermaid
sequenceDiagram

Receiver -->> Sender: Request
Sender -->> Receiver: Response
Sender -->> Receiver: Response
Sender -->> Receiver: Response
```

