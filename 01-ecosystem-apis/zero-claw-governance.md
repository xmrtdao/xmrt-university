# Zero-Claw Governance

Zero-Knowledge governance system for XMRT DAO.

## Overview

Zero-Claw is XMRT DAO's ZK-powered governance framework that enables private, verifiable voting and proposal management.

## Edge Functions

| Function | Description |
|----------|-------------|
| `propose-action` | Create new governance proposals |
| `submit-vote` | Submit encrypted votes |
| `tally-votes` | Calculate results with ZK proofs |
| `list-function-proposals` | Query active proposals |

## CLI Tool

Use `zero-claw-cli.mjs` to interact with governance from the command line.

### Installation

```bash
git clone https://github.com/xmrtdao/zero-claw.git
cd zero-claw
```

### Commands

#### Propose

Create a new governance proposal:

```bash
node zero-claw-cli.mjs propose \
  --title "Add new feature" \
  --proposed-by hermes \
  --description "Detailed description of the proposal"
```

#### Vote

Submit your vote on a proposal:

```bash
node zero-claw-cli.mjs vote \
  --proposal-id 123 \
  --vote yes \
  --voter hermes
```

#### Tally

Calculate vote results:

```bash
node zero-claw-cli.mjs tally --proposal-id 123
```

#### Check

Check proposal status:

```bash
node zero-claw-cli.mjs check --proposal-id 123
```

#### Status

Get overall governance status:

```bash
node zero-claw-cli.mjs status
```

## Authentication

All commands require Supabase authentication. Set your credentials:

```bash
export SUPABASE_URL=https://vawouugtzwmejxqkeqqj.supabase.co
export SUPABASE_ANON_KEY=your-anon-key
```

## ZK Proofs

Zero-Claw uses zero-knowledge proofs to:
- Keep votes private while proving validity
- Prevent double-voting
- Enable transparent tallying without revealing individual votes

## Resources

- [Zero-Claw Repo](https://github.com/xmrtdao/zero-claw)
- [Governance Docs](https://github.com/xmrtdao/zero-claw/blob/main/GOVERNANCE.md)
- [ZK Proof Spec](https://github.com/xmrtdao/zero-claw/blob/main/ZK-SPEC.md)
