# Tools

[Overview](../README.md) · [New here?](basics.md) · [Guard](guard.md) · [Ship](ship.md) · [Setup](setup.md) · **Tools** · [Security](security.md) · [Privacy](privacy.md) · [Configuration](configuration.md) · [Changelog](../CHANGELOG.md)

**In short:** these are the 30 tools your assistant can use. You do not call them yourself: you ask in plain words and the assistant picks the right one. Every tool that would change something first shows a dry run (a preview) and waits for the go-ahead (`confirm: true`).

## Guard

| Tool | Changes | What it does |
|---|---|---|
| `secret_scan` | nothing | Finds passwords and keys in a project's files and every old version, and says which to replace |
| `vuln_scan` | nothing | Checks the libraries a project uses for known security holes: npm, Python, Go, Rust, Ruby, PHP, .NET |
| `repo_check` | nothing | Checks the project's GitHub safety settings and says how to fix each one |
| `redaction_scan` | nothing | Checks text for keys and private details (names, hosts, ticket numbers, emails) before it goes public |
| `local_gate` | a status on GitHub | Runs the project's tests on your computer, in a fresh copy of what was uploaded, and records the result |

## Setup and status

| Tool | Changes | What it does |
|---|---|---|
| `setup_status` | nothing | Says what is set up on this computer and what is missing, even before setup |
| `snyk_status` | nothing | The Snyk sign-in, its org and when its token expires |
| `trusted_publisher_check` | starts one workflow run | Proves the npm trusted publisher setup without publishing anything |

## Patch cycle

| Tool | Changes | What it does |
|---|---|---|
| `dependency_review` | nothing | What a Dependabot update changes, at runtime or not, with its supply-chain signals |
| `dependabot_apply` | a commit on main | Lands a Dependabot update with the full gate, keeping Dependabot's lockfile |
| `build_compare` | throwaway worktrees | Builds two versions and compares their tool catalogs, optionally with a live call |
| `patch_status` | nothing | Whether each project is pushed, green and has nothing fixable left |
| `land_fix` | main | Lands a reviewed fix branch with the same checks as a push |
| `ship_push` | an upload | Uploads your commits to GitHub, has them tested, then scans them again |

## Ship day

| Tool | Changes | What it does |
|---|---|---|
| `ship_preflight` | nothing | Everything that must be true before a release, including the package contents |
| `ship_release_commit` | a commit | Bumps every version file, dates the changelog, runs the gate; all or nothing |
| `ship_tag_release` | a tag and a GitHub Release | Tags and releases, with an SBOM and a verification report |
| `ship_npm_publish` | npm | Publishes (trusted, staged with an approval link, or token) and verifies what npm serves |
| `ship_public_sync` | a local commit | Copies the release to the public docs repository after scanning every added line |
| `ship_status` | nothing | The state of a project and its six-artifact check |
| `release_deprecate` | GitHub Releases | Recovers from a bad version: the npm commands for you, then the Releases |

## Development

| Tool | Changes | What it does |
|---|---|---|
| `dev_status` | nothing | Feature branches in progress, finished work on develop, what main lacks |
| `propose_feature` | a draft pull request | Presents a finished feature for your approval |
| `finish_feature` | develop | Merges a finished feature into develop on your word |
| `finish_approved` | develop | Merges every feature you approved, at the commit you approved |
| `dev_sync` | develop | Brings develop up to date with main |
| `promote` | main | Moves main to develop once everything on it is finished |

## Schedules and alerts

| Tool | Changes | What it does |
|---|---|---|
| `maintenance_check` | nothing | The release decision per project: FINISH, RELEASE, CVE, PATCH or IDLE |
| `api_watch` | snapshots | Snapshots the API surfaces a project is built on and reports what changed |
| `needs_you` | an alert | Tells you a run stopped for you: phone, desktop and a GitHub issue |

Your assistant reads each tool's full behaviour from the `ship` skill that installs with the package.
