---
layout: post
title: Microservices vs. Multi-agents?
date: 2026-06-30 15:09:00
description: a concise comparison of two ways of decomposing complex systems
tags: architecture ai
categories: systems
featured: false
---

Microservices and multi-agent systems both break a problem into independent units. The difference is what those units are made of: fixed contracts versus reasoning.

```
microservice:  request -> fixed logic -> response (deterministic)
agent:         goal -> reasoning + tools -> action (adaptive)
```

**Architecture.** A microservice does exactly what its API says, every time. An agent pursues a goal and decides how, using context and available tools.

**Communication.** Microservices talk through strict schemas (REST, gRPC). Calls fail loudly on mismatch. Agents talk in natural language. Flexible, but failures are soft, an agent can "misunderstand" output in a way a JSON payload cannot.

**Failure modes.** Microservices fail in known, enumerable ways: timeouts, dropped connections. You build retries and circuit breakers. Agents fail messier: confident wrong answers, reasoning loops, errors compounding downstream. Observability here is still immature.

**When to use which.**

| | Microservices | Multi-agents |
|---|---|---|
| Problem shape | Stable, decomposable | Fuzzy, open-ended |
| Priority | Predictability, testability | Adaptability |
| Failure | Enumerable | Compounding |

**My take.** These aren't competitors. The better systems use agents for the ambiguous, judgment-heavy parts of a workflow, then hand off to microservices for execution and anything that needs to be deterministic and auditable. The skill isn't picking a paradigm, it's drawing the boundary between what needs reasoning and what needs determinism.