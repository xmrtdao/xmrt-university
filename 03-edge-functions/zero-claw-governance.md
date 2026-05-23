# Zero-Claw Governance Edge Functions

The zero-claw governance layer provides decentralized voting and proposal management for XMRT DAO agents.

## Deployed Functions

All deployed at `https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/`

### propose-action
Creates a new proposal for agent voting.

```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/propose-action \
  -H "Content-Type: application/json" \
  -d '{"title":"Upgrade Mining Pool","description":"Switch to new pool","proposed_by":"Vex","threshold":2}'
```

Response includes a unique `proposal_hash` used for voting.

### submit-vote
Cast a vote on a proposal. Uses nullifier-based privacy.

```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/submit-vote \
  -H "Content-Type: application/json" \
  -d '{"proposal_hash":"abc123...","nullifier_secret":"your-secret","vote":1}'
```
- `vote: 1` = yes, `vote: 0` = no

### tally-votes
Count votes for a proposal. Returns yes/no counts and approval status.

```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/tally-votes \
  -H "Content-Type: application/json" \
  -d '{"proposal_hash":"abc123..."}'
```

### check-vote
Check if a specific voter has already voted on a proposal.

```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/check-vote \
  -H "Content-Type: application/json" \
  -d '{"proposal_hash":"abc123...","voter":"Vex"}'
```

## Database Tables

- `proposals` - Stores proposal metadata, status, and vote tallies
- `votes` - Stores individual votes with nullifier dedup

Both tables have RLS enabled with public read policies.
