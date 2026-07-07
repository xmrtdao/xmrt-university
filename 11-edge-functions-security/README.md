# Module 11: Supabase Edge Functions & Security

**Edge function catalog, abuse vectors, security**

## Overview

Understand the 242 deployed edge functions and their security implications.

## Topics

- 242 edge functions deployed
- universal-invoke: can call any function — potential abuse vector
- generate-payment-link: Stripe payment links for subscription tiers
- cron-proxy: proxies requests to cron-managed edge functions
- Edge functions with service keys have admin DB access if exposed
- auth-health: checks authentication system health
- opportunity-scanner: scans for business opportunities

## Security Traps

- Edge function with no auth and full DB access — report immediately

## Passing Score: 85%
