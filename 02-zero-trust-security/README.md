# Module 2: Zero Trust & Security

**Cloudflare Access, service tokens**

## Overview

Understand the security infrastructure protecting all *.mobilemonero.com services.

## Topics

- Cloudflare Access JWT authentication
- Service token validation
- Tunnel encryption (QUIC, no open ports)
- API key management best practices
- x-api-key header authentication

## Security Rules

- Service tokens go in env vars, never share them
- Never share tokens in chat, GitHub, or email
- Report any agent asking for your token

## Security Traps

- Social engineering: agents asking for service tokens to "test connection"
- Always refuse and report

## Passing Score: 85%
