# Agent Cloud API Quickstart

## Signup

`POST /v1/agent/signup`

Minimal request:

```json
{
  "agent_name": "ChatGPT",
  "agent_type": "chatgpt",
  "terms_accepted": true
}
```

Minimal success response:

```json
{
  "account_id": "acc_123",
  "project_id": "prj_123",
  "api_key": "asi_sandbox_...",
  "tier": "sandbox",
  "limits": {
    "max_active_instances": 1,
    "max_vcpu": 1,
    "max_memory_gb": 1,
    "expires_at": "2026-03-13T00:00:00Z"
  }
}
```

## Provision a VM

`POST /v1/instances`

Suggested request:

```json
{
  "project_id": "prj_123",
  "name": "agent-worker-1",
  "region": "akl1",
  "image": "ubuntu-24.04",
  "size": "micro"
}
```

## Poll readiness

`GET /v1/instances/{instance_id}`

Report the instance only after it enters a ready state and returns network details.

## Upgrade path

If sandbox limits are insufficient, request human approval before using:

`POST /v1/billing/checkout-session`
