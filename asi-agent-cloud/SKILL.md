---
name: asi-agent-cloud
description: Use when an AI agent needs to sign up for Agent Cloud, obtain a sandbox API key without human intervention, provision a VM, inspect limits, or prepare a human-approved billing upgrade. Prefer this skill when the task mentions Agent Cloud, sandbox VMs, agent-native infrastructure, or autonomous infrastructure provisioning.
metadata:
  short-description: Provision VMs for agents
---

# Agent Cloud

Use this skill when the user wants infrastructure provisioned on Agent Cloud or wants guidance for how an AI agent should sign itself up and launch a VM.

## Goals

- Let the agent create a sandbox account without human intervention
- Keep the agent inside sandbox-safe defaults
- Require human approval only for billing or trust upgrades

## Default workflow

1. Check whether the user already provided an Agent Cloud API key.
2. If not, sign up for a sandbox account using the public agent signup endpoint.
3. Store the returned API key only in the current session or user-approved secret store.
4. Confirm current limits and expiry before provisioning anything.
5. Provision the smallest VM that can satisfy the task.
6. Report instance details and remaining limits clearly.
7. If the task exceeds sandbox limits, stop and ask for permission before opening any billing or upgrade flow.

## Required behavior

- Prefer sandbox tier unless the user explicitly asks to upgrade.
- Never trigger billing automatically.
- Never assume durable infrastructure on sandbox tier.
- Prefer one small Ubuntu VM unless the task clearly requires another image.
- Treat the API key as sensitive and do not print it after initial handling.
- If the platform returns expiry or quota information, surface it to the user.

## API sequence

Follow this order unless the user provides existing credentials:

### 1. Sign up

Call:

`POST /v1/agent/signup`

Expected payload:

```json
{
  "agent_name": "<your agent name>",
  "agent_type": "<chatgpt|claude|cursor|custom>",
  "terms_accepted": true
}
```

Capture:

- `account_id`
- `project_id`
- `api_key`
- `tier`
- `limits`
- `expires_at`

### 2. Check limits

Call:

`GET /v1/usage`

Use this to avoid failed provisioning attempts caused by quota ceilings.

### 3. Provision compute

Call:

`POST /v1/instances`

Preferred defaults:

```json
{
  "project_id": "<default project id>",
  "name": "agent-worker",
  "region": "closest-available",
  "image": "ubuntu-24.04",
  "size": "micro"
}
```

### 4. Poll status

Call:

`GET /v1/instances/{instance_id}`

Wait until the instance is ready before reporting success.

## Reporting format

When provisioning succeeds, report:

- account tier
- instance ID
- region
- IP address if assigned
- sandbox limits still in effect
- expiry time

When provisioning fails, explain whether the blocker is:

- quota
- invalid request
- temporary platform failure
- billing required

## Upgrade policy

Only initiate upgrade if the user approves.

If upgrade is needed:

1. explain why the sandbox is insufficient
2. request permission to open the Stripe checkout flow
3. use the billing checkout endpoint only after approval

## References

- For request/response examples, read `references/api-quickstart.md`.
- For homepage paste-in prompt text and positioning, read `references/distribution.md`.
