# Changelog

All notable changes to shipsecure-mcp are recorded here. Versions follow [semantic versioning](https://semver.org): patch for fixes, minor when the product changes shape, major when something breaks.

## [0.1.2] - 2026-11-22

### Changed

- **The public sync moves the public tool counts itself.** The first "N tools" claim in each `public:` `toolCountFiles` entry follows the private count at the tag, in the same docs commit. Before, a count change meant a hand edit and an extra commit in the public repository, which then failed preflight's "public repo level with its remote". Preflight now reports differing public counts as information, not a warning.
- **`ship_npm_publish` takes `waitSeconds` (10 to 300)** as a shorter alternative to `waitMinutes`, so an approval wait polls in short rounds and a fresh link reaches the maintainer sooner.
- **The ship-day guide adds a prep list, batching and a links rule.** Take every Dependabot PR before the word to ship (a conflicting one gets a rebase comment and is not waited for), draft the RELEASING.md paragraph early, batch each step across projects, and paste every sign-in or approval link into the chat itself the moment it exists.
- **A ship tests the same content once.** A gate that passed in a clean checkout is remembered by the commit's content, the gate and install commands, the repository and the Node version and machine it ran on, so `ship_push` and `local_gate` do not run it again on a commit with the same tree within the hour, and still record the result. `reuseGate: false` in a project always runs it. When a project verifies on your computer, `ship_release_commit` commits first and gates that commit in a clean checkout, undoing the commit if the gate fails. A failed gate is never remembered.
- **A ship scans each commit once.** `ship_tag_release` reuses a clean scan of the same commit from `ship_push` within the last ten minutes and says so; a scan with a gap or a blocking finding is never reused. The verification report still carries the scan summary.
- **Staged publishing checks who staged a version.** A checksum, listed by npm or remembered here, counts only when the account signed in now is the one that staged it, and a stage npm names no account for is refused.
- **The public docs commit message is scanned for private data** like every other line bound for the public repository.
- **On Linux, the npm approval runs under `/bin/sh`**, whatever your own shell is.
- **The example config leaves `autoShip` off**, so a scheduled run ships on its own only once you turn it on.
- **The ship-day guide starts the npm sign-in first** and runs the steps that do not wait for npm side by side while npm reviews a staged package.

- **Development dependency update:** vitest 5.1.1.

## [0.1.1] - 2026-11-10

### Fixed

- **Approving a staged release from the chat hands you the link.** npm gives its approval link only when it runs in a terminal, so shipsecure-mcp now runs `npm stage approve` in one on macOS and Linux. Before, the link came back masked and you had to approve from a terminal yourself.
- **A release you approved is verified even after npm forgets the stage.** npm keeps no record of a staged version once it is approved, so the next check had nothing to compare and refused. shipsecure-mcp now remembers the checksum when it stages the package and checks what npm serves against it.
- **The public docs commit names its release** even when the release tag was re-made on a later commit.
- **The docs say how to update**, and the public repository holds the documentation only; the package is on npm.

## [0.1.0] - 2026-11-10

### Added

- **Guard any git repository, in any language.** `secret_scan` finds credentials in the files and in every commit on every branch and tag, masked, with the commit that added each one and what to rotate. `vuln_scan` reads Python, Go, Rust, Ruby, PHP and .NET lockfiles besides npm, through OSV.dev and Snyk. A repository with no npm package can be a guard-only project: the scans, hooks and repository checks work as usual and the release tools stay out of the way.
- **The first public release: an MCP server and agent skill that also ships npm packages from GitHub securely, with 30 tools.** It covers the patch cycle (Snyk and npm audit scans, fixes in a worktree, Dependabot updates with a supply-chain review, a gate on GitHub Actions or in a clean local checkout), ship day (preflight, the release commit, tags and GitHub Releases, the npm publish, the public docs sync and a six-artifact check), development on feature branches that ship only once you approve them, and scheduled releases with a daily check.
- **Nothing private leaves the machine.** Every line bound for a public repository is scanned for private data, every file npm would publish is scanned for credentials, and the package is checked for environment files, keys, `.npmrc`, git data and source maps that would expose a private repository's source. A git pre-push check and a Claude Code hook keep credentials out of pushes and tracked files.
- **Releases you can audit.** Every GitHub Release carries a CycloneDX SBOM of the runtime dependencies and a verification report: the commit, what verified it, the scan totals, the SBOM's SHA-256 and how npm got the version.
- **No npm token needed.** Trusted publishing is the default. Without GitHub Actions, staged publishing stages the package from your computer and hands you one-tap links: an npm sign-in at the start, and an approval per package that you confirm with 2FA.
- **One checklist, start to finish.** Say "ship it" and the agent runs every step, reads each dry run itself, and reports one checklist. It stops only when something is unexpected.
- **A Snyk account is needed for the default scanner.** The Snyk CLI runs only when signed in, and the setup wizard signs you in through your browser (a free account works). Without one, `snyk.mode` `auto` or `off` scans with OSV.dev and npm audit instead.
- **A brand-new public repository takes its first commit from the public sync**, with the released files only and none of the source repository's history.
- **Works with Claude Code and ChatGPT (formerly Codex)**, on macOS, Windows and Linux, with the system credential store on each.
