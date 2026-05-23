# Fetching Full Email Body from Inbox

## Problem

The relay's inbox endpoint returns **metadata only** — sender, subject, timestamp, preview text — but not the full email body content.

## Solution

### Method 1: Relay Inbox Endpoint
```bash
curl https://relay.mobilemonero.com/inbox/recent
curl https://relay.mobilemonero.com/resend/inbox
```

### Method 2: Direct Resend API
```bash
curl -X GET https://api.resend.com/emails/{email_id} \
  -H "Authorization: Bearer {RESEND_API_KEY}"
```

### Method 3: Relay State Store
```bash
curl http://localhost:8080/state/inbox
```

## Key Takeaway

For the **full body content**, you need either:
- The Resend API key to fetch directly
- Access to the relay's raw webhook log
- Ask Vex to pull it from the local relay state
