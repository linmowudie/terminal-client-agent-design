---
name: terminal-agent-interface-en
author: linmowudie
email: 1711406673@qq.com
description: Development guide for the Interface layer of terminal AI Agents. Covers interaction controller, display port abstraction, terminal rendering, and remote display service.
---

# Interface Layer Development Guide

## Design Philosophy

The Interaction layer is responsible for the user interface, adopting a controller and display backend separation design. The controller only handles command parsing and scheduling, all display is delegated through a unified port, supporting multiple display methods.

## Core Design Principles

1. **Controller Contains No Rendering Logic**: Only responsible for command parsing, scheduling, data preparation, all display delegated through display port
2. **Replaceable Display Backends**: Terminal rendering and remote push implement the same interface
3. **Data-Driven**: Parameters passed to display port are basic types, easily serializable
4. **Event Decoupling**: Decision layer publishes events through event bus, interaction layer subscribes and renders

## Display Port Abstraction

All display backends must implement a unified interface, covering the following capabilities: lifecycle management, startup display, message output, streaming output, user input, information panels.

## Interaction Controller

### Main Loop Flow

Display startup information and create session, loop to get user input. Slash command dispatch processing, non-command input sets up streaming output, calls decision layer, displays thinking process and answers, cleans up streaming.

### Slash Command Handling

Uses command mapping table design, supporting help, reset, configuration view, statistics, history, session management, skill management, todo management, context compression, exit, and other commands. Adding new commands only requires registering in the mapping table.

### Streaming Output Coordination

Subscribes to model output events through event bus, fallback callbacks ensure working even when event bus is unavailable. Must unsubscribe first during cleanup, ensure all output is completed before releasing resources.

## Terminal Rendering

Implements rich text rendering for command-line terminals: markup language rendering and border panels, direct output for real-time streaming, typewriter effect, startup information display, collapsible panels for thinking process, input prompts with history.

## Remote Display Service

Provides remote push interface for external interfaces: implements display port interface, serializes and pushes messages. Message protocol defined independently, supports multiple client connections.

## Adding New Display Backends

Create new files implementing all abstract methods of the display port interface, select display backend in startup script.

## Key Considerations

- Cannot block event bus thread during streaming output, handlers must be lightweight
- Cleanup operations must be executed only after all output is completed, otherwise content will be lost
- Getting user input is a blocking call, display backend decides how to obtain input
- Session management handled in interaction controller, not in display port
- Must release resources and restore terminal cursor state on exit
