---
name: terminal-agent-services-en
author: linmowudie
email: 1711406673@qq.com
description: Development guide for the Services layer of terminal AI Agents. Covers context management, caching strategies, semantic retrieval, session/task/skill management, exploration engine, and hook system.
---

# Services Layer Development Guide

## Design Philosophy

The Services layer is the service hub of the Agent, responsible for context management, caching, retrieval, and persistence. All resource reading and retrieval is completed through a unified hub, with each manager initialized on demand.

## Core Hub

The hub is the core of the entire service layer, providing four types of capabilities:

### Unified Resource Reading

Unified entry point, routing to corresponding managers by resource type. Flow: check cache, route by type, write to cache, return.

### Semantic Retrieval

Supports multi-level fallback strategy chain: cloud vector search, local vector index, database aggregation fallback, keyword matching fallback. Each strategy implements a unified interface; when the previous strategy fails, it automatically falls back to the next.

Scenarios where retrieval is skipped: explicit file references, no keywords, query too short.

### Dynamic Context Injection

Assembled by priority zones, from non-compressible system instructions to truncatable historical dialogue, allocated level by level.

Key mechanisms:
- **Budget Control**: When exceeded, compress from lowest to highest priority
- **Incremental Update**: Reuse last result when input hasn't changed
- **Routing Table**: Build file index from metadata, sort by keyword relevance, support time-based caching

### File Access Recording

Synchronous quick check for deduplication, background threads execute summarization and vectorization, not blocking the main flow. Uses elimination mechanism to prevent infinite record growth. Vectorization uses chunking strategy: prefer by paragraph, if too long by line, then by character window.

## Manager Pattern

Each manager uses lazy loading plus optional external injection: initialized on first access to avoid startup overhead. Supports external injection for easier testing and flexible initialization. Calls model and embedding services uniformly through the hub, not holding clients directly.

## Pre-Request Hooks

Executes preprocessing before core handling: keyword extraction, file path recognition, task recognition, semantic query text extraction.

## Key Considerations

- File records use modification time for deduplication; files can be re-indexed after modification
- Embedding interfaces have batch limits that must not be exceeded
- File vectorization chunking strategy must consider different file formats
