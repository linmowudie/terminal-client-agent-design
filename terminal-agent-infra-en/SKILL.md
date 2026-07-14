---
name: terminal-agent-infra-en
author: linmowudie
email: 1711406673@qq.com
description: Development guide for the Infrastructure layer of terminal AI Agents. Covers persistent storage, vector indexing, multi-channel terminal execution, file system operations, file monitoring, event bus, and logging system.
---

# Infrastructure Layer Development Guide

## Design Philosophy

The Infrastructure layer provides all underlying capabilities: persistent storage, vector retrieval, command execution, file operations, event communication, and logging. All upper-layer services access system resources through this layer, not directly manipulating underlying details.

## Persistent Storage

Adopts dual storage design separating metadata and vectors, isolated by workspace hash.

Metadata collections store file path indexes, including path, type, language, summary, tags, and other information. Vector collections store embeddings and chunked content for semantic retrieval. Task collections store task management information.

Core principle: Database only stores paths and metadata, not the file content itself.

Indexing strategy: Metadata establishes unique indexes by workspace and path, vectors establish normal indexes by workspace and file identifier, vector search indexes are created separately through scripts.

## Local Vector Index

Fallback solution when cloud vector search is unavailable. Isolated by workspace, each workspace has independent index files. Supports document addition and similarity search.

## Multi-Channel Terminal Execution

### Synchronous Terminal
Cross-platform adaptation, output truncation protection, encoding degradation handling.

### Background Terminal
Maintains execution environment (working directory, environment variables preserved across commands), does not block main flow, suitable for long-running tasks. Hidden process, cannot pop up graphical interfaces or accept user input. Managed uniformly through singleton manager.

### Visible Terminal
Supports interactive programs and graphical applications, supports Chinese commands, captures output through logs. Depends on platform-specific interfaces, cross-platform compatibility needs attention.

### Sandbox Isolation
Isolates execution environment, provides complete lifecycle management.

## File System Operations

Provides safe file reading capabilities, using encoding degradation strategy: prefer universal encoding, gradually degrade to system encoding, finally skip. Automatic encoding detection.

## File Monitoring

Background thread monitors file system events, debouncing mechanism avoids repeatedly triggering indexes for multiple modifications in short time. Automatically excludes version control, dependency management, cache, and other directories. Automatically triggers index updates when files change.

## Event Bus

Lightweight publish-subscribe pattern for decoupling decision layer and interaction layer. Decision layer publishes events (model starts output, token output, tool execution, etc.), interaction layer subscribes and renders.

Design principle: Handlers must be lightweight, only doing interface updates or state recording, decision layer does not depend on interaction layer. Synchronous distribution, handlers cannot perform time-consuming operations.

## Logging System

Multiple recorders log to separate files, distinguished by module name. Automatic rotation to prevent single files from growing too large. Unified log directory management.

## Resource Manager

Anchors project root path, all relative paths are resolved based on this. Cannot use current working directory, must obtain project root through this manager.

## Key Considerations

- Database connection failures should throw explicit exceptions, not silently degrade
- Background terminals use pipe communication, watch for deadlock risks
- Visible terminal depends on platform-specific interfaces, cross-platform compatibility needs attention
- All path resolution must anchor to project root, cannot use current working directory
