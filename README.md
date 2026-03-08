# GateRift

**AI API gateway for developer teams.**

GateRift sits between your application and AI providers (OpenAI, Anthropic, and more), giving your team a single, auditable control plane for every AI call.

## What it does

| Capability | Details |
|---|---|
| **Unified gateway** | Route any OpenAI-compatible request through a single `/v1/chat/completions` endpoint |
| **API key management** | Issue, rotate, and revoke per-team or per-application keys |
| **Rate limiting** | Org-level and key-level rate limits enforced via Redis |
| **PII detection** | Automatic scanning via Microsoft Presidio before requests are forwarded |
| **Policy enforcement** | OPA-powered rules — restrict models, providers, or request patterns per org |
| **Audit trail** | Every request is logged to Kafka and stored with cryptographic integrity |
| **Multi-provider** | OpenAI and Anthropic today; additional providers via the adapter layer |

## Repositories

| Repo | Description |
|---|---|
| [`entrepreneur-platform`](https://github.com/girisenji/entrepreneur-platform) | Mono-repo containing all backend services |
| [`gaterift-cli`](https://github.com/girisenji/gaterift-cli) | Command-line interface for managing keys, config, and testing |

## Architecture overview

```
Your app
   │
   ▼
gateway-service  (Spring Boot 3.5.11 · Java 21 virtual threads)
   ├── pii-scanner sidecar  (Python FastAPI · Microsoft Presidio)
   ├── policy-engine        (OPA rules)
   ├── provider-adapters    (OpenAI / Anthropic)
   └── management-api       (key/org management · shared with TraceKeep)
```

Event bus: Redpanda (Kafka-compatible)  
Database: PostgreSQL 15  
Cache / rate-limit: Redis 7  
Analytics: ClickHouse

## License

[Apache 2.0](LICENSE)