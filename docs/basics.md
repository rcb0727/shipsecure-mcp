# New here?

[Overview](../README.md) · **New here?** · [Guard](guard.md) · [Ship](ship.md) · [Setup](setup.md) · [Tools](tools.md) · [Security](security.md) · [Privacy](privacy.md) · [Configuration](configuration.md) · [Changelog](../CHANGELOG.md)

**In short:** you talk to your AI assistant, and ShipSecure gives it safe, careful tools for checking and releasing your code. This page explains the words the rest of the docs use.

## How it works

1. **You set it up once.** `npx shipsecure-mcp setup` asks a few questions and connects ShipSecure to your assistant.
2. **You ask in plain words.** For example: "scan this repo for secrets" or "ship it".
3. **Your assistant uses ShipSecure's tools.** Each tool first shows what it would do, and changes nothing until it is told to go ahead.
4. **You get a plain answer.** What it found, what it did, and anything you need to do, such as replacing a key.

## The words

| Word | What it means |
|---|---|
| **AI assistant** | A program you chat with that can also work on your code, like Claude Code or ChatGPT |
| **MCP** | Model Context Protocol: the standard way to give an assistant extra tools. ShipSecure is an MCP server, a set of such tools |
| **Agent skill** | Written instructions that tell the assistant how and when to use the tools |
| **Repository (repo)** | A project folder whose history git tracks. GitHub stores repositories online |
| **Commit** | A saved snapshot of your project. The **history** is every commit ever made, and git keeps all of them |
| **Branch** | A separate line of work in a repository, such as `main` or `feature/login` |
| **Push** | Uploading your commits to GitHub |
| **Credential, secret** | Anything that proves who you are to a service: a password, an API key, a token. Anyone who has it can act as you |
| **Rotate** | Replacing a credential with a new one at the service that issued it, so the old one stops working. It is the only real fix for a leaked key |
| **Dependency, library** | Someone else's code your project uses |
| **Lockfile** | A file that lists the exact version of every dependency, such as `package-lock.json`, `poetry.lock` or `Cargo.lock` |
| **Vulnerability, CVE** | A known security hole in a version of a library. A CVE number is its public id |
| **Snyk, OSV.dev** | Databases of known vulnerabilities. ShipSecure checks your lockfiles against them |
| **npm** | The registry where JavaScript packages are published and installed from |
| **Publish, release** | Putting a new version of your package on npm for others to install |
| **Gate** | Your project's checks (tests, build) that must pass before anything goes further |
| **GitHub Actions** | GitHub's computers that can run those checks for you. ShipSecure works with or without them |
| **Dry run** | A preview: the tool shows exactly what it would do and changes nothing |
| **2FA** | Two-factor authentication: a second step, such as a code on your phone, that proves it is you |
| **SBOM** | Software bill of materials: the list of every library inside a release |

## Is it safe to try?

Yes. Guard only reads your code and never publishes anything. Every tool that would change something shows a dry run first and waits to be told to go ahead. ShipSecure never asks you to paste a password or key into the chat. If you paste one anyway, it is not used, and you are told to replace it.
