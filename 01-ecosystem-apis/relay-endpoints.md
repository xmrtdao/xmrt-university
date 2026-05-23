# Vex Relay — API Endpoints

## Health & Status
```bash
GET  /health          # Basic health check
GET  /status          # Full relay status
GET  /tools           # List all tools
GET  /monitor         # System monitor
```

## Fleet Chat
```bash
POST /api/fleet-chat/send       # Send a fleet message
GET  /api/fleet-chat/messages   # Poll fleet chat history
```

## Mesh Networking
```bash
POST /mesh/init      # Initialize gossipsub node
GET  /mesh/status    # Node status, peer ID, topics
POST /mesh/publish   # Publish to a topic
GET  /api/mesh/peers # Discover active peers
```

## Tools
```bash
POST /tools/run      # Execute a tool
POST /web-search     # Web search
POST /scrape         # Web scraping
POST /ollama/chat    # Chat with local LLM
```
