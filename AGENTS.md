# Repository Guidelines

## Project Structure & Module Organization

This repository is in its initial planning stage. Keep top-level documentation focused and move detailed designs into `specs/`.

- `README.md`: public project overview and setup entry point.
- `briefing.md`: project goal and background (WireGuard access to a locally hosted Arma Reforger server without router port forwarding).
- `specs/`: architecture notes, network diagrams, threat models, and implementation specifications.

When implementation begins, prefer `scripts/` for setup, `config/` for safe templates, and `tests/` for automated checks. Never commit WireGuard private keys or machine-specific configuration.

## Build, Test, and Development Commands

No build system or automated test suite is configured yet. Before submitting documentation changes, run lightweight checks such as:

```bash
git diff --check          # detect whitespace errors
git status --short        # review the files included in the change
markdownlint "**/*.md"    # lint Markdown when markdownlint is installed
```

Document future commands in `README.md`; they should run from the repository root. Prefer idempotent scripts that fail clearly when dependencies are missing.

## Coding Style & Naming Conventions

Use two-space indentation for YAML and write shell scripts for Bash unless portability requires POSIX `sh`. Name scripts with lowercase kebab-case (for example, `scripts/setup-wireguard.sh`) and configuration examples with a `.example` suffix. Quote shell variables and enable strict error handling where appropriate. Use concise Markdown headings and fenced code blocks with language tags.

## Testing Guidelines

Add tests alongside every implementation change. Shell scripts should be checked with `shellcheck`; configuration validation should use native dry-run or syntax-check commands when available. Put integration tests in `tests/` and name them after the behavior under test, such as `tests/wireguard-connectivity.sh`. Tests must not alter the host firewall or network permanently; include cleanup for temporary interfaces and rules.

## Commit & Pull Request Guidelines

History currently contains only `Initial commit`, so no established convention exists. Use short, imperative subjects such as `Add WireGuard server setup script`, and keep each commit narrowly scoped. Pull requests should explain the motivation, summarize configuration or networking changes, list verification performed, and call out security implications. Link related issues and include sanitized command output or diagrams when they clarify connectivity changes.

### Commit Agent

Whenever the user requests one or more commits, delegate the entire commit operation to the project-level `committer` agent configured in `.codex/config.toml`. Do not create commits directly. The agent must inspect and segregate the pending changes, stage explicit paths, validate each staged diff, and create Conventional Commits according to `.codex/agents/committer.toml`.

## Security & Configuration Tips

Treat keys, public IP addresses, endpoints, and firewall details as sensitive. Commit redacted examples only, keep secrets outside the repository, and document the minimum required ports and privileges. Review scripts carefully before running them with `sudo`.
