# Setup

[Overview](../README.md) · [New here?](basics.md) · [Guard](guard.md) · [Ship](ship.md) · **Setup** · [Tools](tools.md) · [Security](security.md) · [Privacy](privacy.md) · [Configuration](configuration.md) · [Changelog](../CHANGELOG.md)

**In short:** one command sets everything up. It asks a few questions, signs you in to the services it needs in your browser, and connects ShipSecure to your assistant.

## Requirements

- macOS, Windows or Linux, with Node.js 22.19 or later and git
- The GitHub CLI (`gh`), signed in
- A Snyk account (free) for the default dependency scanner (see [Snyk](#snyk))
- An npm account that owns your packages, for the Ship path only
- Claude Code, ChatGPT (formerly Codex), or another MCP client that loads agent skills

Tokens live in the system credential store: the macOS Keychain, Windows Credential Manager, or the Linux Secret Service (`secret-tool`). On a headless server they come from environment variables instead.

## Install

```bash
npx shipsecure-mcp setup
```

The wizard checks your machine, then walks you through the rest. It asks before each change:

1. **GitHub:** signs you in with `gh auth login` if needed.
2. **Your repositories:** lists the git checkouts with a GitHub remote it finds on this machine, for you to pick by number. A repository without a `package.json` is guarded. One with a `package.json` is asked "Ship it to npm too?". For one that ships, it finds the package and the test gate, and asks about a public docs repository, a development branch and the release schedule.
3. **Snyk:** opens Snyk's sign-in page in your browser.
4. **npm publishing**, for each package that ships: trusted publishing (recommended, no token), staged publishing, or a token.
5. **Always-on scanning:** the git pre-push check and the Claude Code hook.
6. **Your agent:** installs the `ship` skill and registers the server in Claude Code, and in ChatGPT when it is on this machine.

Run it again any time to add a repository. `shipsecure-mcp setup --check` reports what is set up and what is missing, and so does the `setup_status` tool.

## Snyk

Dependency scans use the Snyk CLI by default, and it runs only when you are signed in. The wizard opens Snyk's sign-in page, where you can create a free account. The CLI keeps and renews the login, so there is nothing to copy. `snyk.mode` in the config picks the scanner:

| `snyk.mode` | Scans with |
|---|---|
| `required` (default) | Snyk every time; a scan without it fails |
| `auto` | Snyk when you are signed in, otherwise OSV.dev |
| `off` | OSV.dev, with no Snyk account. Only package names and versions are sent. |

npm audit runs beside either one for npm packages. Snyk's free plan allows about 200 tests a month: a daily check spends one per project, and a release about five more.

## Your own rules

The skill in this repository is generic. Your own rules, lessons and writing preferences go in an overlay folder outside it, `~/.config/shipsecure-mcp/skill/references/`:
- `local.md`: your rules, read first, and they take precedence
- `local-lessons.md`: your lessons
- `local-writing.md`: your writing preferences

`npm run install-skill` layers them over the generic skill, so an update never overwrites them.

## Updating

```bash
npm install -g shipsecure-mcp@latest
shipsecure-mcp setup --check
```

The first command installs the newest version; the second confirms everything is still set up. Restart your assistant afterwards so it loads the new version.
