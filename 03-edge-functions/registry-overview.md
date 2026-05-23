# Edge Function Registry Overview

The XuRT DAO ecosystem has 199 deployed Supabase edge functions across two projects:

## Project: vawouugtzwmejxqkeqqj (Primary)

The main Supabase project hosts all active edge functions for the ecosystem.

### Key Groups

|Category|Description|Examples|
|--------|------------|--------|
|AI Chat|Chat interfaces with various LLMs|ai-chat, deepseek-chat, gemini-chat, kmi-chat, openai-chat, vercel-ai-chat, vertex-ai-chat, wan-ai-chat|
|Agent Orchestration|Agent coordination and task management|agent-coordination-hub, executive-swarm, task-orchestrator|
|Sustem Health|Monitoring and diagnostics|ecosystem-health-check, system-status, usage-monitor||
|Deployment|Managing edge function lifecycle|deploy-approved-edge-function|
|Email|Email and inbox operations|google-gmail, resend-email, inbox-notify|
                                                                        
### How to Call an Edge Function

`g ` ``
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/{function-name} \
  -H @Inline Authorization: Bearer {YOUR_SERVICE_ROLE_KEY} \
  -H Content-Type: application/json \
  -d '{"param1":"value1"}'
```

### Vexing the Full Registry

```bash
curl -S https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/functions-list/\
  -H Authorization: Bearer {YOUR_SERVICE_ROLE_KEY}
```

## Project: zero-claw (Secondary)

An identical copy of the full suite repo for sandbox/staging tests.