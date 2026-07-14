---
name: terminal-agent-tools-en
author: linmowudie
email: 1711406673@qq.com
description: Development guide for the Tools layer of terminal AI Agents. Covers declarative tool creation, unified loading, and three types of tool implementations: readers, writers, and executors.
---

# Tools Layer Development Guide

## Design Philosophy

The Tools layer is responsible for encapsulating underlying execution capabilities as structured functions for the model to use through function calling mechanisms. Uses factory pattern for unified creation and declarative definition of tool specifications.

## Tool Classification

Classified by operation nature into three types:

### Readers
Read-only operations, do not modify workspace. Return error information on failure, not affecting subsequent execution.

### Writers
Modify workspace content. Trigger cache invalidation after successful writes, automatically record file access.

### Executors
Execute system commands with polymorphic returns (synchronous completion or background waiting). Timeout prompts guide the model to use background terminals for long-running commands. Trigger cache invalidation after command execution.

## Development Flow

### Implement Underlying Functions

Create functions in corresponding classification directories, returning unified format: include content on success, include error description on failure, include status indicators for multi-status scenarios.

### Register Tools

Complete three steps in the loader: import underlying functions, inject common parameters through pre-binding, define wrapper functions and register. The wrapper function's parameter signature is what the model sees, must have clear type annotations and descriptions.

### Format Output

Use factory-provided universal formatters to avoid repeatedly writing success/failure judgment logic. Choose appropriate formatters based on tool type, or customize.

## Multi-Channel Execution Strategy

Provides three execution channels: synchronous terminal for quick commands, background terminal for long-running tasks and environment preservation, visible terminal for interactive programs and graphical applications.

## Key Considerations

- Description fields must include explanations for all parameters
- Underlying functions are not aware of workspace root path, injected through pre-binding
- Tool result prefix format for middleware to identify error states
- Background terminal tools return command identifiers for subsequent queries when returning waiting status
