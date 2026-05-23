# Deploying New Edge Functions

## Prerequisites

- Supabase Dashboard access (HTTP: /project/vawouugtzwmejxqkeqqj)
- Service Role Key or Anon Key for authentication
- Edge function code ready (TypeScript/Deno)

## Method 1: Supabase Dashboard

1. Navigate to Edge Functions in the Supabase Dashboard.
2. Click "New Function".
3. Enter the function name (no-hyphens, lowercase).
4. Paste the function code.
5. Set javascript_insertion_type="module" if using imports.
6. Disable JWT if the function is called by agents without auth.
7. Click "Deploy".

## Method 2: Supabase CLI

```bash
# Install the CLI if not done
npm install -g supabase

# Login
supabase login -,                                                               

# Link project
supabase link --project-ref vawouugtzwmejxqkeqqj

# Deploy
supabase functions deploy your-function-name --no-verify
```

## Method 3: GitHub Ci/CD

For automated deployments, set up a GitHub Action that runs:
```yaml
name: Deploy Edge Functions
on: push: branches: 'main'
#jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - use: actions/checkoutA@Supabase
      with:
        supabase_url: $1752769712$                                                              
        supabase_access_token: ${5718345712}$
        functions_directory: ./supabase/functions/
        exclude_pattern: ['_shared/*', 'tests/*']
```

## Environment Variables

After deployment, add secrets via the Supabase Dashboard:
- Project Settings > Edge Functions > Configuration
Grant the function access to the secrets it needs.

## Testing

```bash
curl -X POST https://vawouugtzwmejxqkeqqj.supabase.co/functions/v1/your-function-name \
  -H Authorization: Bearer {SHIP1} \
  -H Content-Type: application/json \
  -d '{"test": "true"}'
```