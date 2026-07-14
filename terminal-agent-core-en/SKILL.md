---
name: terminal-agent-core-en
author: linmowudie
email: 1711406673@qq.com
description: Development guide for the Core decision layer of terminal AI Agents. Covers main loop design, middleware pipeline, model routing, prompt construction, and configuration management.
---

# Core Decision Layer Development Guide

## Design Philosophy

The Core layer is the brain of the Agent, with minimalist responsibilities: call the model, execute tools, return results. All cross-cutting concerns (protection, compression, persistence, statistics, etc.) are injected through middleware pipelines, keeping the main loop unaware of any protection logic.

## Design Principles

1. **Minimalist Main Loop**: The core does only three things; all protection logic is delegated to middleware
2. **Pluggable Middleware**: Configuration files control which middleware is enabled, supporting hot-swapping
3. **Lazy Loading**: Heavy components are initialized only on first use to avoid cold start overhead
4. **Configuration-Driven**: All parameters are read from configuration files and environment variables, never hardcoded

## Main Loop Design

Loop flow:
1. Receive user message, update session state, trim history for protection
2. Build shared middleware context, execute pre-processing
3. Inject dynamic context, build system prompt
4. Enter minimalist loop: call model, execute tools if needed and continue, return if no tool calls
5. Execute post-processing, return final answer

Key constraints:
- Tool results must be passed completely to the model, not truncated at the core layer
- Trim earliest messages when history exceeds limits
- Middleware can modify tool results and can also terminate the loop

## Middleware System

### Lifecycle

Middleware intervenes in the main loop through five hooks: before run, before each iteration, after tool execution, after each iteration, after run.

### Shared Context

All middleware operates on the same data by reference, not by copying. Includes message history, termination signals, and extension data mount points.

### Extension Method

Create middleware implementation files, register in the registry, and declare enabled in configuration files.

## Model Router

Unified interface, shielding multi-cloud differences:
- **Primary Model**: Conversation and tool calls, supporting multi-provider fallback
- **Lightweight Model**: Cost optimization scenarios like summarization and compression, caching clients by parameters
- **Embedding Model**: Vectorization and similarity computation
- **Fallback Chain**: Lightweight model failure falls back to primary model, then simple truncation

## System Prompt Construction

Load files from prompt module directories, assemble system prompts by module. Typical modules include: role definition, core capabilities, workflow, tool catalog, dynamic context template, constraint rules, code standards, response format. Configuration controls which modules are enabled.

## Configuration Management

Multi-source layered loading: global default configuration, specific configuration overrides defaults, environment variables handle sensitive information. Supports referencing environment variables in configuration.

## Key Considerations

- Clients must be lazy-loaded to avoid triggering heavy library cold starts on import
- Middleware pipeline executes in configured order; order affects behavior
- All cross-cutting concerns should be implemented as middleware rather than invading the main loop
