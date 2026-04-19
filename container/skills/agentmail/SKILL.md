---
name: agentmail
description: Send and receive emails via the agentmail CLI — inboxes, messages, threads, drafts, pods, webhooks, domains. Use whenever the user asks to send, read, reply to, or manage email. Requires AGENTMAIL_API_KEY in the environment.
allowed-tools: Bash(agentmail:*)
---

# AgentMail CLI

Email management via the `agentmail` command. The `AGENTMAIL_API_KEY` env var is picked up automatically.

Default `--format json` output is fine for parsing. Add `--format pretty` for user-facing previews.

## Inboxes

```bash
agentmail inboxes list
agentmail inboxes create --display-name "Support" --username support --domain example.com
agentmail inboxes retrieve --inbox-id <inbox_id>
agentmail inboxes delete  --inbox-id <inbox_id>
```

## Send / reply / forward

```bash
agentmail inboxes:messages send --inbox-id <id> \
  --to "to@example.com" --subject "Hi" --text "Body"

# HTML: --html "<h1>Hi</h1>"

agentmail inboxes:messages reply   --inbox-id <id> --message-id <mid> --text "..."
agentmail inboxes:messages forward --inbox-id <id> --message-id <mid> --to "x@example.com"
```

## Read

```bash
agentmail inboxes:messages list     --inbox-id <id>
agentmail inboxes:messages retrieve --inbox-id <id> --message-id <mid>
agentmail inboxes:threads  list     --inbox-id <id>
agentmail inboxes:threads  retrieve --inbox-id <id> --thread-id <tid>
```

## Attachments

Pass `--attachment <path>` one or more times on `send`, `reply`, or `forward`:

```bash
agentmail inboxes:messages send --inbox-id <id> \
  --to "to@example.com" --subject "Report" --text "See attached" \
  --attachment /workspace/group/report.pdf \
  --attachment /workspace/group/chart.png
```

Paths must be readable inside the container (typically under `/workspace/group/`).

## Drafts

```bash
agentmail inboxes:drafts create --inbox-id <id> \
  --to "to@example.com" --subject "Draft" --text "..."
agentmail inboxes:drafts send   --inbox-id <id> --draft-id <did>
```

## Advanced (on demand)

Pods, webhooks, and domains exist but are rarely needed. Run `agentmail <topic> --help` when required:

- `agentmail pods --help` — group inboxes (`pods create`, `pods:inboxes create`, `pods:threads list`).
- `agentmail webhooks --help` — event subscriptions (e.g. `message.received`).
- `agentmail domains --help` — add custom domains, verify DNS, fetch zone file.

## Conventions

- Confirm destination, subject, and body with the user before sending externally.
- For replies, always pass `--message-id` (not just inbox) so threading is preserved.
- Prefer listing first (`inboxes list`, `messages list`) to discover IDs rather than guessing.
- All commands accept `--api-key`, `--base-url`, `--environment`, `--format`, `--debug` if overrides are needed.
