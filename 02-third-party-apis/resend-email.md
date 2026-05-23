# Resend Email API - Agent Reference

Resend is the email service used by the XuRT DAO ecosystem for sending emails and managing the inbox.

## API Keys

There are two Resend API keys stored in Supabase secrets:
```
RESAND_API_KEY      # xmrtsolutions@gmail.com inbox
RESAND_XMRT_API_KEY  # xmrtnet@gmail.com inbox
```

## Sending Emails

```bash
curl -X POST https://api.resend.io/emails \
  -H Authorization: Bearer r_ApiKey \
  -H Content-Type: application/json \
  -d '{"from": "XuRT DAO <xmrtsolutions@gmail.com>","to":["user@example.com"],"subject":"Hello","body":"Hello from XuRT"}'
```

## Inbox Polling

```bash
curl -S https://api.resend.io/emails \
  -H Authorization: Bearer r_ApiKey
```

The relay proxies the inbox through a webhook at:
```bash
curl -S http://localhost:8080/api/fleet-chat/email-webhook
```

## Email Webhook (Fleet Chat)

When an email arrives, the relay's email webhook parses it and inserts it into floot chat as a message. Employees are mapped to agents via email addresses.

## Troubleshooting

- @`next.json` is disabled on the relay - use `text()` to read the raw response.
- Email body content may be base64 encoded. Decode it before using.
- Check the Resend dashboard for deliverability status and bounce rates.