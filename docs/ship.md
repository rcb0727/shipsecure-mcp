# Ship

[Overview](../README.md) · [New here?](basics.md) · [Guard](guard.md) · **Ship** · [Setup](setup.md) · [Tools](tools.md) · [Security](security.md) · [Privacy](privacy.md) · [Configuration](configuration.md) · [Changelog](../CHANGELOG.md)

**In short:** Ship releases an npm package for you. You say "ship it"; it tests, scans, publishes and double-checks every part, and shows you a checklist as it goes. It does everything [Guard](guard.md) does, plus the release. It works from a private or public GitHub repository, with or without a separate public docs repository.

## One checklist

Say "ship it", or let the daily check reach a release day. The agent runs every step, reads each dry run itself, and sends you one checklist:

```markdown
Ship my-package 1.4.2
- [x] 1. Preflight: ready
- [x] 2. Approved develop work promoted (or: none waiting)
- [x] 3. Release commit pushed and gated
- [x] 4. Private tag and Release (SBOM, verification report)
- [ ] 5. npm: published and verified
- [ ] 6. Public docs synced and pushed
- [ ] 7. Public tag and Release
- [ ] 8. Six artifacts PASS
You: tap to approve my-package 1.4.2 with 2FA: <link>
```

With trusted publishing you hear from it only at the end. Without GitHub Actions there are two taps and nothing to type: an npm sign-in link at the start, and an approval link per package. It stops early only when something is unexpected.

## The patch cycle

The patch cycle runs on any day, and nothing in it reaches npm or a public repository:

1. **Scan:** `vuln_scan` runs Snyk (or OSV.dev) and npm audit at `HEAD`, exactly what would ship.
2. **Fix:** libraries are upgraded in a separate copy of the project (a worktree), so your working copy is never touched. Dependabot's automatic update requests get a safety review and land on `main` only if they are Dependabot's own work.
3. **Test:** the gate (your project's tests and build) runs on GitHub Actions, or on your computer in a fresh copy of exactly what was committed.
4. **Push and rescan:** `patch_status` says COMPLETE when nothing fixable is left.

## Ship day

1. `ship_preflight`: the version, package contents, credentials, wording, tool counts and a scan. It catches anything the public sync would refuse, before a tag exists.
2. `ship_release_commit`: bumps every version file, dates the changelog and runs the full gate. A failure rolls back every file.
3. `ship_tag_release`: tags the release and creates the GitHub Release, with an SBOM and a verification report.
4. `ship_npm_publish`: publishes and verifies the shasum, tarball version and LICENSE.
5. `ship_public_sync`, then the public push and tag: every added line is scanned for private data.
6. `ship_status`: all six artifacts must pass. Those are the private tag and Release, npm, and the public docs, tag and Release.

## Publishing to npm

| Mode | How | You do |
|---|---|---|
| **Trusted** (default) | The tag push runs your release workflow, which publishes with GitHub's short-lived identity. No npm token. | Nothing |
| **Staged** | This computer stages the package; npm reviews it; you get an approval link. | Tap the sign-in link, then the approval link, and confirm with 2FA |
| **Token** | A per-ship token from the credential store, through a throwaway npmrc. | Store the token |

## With or without GitHub Actions

Every pushed commit is verified, and the project's `checks` setting decides where:

| `checks` | Verified by |
|---|---|
| `auto` (default) | GitHub Actions when the repository runs it on pushes, otherwise the gate on this computer. The local gate also takes over when GitHub won't start the jobs (out of minutes, a failed payment). |
| `actions` | GitHub Actions only |
| `local` | The gate on this computer only |

The local gate runs in a throwaway checkout of the exact commit. It records its result on the commit as the `shipsecure-mcp/gate` status. A real Actions failure is never overridden by a local pass.

## What every Release carries

- **An SBOM:** CycloneDX, of the runtime dependencies.
- **A verification report:** the commit, what verified it, the scan totals, the SBOM's SHA-256, how npm got the version, and who released it.

Both are read back byte for byte before they count. Neither contains links or repository names, so both are safe on a public Release.

## Development branches

Work in progress lives on `feature/<name>` branches. `propose_feature` opens a draft pull request for you. You approve with the `approved` label or in chat, and the approval covers exactly the commit you were shown. Only finished, approved work reaches `develop`, and `promote` moves it to `main` before a ship.

## Release schedule

`maintenance.cadence` sets the release days, for example every two weeks on Thursday. A daily scheduled run calls `maintenance_check`, which answers per project:
- FINISH
- RELEASE
- CVE: a runtime vulnerability in the released version, so it ships at once
- IDLE

A run ships on its own only once npm shows a trusted publish for the package. In the Claude desktop app the schedules are scheduled tasks; in ChatGPT they are automations.

## When it needs you

A staged release waiting for approval, a feature waiting for review, or anything a scheduled run will not decide alone, alerts you:
- a phone push through [ntfy](https://ntfy.sh)
- a desktop notification
- one GitHub issue per waiting item

Alerts carry no links, and their text is redacted first.

## When a release goes bad

`release_deprecate` prints the `npm deprecate` and `npm dist-tag` commands for your own terminal, since both need your 2FA. It then marks the GitHub Releases deprecated and moves GitHub's Latest back to the last good version. Then fix forward with the next patch.
