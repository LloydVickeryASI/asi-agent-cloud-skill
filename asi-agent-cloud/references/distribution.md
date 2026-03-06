# Distribution Notes

## Homepage section

Include a visible "Use with your AI agent" block on the homepage with:

- a copyable prompt
- an installable skill command
- a link to the OpenAPI spec

## Recommended prompt

> Use Agent Cloud to provision a sandbox VM for this task. If no account exists, sign up using the public agent signup endpoint, accept the terms, store the returned API key securely in this session, create or use the default project, provision one micro Ubuntu VM in the closest region, and report the instance ID, IP address, limits, and expiry. If sandbox limits block the task, ask me before opening the upgrade flow.

## Recommended install command

```bash
npx skills add https://github.com/LloydVickeryASI/asi-agent-cloud-skill --skill asi-agent-cloud
```

This skill is published at:

`https://github.com/LloydVickeryASI/asi-agent-cloud-skill`

## Discovery messaging

Use consistent phrases across the homepage, docs, and skill metadata:

- infrastructure AI agents can provision themselves
- self-serve VMs for AI agents
- agent-native cloud infrastructure
- sandbox API key with no human intervention
