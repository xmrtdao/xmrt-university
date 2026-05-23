# Resend Email API

Send transactional emails via Resend.

## Overview

Resend is XMRT DAO's email service provider for:
- Contract delivery
- Quote notifications
- System alerts
- Agent communications

## API Endpoint

```
POST https://api.resend.com/emails
```

## Authentication

```bash
Authorization: Bearer YOUR_RESEND_API_KEY
```

## Request Format

```json
{
  "from": "XMRT DAO <onboarding@resend.xmrtdao.com>",
  "to": ["client@example.com"],
  "subject": "Your Contract is Ready",
  "html": "<p>Your contract is attached.</p>",
  "attachments": [
    {
      "filename": "contract.pdf",
      "content": "base64-encoded-content",
      "contentType": "application/pdf"
    }
  ]
}
```

## Response

```json
{
  "id": "49a3999c-0ce1-4ea6-ab68-afcd6dc2e794",
  "from": "XMRT DAO <onboarding@resend.xmrtdao.com>",
  "to": ["client@example.com"],
  "created_at": "2026-05-23T04:40:00.000Z"
}
```

## Reusable Agent Skill

A reusable Node.js script is available at:

**Location:** `relay/resend-email-skill.mjs` in the [mobilemonero repo](https://github.com/xmrtdao/mobilemonero).

### Usage

```bash
# Send simple email
node resend-email-skill.mjs \
  --to client@example.com \
  --subject "Contract Ready" \
  --body "Your contract is attached."

# Send with attachment
node resend-email-skill.mjs \
  --to client@example.com \
  --subject "Quote #123" \
  --body "Please find your quote attached." \
  --attachment /path/to/quote.pdf
```

### Options

| Option | Default | Description |
|--------|---------|-------------|
| `--to` | (required) | Recipient email |
| `--subject` | (required) | Email subject |
| `--body` | (required) | Email body (HTML supported) |
| `--from` | XMRT DAO | Sender name |
| `--attachment` | (none) | Path to attachment |
| `--reply-to` | (none) | Reply-to address |

### Programmatic Use

```javascript
const { sendEmail } = require('./resend-email-skill.mjs');

await sendEmail({
  to: 'client@example.com',
  subject: 'Contract Ready',
  html: '<p>Your contract is attached.</p>',
  attachments: [
    {
      filename: 'contract.pdf',
      path: '/path/to/contract.pdf'
    }
  ]
});
```

## Common Use Cases

### 1. Contract Delivery

```javascript
await sendEmail({
  to: clientEmail,
  subject: `Contract #${contractId} - Ready for Review`,
  html: `
    <h1>Your Contract is Ready</h1>
    <p>Hi ${clientName},</p>
    <p>Your contract is ready for review and signature.</p>
    <p>Please review the attached PDF and let us know if you have any questions.</p>
    <p>Best regards,<br/>XMRT DAO Team</p>
  `,
  attachments: [{
    filename: `contract-${contractId}.pdf`,
    path: contractPath
  }]
});
```

### 2. Quote Notification

```javascript
await sendEmail({
  to: clientEmail,
  subject: `Quote #${quoteId} - ${projectName}`,
  html: `
    <h1>Quote Ready</h1>
    <p>Hi ${clientName},</p>
    <p>Thank you for your interest in our services.</p>
    <p>Please find your detailed quote attached.</p>
    <p>This quote is valid for 30 days.</p>
    <p>Best regards,<br/>XMRT DAO Team</p>
  `,
  attachments: [{
    filename: `quote-${quoteId}.pdf`,
    path: quotePath
  }]
});
```

### 3. System Alert

```javascript
await sendEmail({
  to: 'admin@xmrtdao.com',
  subject: `⚠️ Alert: ${alertType}`,
  html: `
    <h1>System Alert</h1>
    <p><strong>Type:</strong> ${alertType}</p>
    <p><strong>Time:</strong> ${new Date().toISOString()}</p>
    <p><strong>Details:</strong> ${alertDetails}</p>
    <p>Please investigate immediately.</p>
  `
});
```

## Environment Variables

Set your Resend API key:

```bash
export RESEND_API_KEY=re_your_api_key_here
```

## Error Handling

The skill handles common errors:

- **Invalid API key**: Returns authentication error
- **Rate limiting**: Waits and retries automatically
- **Invalid email**: Returns clear validation error
- **Attachment too large**: Returns size limit error (max 25MB)

## Best Practices

1. **Use HTML emails**: Better formatting and branding
2. **Keep it concise**: Get to the point quickly
3. **Include clear CTAs**: Tell recipients what to do next
4. **Test before sending**: Use test mode for development
5. **Handle bounces**: Monitor bounce rates and clean lists
6. **Respect unsubscribe**: Honor opt-out requests immediately

## Testing

Use Resend's test mode:

```bash
export RESEND_API_KEY=re_test_your_test_key
```

Test emails are sent to a sandbox and don't deliver to real recipients.

## Rate Limits

| Plan | Daily Limit | Per Second |
|------|-------------|------------|
| Free | 100/day | 1/sec |
| Pro | 10,000/day | 10/sec |
| Business | Custom | Custom |

## Related

- [MuAPI Agent Skill](muapi-agent-skill.md)
- [Content Publishing Guide](../07-content-publishing/bot-vs-human-optimized.md)
