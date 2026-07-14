# Terminal AI Agent Skills 

A comprehensive skill set for developing terminal-based AI Agents with a five-layer architecture design.

## Author

**linmowudie**  
Email: 1711406673@qq.com

## Skills Overview

This skill set provides development guides for building terminal AI Agents, organized by the five-layer architecture pattern. Each skill focuses on design philosophy and architectural concepts rather than specific implementation details or technology stacks.

### 1. Core Decision Layer (`terminal-agent-core-en`)

The brain of the Agent with minimalist responsibilities. Covers:
- Main loop design (call model → execute tools → return results)
- Middleware pipeline system with five lifecycle hooks
- Model router for multi-cloud support with fallback chains
- System prompt construction from modular components
- Configuration management with multi-source layered loading

**Key Design Principles:**
- Minimalist main loop delegates all protection logic to middleware
- Pluggable middleware controlled by configuration
- Lazy loading to avoid cold start overhead
- Configuration-driven, no hardcoded parameters

### 2. Services Layer (`terminal-agent-services-en`)

The service hub responsible for context management and resource orchestration. Covers:
- Unified resource reading with caching
- Multi-level semantic retrieval with fallback strategies
- Dynamic context injection with priority-based assembly
- File access recording with background vectorization
- Manager pattern with lazy loading and external injection

**Key Design Principles:**
- All resource access through unified hub
- Budget control for token management
- Incremental updates to avoid redundant computation
- Pre-request hooks for preprocessing

### 3. Tools Layer (`terminal-agent-tools-en`)

Encapsulates execution capabilities as structured functions for model invocation. Covers:
- Three tool classifications: Readers, Writers, Executors
- Declarative tool creation with factory pattern
- Unified tool loading and registration
- Multi-channel execution strategy (synchronous, background, visible)
- Standardized output formatting

**Key Design Principles:**
- Factory pattern for unified tool creation
- Pre-binding injection for common parameters
- Wrapper functions define model-visible signatures
- Polymorphic returns for different execution scenarios

### 4. Infrastructure Layer (`terminal-agent-infra-en`)

Provides all underlying capabilities for the system. Covers:
- Persistent storage with metadata-vector separation
- Local vector index as fallback solution
- Multi-channel terminal execution (synchronous, background, visible, sandbox)
- File system operations with encoding degradation
- File monitoring with debouncing
- Event bus for layer decoupling
- Logging system with automatic rotation

**Key Design Principles:**
- Database stores only paths and metadata, not file content
- Workspace isolation through hash-based separation
- Platform-specific interfaces with cross-platform awareness
- Project root anchoring for all path resolution

### 5. Interface Layer (`terminal-agent-interface-en`)

Handles user interaction with controller-display separation. Covers:
- Display port abstraction for multiple backends
- Interaction controller for command parsing and scheduling
- Slash command handling with mapping table design
- Streaming output coordination with event bus
- Terminal rendering and remote display service

**Key Design Principles:**
- Controller contains no rendering logic
- Replaceable display backends implementing unified interface
- Data-driven parameters for easy serialization
- Event decoupling between decision and interaction layers

## Architecture Philosophy

The five-layer architecture follows strict separation of concerns:

```
┌─────────────────────────────────────┐
│      Interface Layer                │  User interaction
├─────────────────────────────────────┤
│      Core Decision Layer            │  Main loop & middleware
├─────────────────────────────────────┤
│      Services Layer                 │  Context & resource management
├─────────────────────────────────────┤
│      Tools Layer                    │  Execution capabilities
├─────────────────────────────────────┤
│      Infrastructure Layer           │  Underlying system capabilities
└─────────────────────────────────────┘
```

**Key Architectural Concepts:**
- **Layer Independence**: Each layer can be developed and modified independently
- **Abstraction First**: Upper layers depend on abstractions, not implementations
- **Event-Driven Decoupling**: Layers communicate through events, not direct calls
- **Configuration-Driven**: Behavior controlled by configuration, not code changes

## Design Philosophy Highlights

### No Technology Stack Exposure

All skill documents focus purely on design concepts and architectural principles, without exposing specific technology choices, frameworks, or implementation details. This makes the skills applicable to various technology stacks.

### Pure Concept Description

The skills describe what each layer should achieve and why, not how to implement it with specific libraries or tools. This approach:
- Enables technology-agnostic understanding
- Supports adaptation to different tech stacks
- Focuses on architectural thinking
- Facilitates knowledge transfer

## Usage

These skills are designed to be used with Qoder or similar AI assistants to guide the development of terminal-based AI Agents. So, you can also find a consolidated version of this skill in the QoderCN skill marketplace. They provide:

- **Architectural Guidance**: Understand how to structure a terminal AI Agent
- **Design Principles**: Learn the key principles for each layer
- **Extension Patterns**: Know how to add new capabilities
- **Best Practices**: Follow proven design patterns

## License

© 2026 linmowudie. All rights reserved.
