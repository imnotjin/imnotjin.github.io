---
layout: post
title: Stop Writing Types You Already Have
date: 2026-07-01 11:20:00
description: how openapi-typescript saved my frontend dev from writing type definitions by hand
tags: openapi backend
categories: code
featured: false
---

Today I watched a frontend dev carefully hand-write TypeScript type definitions to match a FastAPI response model I had just shown him in Swagger. It was meticulous, correct, and completely unnecessary.

I've been building full-stack apps inside a single framework my whole career. When the frontend and backend live together, type sharing is just... solved. You never think about it. Split the repos, and suddenly you have two people maintaining the same schema in two different languages.

That's exactly what OpenAPI is for, and I only realized this today.

**The setup**

```
backend/   FastAPI + Pydantic response models -> /openapi.json
frontend/  Next.js + TypeScript
```

FastAPI exposes an OpenAPI spec at `/openapi.json` automatically. That spec is language-agnostic, a complete description of every endpoint, request, and response shape.

**The fix**

```bash
npx openapi-typescript http://XXX:XXX/openapi.json -o src/types/api.d.ts
```

One command. The frontend now has accurate, auto-generated types derived directly from the backend's source of truth. No drift, no duplication, no one manually transcribing a Pydantic model into a TypeScript interface.

**Why it clicked late**

Full-stack frameworks hide this problem entirely. Next.js API routes, tRPC, Remix loaders: they all let you share types across the boundary as if it isn't there. When I finally worked on a project with separate repos and separate languages, I had no intuition for it and defaulted to "just document it and sync manually."

The lesson isn't really about a library. It's that a well-documented API is already a contract, and contracts can be turned into code. You just need the right tool to read them.