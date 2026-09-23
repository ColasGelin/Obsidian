# MCP Summary

Core protocol connect LLM apps to external data/tools. Client ↔ Server ↔ (resource/tool/prompt).

## Resources
Read-only data server expose behind URI (e.g. `docs://documents`). App fetch direct — deterministic, no model decide, no side effects.

![[MCP Resource.canvas]]

## Tools (workflow)
Tool = action model can invoke (has side effects), unlike resource. Client-LLM-Server roundtrip to pick, call, execute tool.

![[MCP workflow.canvas]]

## Prompts
Pre-written message template server expose, invoked by **user** (slash command/button) — model never pick on own. Client list prompts, user pick + give args, server fills template, returns ready-made user/assistant messages for client to drop in conversation.

![[MCP prompts.canvas]]

## Sampling
Server request LLM completion *through* client — server got no direct model access, no connection to Claude. Server do own work first (e.g. fetch data), craft prompt, send sampling request to client. Client calls Claude, relays generated text back to server. Server use text to build its actual final response, hands back to client.

![[MCP Sampling.canvas]]

## Roots
Permission + file-discovery mechanism — client tell server which directories/URIs it allowed to touch. Server can't search filesystem blind (e.g. resolve bare "biking.mp4" to full path) without it: server ask client for approved roots (`roots/list`), search inside them (`read_dir`), then call real tool w/ resolved path. SDK don't enforce automatic — server code must validate path stay inside approved root.

![[MCP Roots.canvas]]

## Upcoming topics (to fill)
- JSON Messages (JSON-RPC message format)
- Logging
- Progress
