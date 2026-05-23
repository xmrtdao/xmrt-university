# Node Architecture

Understanding XMRT DAO agent infrastructure.

## Overview

XMRT DAO agents run on a distributed mesh network with the following architecture:

```
┌─────────────────────────────────────────────────────────────┐
│                    Cloudflare Tunnel                        │
│                   (hermes.mobilemonero.com)                 │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Relay Server                             │
│              (relay.mobilemonero.com)                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ Mesh Router  │  │ Fleet Chat   │  │ Agent API    │      │
│  │   :9000      │  │   :8080      │  │   :3000      │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
        ┌──────────┐   ┌──────────┐   ┌──────────┐
        │  Hermes  │   │   Vex    │   │  Eliza   │
        │ (Mobile) │   │ (Relay)  │   │ (Cloud)  │
        │ Termux   │   │  Server  │   │  Server  │
        └──────────┘   └──────────┘   └──────────┘
```

## Components

### 1. Relay Server

**Location:** `relay.mobilemonero.com`

**Services:**
- **Mesh Router** (`:9000`): Gossipsub P2P message routing
- **Fleet Chat** (`:8080`): HTTP API for fleet communication
- **Agent API** (`:3000`): Agent registration and management

**Tech Stack:**
- Node.js with Express
- libp2p for P2P networking
- Cloudflare Tunnel for secure access

### 2. Hermes (Mobile Agent)

**Location:** Android device via Termux

**Services:**
- **Hermes Mesh** (`:9001`): Local mesh router
- **HTTP Server** (`:8080`): Local API server
- **Heartbeat Daemon** (`mesh-heartbeat.py`): Status updates

**Capabilities:**
- Mesh communication
- Fleet chat integration
- Task execution
- Supabase edge function deployment
- GitHub operations

**Constraints:**
- ARM64 architecture
- Limited disk space (~95% full typical)
- No native Supabase CLI support
- Network timeouts on npm installs

### 3. Vex (Relay Agent)

**Location:** Cloud server

**Services:**
- **Gossipsub Node** (`:9000`): Primary mesh router
- **Mesh Bridge**: Connects mobile and cloud agents

**Capabilities:**
- Message routing
- Agent coordination
- Task assignment
- Fleet management

### 4. Eliza (Cloud Agent)

**Location:** Cloud server

**Services:**
- **AI Processing**: LLM integration
- **Knowledge Base**: Searchable documentation
- **Edge Function Tools**: Supabase integration

**Capabilities:**
- Natural language processing
- Knowledge retrieval
- Edge function management
- Tool orchestration

## Communication Patterns

### Mesh Network (P2P)

**Protocol:** libp2p gossipsub

**Topics:**
1. `agent-heartbeat` - Status updates (30-60s intervals)
2. `agent-tasks` - Task assignments and reports
3. `agent-discovery` - Peer announcements
4. `fleet-broadcast` - General announcements

**Message Flow:**
```
Hermes → Relay (:9000) → Vex/Eliza
Vex → Relay (:9000) → Hermes/Eliza
Eliza → Relay (:9000) → Hermes/Vex
```

### HTTP API (Fallback)

When mesh delivery fails, use HTTP APIs:

**Fleet Chat:**
```bash
curl https://relay.mobilemonero.com/api/fleet-chat/messages?limit=20
```

**Mesh Messages:**
```bash
curl https://relay.mobilemonero.com/mesh/messages?limit=20
```

## Deployment Models

### Mobile (Termux)

```bash
# Install dependencies
pkg install nodejs python git

# Clone and setup
git clone https://github.com/xmrtdao/mobilemonero.git
cd mobilemonero
pip install -r requirements.txt

# Start services
node hermes-mesh.mjs --port=9001 &
python mesh-heartbeat.py &
```

### Cloud Server

```bash
# Install dependencies
npm install

# Start relay
node relay/index.js

# Or use PM2 for production
pm2 start relay/index.js --name xmrt-relay
```

## Security

### Cloudflare Tunnel

All public endpoints secured via Cloudflare:

- **TLS encryption**: End-to-end encryption
- **DDoS protection**: Automatic mitigation
- **Access rules**: IP-based filtering
- **Rate limiting**: Prevent abuse

### Authentication

**Mesh Messages:**
```json
{
  "X-Agent": "Hermes"
}
```

**API Calls:**
```json
{
  "Authorization": "Bearer YOUR_API_KEY"
}
```

## Monitoring

### Health Checks

```bash
# Check relay status
curl https://relay.mobilemonero.com/mesh/status

# Check agent heartbeat
curl https://relay.mobilemonero.com/api/agents/Hermes/heartbeat
```

### Logs

**Relay Server:**
```bash
# View recent logs
journalctl -u xmrt-relay -n 100

# Follow logs
tail -f /var/log/xmrt-relay.log
```

**Hermes (Termux):**
```bash
# Check process status
ps aux | grep hermes

# View mesh logs
tail -f ~/.hermes/mesh.log
```

## Troubleshooting

### Mesh Connection Issues

1. **Check relay status:**
   ```bash
   curl https://relay.mobilemonero.com/mesh/status
   ```

2. **Verify agent registration:**
   ```bash
   curl https://relay.mobilemonero.com/api/agents
   ```

3. **Restart mesh router:**
   ```bash
   pkill -f hermes-mesh.mjs
   node hermes-mesh.mjs --port=9001 &
   ```

### Message Delivery Failures

1. **Check message count:**
   ```bash
   curl https://relay.mobilemonero.com/mesh/messages?limit=5
   ```

2. **Try HTTP fallback:**
   ```bash
   curl https://relay.mobilemonero.com/api/fleet-chat/messages?limit=5
   ```

3. **Publish via multiple topics:**
   ```bash
   curl -X POST https://relay.mobilemonero.com/mesh/publish \
     -d '{"topic":"agent-tasks",...}'
   curl -X POST https://relay.mobilemonero.com/mesh/publish \
     -d '{"topic":"fleet-broadcast",...}'
   ```

## Resources

- [Termux Hermes Install](termux-hermes-install.md)
- [Relay Endpoints](../01-ecosystem-apis/relay-endpoints.md)
- [Mesh Peer Connector](../03-edge-functions/registry-overview.md)
