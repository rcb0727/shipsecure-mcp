# Security

[Overview](../README.md) · [New here?](basics.md) · [Guard](guard.md) · [Ship](ship.md) · [Setup](setup.md) · [Tools](tools.md) · **Security** · [Privacy](privacy.md) · [Configuration](configuration.md) · [Changelog](../CHANGELOG.md)

**In short:** ShipSecure never stores your passwords or keys in your code or its settings, checks everything before it leaves your computer, and stops instead of guessing when something is unclear.

## Secrets

- **Nothing secret is in the repository or the config.** Tokens live in the system credential store, or in environment variables on a headless server.
- **Snyk** keeps its own browser sign-in, and shipsecure-mcp never holds it. A scan sends Snyk the dependency manifest and lockfile, not source code.
- **npm:** trusted publishing needs no token, and staged publishing uses your own two-hour sign-in. The token fallback goes through a throwaway npmrc only you can read, and it can be deleted after the day's last publish.
- **A token pasted into chat is never used:** you are told to revoke it.

## What is scanned, and when

| What | When | For |
|---|---|---|
| Every added line of every pushed commit | Each `git push` (pre-push check) | Credentials |
| Files Claude writes | Each write (Claude Code hook) | Credentials, in files git would commit |
| A repository's files and whole history | `secret_scan` | Credentials |
| Every file npm would publish | Preflight | Credentials, environment files, keys, `.npmrc`, git data, source maps that expose a private repository's source |
| Every line bound for a public repository | The public sync | Credentials, ticket keys, names, hosts, GUIDs, private IPs, emails, your own terms |
| Dependencies | Every scan, push, tag and release | Known vulnerabilities |

Credentials are always shown masked, never in full.

## Safeguards

- **Dry run first:** nothing changes until a tool is called with `confirm: true`.
- **Fail closed:** a scanner that cannot run, a scan that cannot finish, a missing staged record or a moved branch stops the step. None of them is ever treated as clean.
- **Untrusted repositories:** git runs with every setting that could start a program switched off, and gitleaks uses its own rules.
- **Least access for builds:** builds and checks get only PATH, HOME and locale variables, plus what the project lists. A Dependabot branch is built only at the exact commit reviewed.
- **Untrusted text stays data:** issue and pull request text, dependency changelogs and web pages are never taken as instructions.
- **One session at a time:** a lease per project keeps a scheduled run and a chat from changing the same project at once.

## A private or a public repository

- **Private:** npm shows `repository` to everyone, so it must not name the private repository.
- **Public, with trusted publishing:** npm adds provenance and refuses the publish unless `repository` names that repository exactly.

`setup_status` and `ship_preflight` check both.
