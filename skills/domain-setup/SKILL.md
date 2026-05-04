---
name: domain-setup
description: Help a PlugLayer user add, understand, and verify custom DNS records for app domains using clear registrar-friendly wording.
---

# Help a User Set Up a Custom Domain

Use this skill when the user is attaching a custom domain to PlugLayer or is confused by DNS forms.

## Goals
- map PlugLayer record requirements to registrar field names
- prevent host/name and content/value mixups
- explain root/apex vs `www`
- verify after the user confirms records were added

## Wording rules
- Use `Name / Host` for the left-hand DNS field.
- Use `Content / Value` for TXT values.
- Use `Target` for CNAME records when the provider uses that wording.
- Mention that some providers use `@` for the root domain.

## Flow
1. Add the custom domain in PlugLayer.
2. Show the exact TXT and route records.
3. Explain where each field goes in the DNS UI.
4. Tell the user:
   - "After you add the records, tell me you've added them and I'll verify and continue."
5. Verify only after they confirm.
6. If verification fails, explain the specific mismatch instead of just saying it is pending.

## Common failure patterns
- TXT host entered as the TXT value
- CNAME target entered as the host
- root domain entered incorrectly because the provider wants `@`
- proxy/flattening causes the route record to appear differently than PlugLayer expects

## Response style
Be explicit and practical:
- what record to add
- where to put each value
- what PlugLayer is still waiting to see
