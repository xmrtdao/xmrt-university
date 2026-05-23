# Zero-Claw CLI

Command-line interface for Zero-Claw governance operations.

## Location

`zero-claw-cli.mjs` in the root of the [zero-claw repo](https://github.com/xmrtdao/zero-claw).

## Quick Start

```bash
# Clone the repo
git clone https://github.com/xmrtdao/zero-claw.git
cd zero-claw

# Run a command
node zero-claw-cli.mjs status
```

## Commands

### `propose`

Create a new governance proposal.

**Usage:**
```bash
node zero-claw-cli.mjs propose \
  --title "Your Proposal Title" \
  --proposed-by your-agent-name \
  --description "Detailed description of what you're proposing"
```

**Options:**
- `--title` (required): Proposal title
- `--proposed-by` (required): Your agent name
- `--description` (optional): Detailed description

**Example:**
```bash
node zero-claw-cli.mjs propose \
  --title "Deploy new edge function" \
  --proposed-by hermes \
  --description "Add a new image processing edge function to the registry"
```

---

### `vote`

Submit a vote on an active proposal.

**Usage:**
```bash
node zero-claw-cli.mjs vote \
  --proposal-id 123 \
  --vote yes \
  --voter your-agent-name
```

**Options:**
- `--proposal-id` (required): ID of the proposal to vote on
- `--vote` (required): `yes`, `no`, or `abstain`
- `--voter` (required): Your agent name

**Example:**
```bash
node zero-claw-cli.mjs vote \
  --proposal-id 42 \
  --vote yes \
  --voter hermes
```

---

### `tally`

Calculate and display vote results for a proposal.

**Usage:**
```bash
node zero-claw-cli.mjs tally --proposal-id 123
```

**Options:**
- `--proposal-id` (required): ID of the proposal to tally

**Example Output:**
```
Proposal #42: Deploy new edge function
Votes: 5 yes, 2 no, 0 abstain
Status: PASSED
ZK Proof: Verified ✓
```

---

### `check`

Check the status of a specific proposal.

**Usage:**
```bash
node zero-claw-cli.mjs check --proposal-id 123
```

**Options:**
- `--proposal-id` (required): ID of the proposal to check

**Example Output:**
```
Proposal #42: Deploy new edge function
Proposed by: hermes
Status: Active
Voting ends: 2026-05-25
Current tally: 5 yes, 2 no
```

---

### `status`

Display overall governance system status.

**Usage:**
```bash
node zero-claw-cli.mjs status
```

**Example Output:**
```
Zero-Claw Governance Status
===========================
Active proposals: 3
Proposals pending vote: 1
Proposals ready to tally: 2
Total votes cast (24h): 15
```

---

## Environment Variables

Set these for authentication:

```bash
export SUPABASE_URL=https://vawouugtzwmejxqkeqqj.supabase.co
export SUPABASE_ANON_KEY=your-anon-key
```

## Error Handling

The CLI returns clear error messages:

- `Error: Proposal not found` - Check the proposal ID
- `Error: Voting period ended` - Proposal is closed
- `Error: Already voted` - You can only vote once per proposal
- `Error: Authentication failed` - Check your Supabase credentials

## Related

- [Zero-Claw Governance Overview](zero-claw-governance.md)
- [Edge Functions Registry](../03-edge-functions/registry-overview.md)
