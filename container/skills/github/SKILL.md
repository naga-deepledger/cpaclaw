---
name: github
description: Interact with GitHub via the gh CLI — repos, issues, PRs, releases, Actions, gists, and the raw API. Use whenever the user asks about GitHub. Authenticates automatically via the GITHUB_TOKEN environment variable.
allowed-tools: Bash(gh:*), Bash(git:*)
---

# GitHub CLI

All commands authenticate automatically via `GITHUB_TOKEN`. Default output is human-readable; add `--json <fields>` for structured output when parsing.

## Repos

```bash
gh repo view owner/name                 # Summary
gh repo view owner/name --json name,description,stargazerCount
gh repo list <owner> --limit 20         # List a user/org's repos
gh repo clone owner/name                # Clone into cwd
gh search repos "topic:llm language:ts" --limit 10
```

## Issues

```bash
gh issue list   --repo owner/name --state open --limit 20
gh issue view   <number> --repo owner/name
gh issue create --repo owner/name --title "..." --body "..."
gh issue comment <number> --repo owner/name --body "..."
gh issue close  <number> --repo owner/name
```

## Pull requests

```bash
gh pr list   --repo owner/name --state open
gh pr view   <number> --repo owner/name
gh pr create --repo owner/name --title "..." --body "..." --head branch --base main
gh pr checkout <number>                  # Checkout locally
gh pr diff   <number> --repo owner/name
gh pr review <number> --repo owner/name --approve|--request-changes|--comment -b "..."
gh pr merge  <number> --repo owner/name --squash|--merge|--rebase
```

## Actions / workflows

```bash
gh run list   --repo owner/name --limit 10
gh run view   <run-id> --repo owner/name --log
gh run watch  <run-id> --repo owner/name
gh workflow list --repo owner/name
gh workflow run  <workflow.yml> --repo owner/name --ref main
```

## Releases & gists

```bash
gh release list   --repo owner/name
gh release view   v1.2.3 --repo owner/name
gh gist create file.txt --public
gh gist list
```

## Raw API (for anything gh doesn't wrap)

```bash
gh api user                                          # Authenticated user
gh api repos/owner/name/commits --paginate
gh api graphql -f query='{ viewer { login } }'
```

## Conventions

- Confirm before destructive writes: merging PRs, closing issues, deleting branches, force-pushing.
- Prefer `--json` + `--jq` over regex parsing of human output.
- For operations on the current repo, `gh` auto-detects from `git remote`; pass `--repo owner/name` when cwd isn't a clone.
- Token scope limits what works — if a command errors with 403/404, the token likely lacks that permission.
