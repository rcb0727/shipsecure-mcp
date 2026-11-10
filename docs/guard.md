# Guard

[Overview](../README.md) · [New here?](basics.md) · **Guard** · [Ship](ship.md) · [Setup](setup.md) · [Tools](tools.md) · [Security](security.md) · [Privacy](privacy.md) · [Configuration](configuration.md) · [Changelog](../CHANGELOG.md)

**In short:** Guard looks for passwords and keys in your code, old versions included, checks the libraries you use for known security holes, and stops new keys from being uploaded. It works on any project on GitHub, in any language, and never publishes anything.

## Find leaked credentials

Ask your agent to "scan this repo for secrets", or run `secret_scan` on any local checkout. It works even before setup. It reads:

- **The files as they are now:** tracked files, and untracked files git does not ignore, since the next commit could pick those up. Gitignored files such as `.env`, where credentials belong, are left alone.
- **Every commit on every branch and tag:** a credential committed once stays in the history after a later commit removes it.

Each finding is masked (its kind, a short prefix and its length), with the file, the line and the commit that added it. It also says whether the credential is still in the files. The report says what to do, in this order:

1. Revoke and rotate each real credential at the service that issued it.
2. Take the ones still in the files out: an environment variable, a gitignored `.env`, or the system credential store.
3. Rewriting history (`git filter-repo`) only hides a credential from new clones. Old clones, forks and caches keep it, so rotating is what makes it harmless.

A scan that could not finish is never reported as clean. The repository may be anyone's, so no program its own settings name ever runs during a scan. [gitleaks](https://github.com/gitleaks/gitleaks) adds its rules when it is installed.

## Stop new leaks

- **Git pre-push check:** every line that each pushed commit adds is scanned. A credential stops the push, whoever pushes, and its value is never shown.
- **Claude Code hook:** stops Claude from writing a credential into a file git would commit.

The setup wizard turns both on.

## Scan dependencies

`vuln_scan` reads these lockfiles at the repository root:

| Ecosystem | Lockfiles | Answered by |
|---|---|---|
| npm | `package-lock.json` | Snyk (or OSV.dev) and npm audit |
| Python | `poetry.lock`, `uv.lock`, `Pipfile.lock`, `requirements*.txt` with exact `==` pins | OSV.dev; Snyk too for `poetry.lock` |
| Go | `go.mod` with `go.sum` | OSV.dev |
| Rust | `Cargo.lock` | OSV.dev |
| Ruby | `Gemfile.lock` | OSV.dev and Snyk |
| PHP | `composer.lock` | OSV.dev and Snyk |
| .NET | `packages.lock.json` | OSV.dev |

Anything it cannot read in full counts as a failed scan, never a pass. That covers an unpinned requirement, a lockfile it cannot parse, and a manifest without its lockfile, such as Maven, Gradle, or `go.mod` without `go.sum`. Lockfiles in subfolders are listed as not scanned.

## Check the repository

`repo_check` reports the GitHub settings that keep a repository safe, with how to fix each warning:
- Dependabot alerts and security updates
- secret scanning and push protection
- branch protection or a ruleset
- the default workflow token
- per workflow, a permissions block, actions pinned to a commit SHA, and no `pull_request_target`

## Guard-only projects

A configured project with no npm package is guard-only. The scans, the hooks, `repo_check`, `local_gate` and `ship_push` work as usual. The release tools say in one line that they do not apply. The daily check reports a fixable runtime vulnerability as PATCH: fix it, and nothing ships.
