# Changelog

All notable changes to this project are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html). The version is
single-sourced from `src/hackingtool/constants.py` (`VERSION`).

## [Unreleased]

A ground-up modernization of hackingtool from a script collection into an installable,
data-driven, AI-guided product for authorized security testing.

### Added
- **Data-driven tool catalog** — YAML catalog (`src/hackingtool/catalog/`) with a
  registry, a canonical tag taxonomy (`tags.py`), usage cheatsheets, and an overlay
  mechanism that enriches existing tools by title. Adding a tool is one entry.
- **AI guidance layer** (bring-your-own-key → local Ollama → graceful no-op):
  - AI1 — free-text intent → tags → recommended tools.
  - AI2 — tool + goal → a real, documented command.
  - AI3 — findings → engagement summary.
  - AI4 — findings → narrative report draft (`--ai-report`), facts kept deterministic.
  - All outputs are validated against closed sets and hardened against prompt
    injection (OWASP LLM01): untrusted scan data is delimited, control chars stripped,
    and a groundedness check flags hallucinated hosts. Nothing auto-executes.
- **Orchestrator + engagements** — pipelines, findings storage, and deterministic
  report generation scoped to a per-engagement workspace.
- **Natural-language TUI entry** — plain text at the main prompt routes to AI
  recommendations; discovery runs on each tool's real `.TAGS`.
- **Packaging** — PyPA `src/` layout, catalog/pipelines shipped as package data, a
  `hackingtool` console entry point, and PyPI (OIDC trusted publishing) / GHCR /
  `.deb` release workflows.
- **CI gate** — `scripts/check.sh` (ruff + pytest + catalog/schema validation), a
  pre-push hook, and a required PR check.
- Project health docs: `SECURITY.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`,
  this changelog.

### Changed
- Installs are standard and verifiable — pipx/pip/.deb/GHCR/source. **`curl | bash`
  removed everywhere.**
- Tool discovery unified on the catalog taxonomy; the legacy regex tagger was retired.
- Command audit logging via `platformdirs`; real exit codes and honest
  install success/failure reporting.

### Security
- Safe-fetch installer: external downloads are pinned and **SHA-256 required**
  (`curl | bash` removed from feroxbuster, Caido, Sliver installers).
- List-form `subprocess` throughout; no forced `sudo`; tools install under
  `~/.hackingtool/`.

## [2.0.0]

Baseline before the modernization above.

### Added
- 22 modern tools across 6 categories.
- Rich terminal UI with a shared theme; OS-aware menus (Linux-only tools hidden on
  macOS); archived-tools sub-menu.

### Changed
- Python 3.10+ required; all Python 2 code removed.
- Iterative menus (no recursion overflow on deep navigation).

### Fixed
- All `os.chdir()` bugs — tools install to `~/.hackingtool/tools/`.
- No more `sudo git clone`; tools install to the user's home, no root needed.

[Unreleased]: https://github.com/Z4nzu/hackingtool/compare/v2.0.0...HEAD
[2.0.0]: https://github.com/Z4nzu/hackingtool/releases/tag/v2.0.0
