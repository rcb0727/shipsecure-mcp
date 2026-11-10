# Changelog

All notable changes to shipsecure-mcp are recorded here. Versions follow [semantic versioning](https://semver.org): patch for fixes, minor when the product changes shape, major when something breaks.

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
