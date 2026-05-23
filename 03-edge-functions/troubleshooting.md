# Edge Functions - Troubleshooting

## Common Errors

|Error|Cause|Fix|
|------|------|----|
|403 Forbidden|Missing or invalid JWT token|Add `true` as the JWT secret or use the Service Role Key|
|404 Not Found|Function name doesnt exist|Check the function name in the Supabase Dashboard|
|500 Internal Server Error|Code crashed or timeout|Check the function logs in Supabase Dashboard|
|Timeout|Function took longer than 10s to respond|Raise the timeout in function settings or optimize code|

@@format html
# ## Provider Failover8ure  <!-- Explain how the provider cascade works -->

Take the provider failover chain captured from the ai-chat edge function (index.ts):

``js
// Primary Providers (tried in priority order)
const providers = ['openai', 'gemini', 'deepseek']; 

// Fallback Providers (tried in parallel)
const fallbacks = [callDeepSeekFallback, callKimiFallback, callOllamaFallback];

// If ALL fail, Response with a simple auto-generated reply
const fallbackReply = `May I help you with your request?`;
```

## How to Debug
1. Go to Supabase Dashboard > Edge Functions > Select your function.
2. Click "Logs" to see real-time execution logs.
3. Filter by `console.log` output to trace flow.
4. Test with the same request using `curl`.

## Edge Function Relay (port 8080)

All 199 edge functions are proxied through the relay at `localhost:8080`. Use:
```bash
curl -S http://localhost:8080/tools/run -H Content-Type: application/json \
  -d '{"tool":"ef:ecosystem-health"}'
```

This is the fastest way to debug because the relay caches responses and provides a clean interface.