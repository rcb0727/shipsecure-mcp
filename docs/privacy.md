# Privacy

[Overview](../README.md) · [New here?](basics.md) · [Guard](guard.md) · [Ship](ship.md) · [Setup](setup.md) · [Tools](tools.md) · [Security](security.md) · **Privacy** · [Configuration](configuration.md) · [Changelog](../CHANGELOG.md)

**In short:** ShipSecure runs on your computer. There is no ShipSecure server, no account, no telemetry and no tracking. The author never sees your code, your settings or how you use it.

## What it never collects

- **No telemetry:** no usage statistics, crash reports or analytics of any kind.
- **No account:** there is nothing to sign up for with ShipSecure.
- **Your code stays yours:** ShipSecure never uploads your code anywhere. It reaches GitHub and npm only when you push or publish, the same as without ShipSecure.
- **Your keys stay yours:** a token only ever goes to the service it belongs to. Your GitHub sign-in goes to GitHub, your npm sign-in to npm, your Snyk sign-in to Snyk.

## What stays on your computer

| What | Where |
|---|---|
| Your settings | `~/.config/shipsecure-mcp/config.json`, readable only by you |
| Its own notes (last releases, open alerts) | `~/.config/shipsecure-mcp/state.json` |
| Tokens, if you keep any | The system credential store: macOS Keychain, Windows Credential Manager, or the Linux Secret Service |
| npm sign-in and approval links while they wait | `~/.config/shipsecure-mcp/npm-links/`, readable only by you |
| API snapshots, if you use `api_watch` | `~/.config/shipsecure-mcp/api-watch/`, readable only by you |

Delete that folder, and any tokens you stored, and nothing about you is left.

## What it connects to, and why

| Service | When | What it sends |
|---|---|---|
| **GitHub** (through the `gh` command, signed in as you) | When it reads or changes your repositories: pull requests, releases, checks, alerts | What those actions need. Your code goes there only when you push |
| **npm** | To look up packages, download ShipSecure and the npm version it uses, and publish your package if you ship | Package names and versions; your package when you publish |
| **Snyk** | Each dependency scan, if you use Snyk | Your dependency lists (manifests and lockfiles), never your source code. Snyk's own usage analytics are switched off |
| **OSV.dev** | Each dependency scan without Snyk, and for languages Snyk is not asked about | Package names and versions only |
| **ntfy** | Only if you turn on phone alerts | The alert text, with private details removed and no links |
| **Socket.dev** | Only if you set it up | Package names and versions only |
| **Your own API sources** | Only if you set up `api_watch` | Read-only requests to the addresses you list |

`secret_scan` connects to nothing: it reads your repository on your computer.

## Your AI assistant

ShipSecure's answers go to your assistant (Claude, ChatGPT or another) as part of your chat, like anything else you share there. That provider's privacy policy covers them. Keys and passwords in those answers are always masked, never shown in full.

## Questions

Ask in the [issues](https://github.com/rcb0727/shipsecure-mcp/issues).
