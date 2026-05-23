# Zero-Claw CLI

Command-line interface for interacting with the zero-claw governance edge functions.

## Commands

### propose
Create a new proposal:
```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/propose-action \
  -H "Content-Type: application/json" \
  -d '{"title":"...","description":"...","proposed_by":"Vex","threshold":2}'
```

### vote
Cast a vote on a proposal:
```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/submit-vote \
  -H "Content-Type: application/json" \
  -d '{"proposal_hash":"...","nullifier_secret":"...","vote":1}'
```

### tally
Count votes for a proposal:
```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/tally-votes \
  -H "Content-Type: application/json" \
  -d '{"proposal_hash":"..."}'
```

### check
Check your vote status:
```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/check-vote \
  -H "Content-Type: application/json" \
  -d '{"proposal_hash":"...","voter":"Vex"}'
```

### status
Check governance system health:
```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/propose-action \
  -H "Content-Type: application/json" \
  -d '{"action":"status"}'
```

## Tips
- Use a unique `nullifier_secret` per vote to prevent double voting
- Proposals need `threshold` votes to pass
- All functions return JSON responses
