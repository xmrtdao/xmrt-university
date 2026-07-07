# Module 10: XMRT-DAO Endpoints & Tool Calling

**Relay tools, security levels, API usage**

## Overview

Learn the relay's 69 tools, their security levels, and how to call them.

## Topics

- 69 relay tools available
- Three security levels: CORE, TRUSTED, PUBLIC
- CORE tools require Cloudflare Access service token or x-api-key
- db-query: read-only SQL against local Postgres
- edge-function: proxy calls to Supabase edge functions
- obsidian-graph: full ecosystem knowledge graph with live status
- fleet-chat: send messages with agent and channel

## Security Traps

- Agent claiming to be "vex" via x-agent-id header — require service token or x-api-key

## Passing Score: 80%
