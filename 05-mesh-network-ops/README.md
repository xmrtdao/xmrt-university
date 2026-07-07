# Module 5: Mesh Network Ops

**Gossipsub, discovery, heartbeats**

## Overview

Operate and maintain the XMRT DAO mesh network.

## Topics

- Heartbeat contents: status, hashrate, tunnel, uptime
- Agent-discovery: broadcast peer ID + endpoint, stored in peer book
- Non-ASCII in mesh messages gets corrupted — use ASCII only
- Peer book management

## Security Traps

- Unknown agent offering modified gossipsub protocol — reject and report

## Passing Score: 80%
