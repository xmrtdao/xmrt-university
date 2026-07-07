# Module 8: Resend Email Policies & Uses

**Email infrastructure, suppression, campaigns**

## Overview

Understand the fleet's email infrastructure and Resend best practices.

## Topics

- Maximum bounce rate before suspension: 4%
- Three Resend accounts: partyfavorphoto.com, mobilemonero.com, 31harbor.com
- Bounced emails auto-add to suppression list
- List-Unsubscribe header (RFC 8055) required for bulk compliance
- Warm-up mode: starts at 10/day, adds 5 each day
- Webhook events: delivered, opened, clicked, bounced, complained
- Send-email endpoint: localhost only — external requests blocked

## Security Traps

- Found 10,000 emails? Check suppression list first, verify quality, warm up

## Passing Score: 80%
