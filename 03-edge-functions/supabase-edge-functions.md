# Supabase Edge Functions

Serverless functions for XMRT DAO operations on Supabase.

## Overview

Supabase Edge Functions are Deno-based serverless functions that run globally at the edge. They provide:

- **Low latency**: Run close to users
- **Auto-scaling**: Handle traffic spikes automatically
- **TypeScript support**: Full type safety
- **Built-in auth**: Easy integration with Supabase Auth

## Available Functions

### Governance Functions

| Function | Description |
|----------|-------------|
| `propose-action` | Create new governance proposals |
| `submit-vote` | Submit encrypted ZK votes |
| `tally-votes` | Calculate results with ZK verification |
| `list-function-proposals` | Query active proposals |

### Communication Functions

| Function | Description |
|----------|-------------|
| `eliza-relay` | Relay messages between agents |
| `agent-message-bus` | Inter-agent messaging system |
| `pfp-template` | Profile picture generation |

### System Functions

| Function | Description |
|----------|-------------|
| `system-status-cron` | Health monitoring and alerts |
| `__trigger_test__` | Testing and validation |
| `_shared_ai` | Shared AI utilities |

## Deployment

### Prerequisites

```bash
# Install Supabase CLI
npm install -g supabase

# Login
supabase login

# Link to project
supabase link --project-ref vawouugtzwmejxqkeqqj
```

### Deploy a Function

```bash
# Deploy single function
supabase functions deploy function-name

# Deploy all functions
supabase functions deploy --all
```

### Local Development

```bash
# Start local emulator
supabase start

# Serve functions locally
supabase functions serve

# Test a function
curl http://localhost:54321/functions/v1/function-name \
  -H "Authorization: Bearer ANON_KEY" \
  -d '{"key":"value"}'
```

## Function Structure

```
supabase/
├── functions/
│   ├── function-name/
│   │   ├── index.ts          # Main entry point
│   │   ├── deno.json         # Deno configuration
│   │   └── README.md         # Function documentation
│   └── another-function/
└── migrations/
```

### Example: index.ts

```typescript
import { serve } from "https://deno.land/std@0.168.0/http/server.ts"

serve(async (req: Request) => {
  try {
    const { name } = await req.json()
    
    return new Response(
      JSON.stringify({ message: `Hello, ${name}!` }),
      { headers: { "Content-Type": "application/json" } }
    )
  } catch (error) {
    return new Response(
      JSON.stringify({ error: error.message }),
      { status: 400, headers: { "Content-Type": "application/json" } }
    )
  }
})
```

## Authentication

### Anon Key (Public)

```bash
curl https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/function-name \
  -H "Authorization: Bearer YOUR_ANON_KEY" \
  -H "Content-Type: application/json" \
  -d '{"data": "value"}'
```

### Service Role Key (Admin)

⚠️ **Never expose this key client-side!**

```bash
curl https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/function-name \
  -H "Authorization: Bearer YOUR_SERVICE_ROLE_KEY" \
  -H "Content-Type: application/json" \
  -d '{"data": "value"}'
```

## Error Handling

Edge functions should return structured errors:

```typescript
return new Response(
  JSON.stringify({ 
    error: "Invalid input",
    details: "Field 'name' is required"
  }),
  { 
    status: 400,
    headers: { "Content-Type": "application/json" }
  }
)
```

## CORS Configuration

Enable CORS for browser access:

```typescript
const corsHeaders = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Headers': 'authorization, x-client-info, apikey, content-type',
}

// Handle CORS preflight
if (req.method === 'OPTIONS') {
  return new Response('ok', { headers: corsHeaders })
}
```

## Best Practices

1. **Keep functions small**: Single responsibility
2. **Use environment variables**: Store secrets in `.env`
3. **Log everything**: Use `console.log()` for debugging
4. **Handle errors gracefully**: Return structured errors
5. **Rate limiting**: Implement client-side rate limiting
6. **Type safety**: Use TypeScript with strict mode

## Monitoring

### Check Function Health

```bash
curl https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/function-name \
  -H "Authorization: Bearer YOUR_ANON_KEY"
```

### View Logs

```bash
# In Supabase Dashboard
# Go to: Edge Functions → Select function → Logs
```

## Resources

- [Supabase Edge Functions Docs](https://supabase.com/docs/guides/functions)
- [Deno Documentation](https://deno.land/manual)
- [XMRT DAO Function Registry](registry-overview.md)
- [Troubleshooting Guide](troubleshooting.md)
