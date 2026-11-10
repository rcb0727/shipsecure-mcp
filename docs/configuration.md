# Configuration

[Overview](../README.md) · [New here?](basics.md) · [Guard](guard.md) · [Ship](ship.md) · [Setup](setup.md) · [Tools](tools.md) · [Security](security.md) · [Privacy](privacy.md) · **Configuration** · [Changelog](../CHANGELOG.md)

**In short:** you rarely need this page. The setup writes your settings for you; this is what they mean if you want to change one by hand.

The settings live in `~/.config/shipsecure-mcp/config.json`, readable only by you, and are read on every call. [`config.example.json`](../config.example.json) shows every setting.

## Per project

| Setting | What it is |
|---|---|
| `source` | The repository: local path, `owner/name`, branch and remote |
| `public` | A separate public docs repository that releases sync to |
| `npm` | The package and how it publishes (`trusted`, `staged` or `token`). Leave it out for a guard-only project |
| `gate` | The commands run before every release commit and by the local gate |
| `checks` | Where pushes are verified: `auto`, `actions` or `local` |
| `checksInstall` | The local gate's install command (`npm ci` by default; empty for a repository without npm) |
| `versionFiles`, `changelog`, `docsCommand` | What the release commit bumps, dates and regenerates |
| `syncFiles`, `toolCountFiles`, `forbiddenPublicTerms` | What the public sync copies and checks |
| `dev` | The development branch and its worktree |

## Top level

| Setting | What it is |
|---|---|
| `snyk` | The scanner mode (`required`, `auto`, `off`) |
| `npm` | The registry and the token's credential-store name |
| `redaction` | Your own names and patterns to keep out of public text |
| `secrets` | gitleaks: `auto`, `required` or `off` |
| `maintenance` | The release days, the daily check's time, and whether a run may ship on its own |
| `notify` | Alerts: ntfy topic, desktop, GitHub issues |
| `liveTest`, `apiWatch`, `socket` | Live checks against a test account, API change tracking, Socket.dev scores |
