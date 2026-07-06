# emprise

> *"A bold undertaking"* — AI-powered **CLI** + **Desktop** assistant with 96+ tools, document intelligence, multi-LLM support, PHI/HIPAA compliance, and a 6-layer prompt engineering system. Same Go runtime, same config, same SQLite history — just two front-ends.

[![Download](https://img.shields.io/github/v/release/pounceapps/emprise-app?label=download)](https://github.com/pounceapps/emprise-app/releases/latest)
[![Website](https://img.shields.io/badge/website-emprise.dev-58d1c9)](https://emprise.dev)
[![Docs](https://img.shields.io/badge/docs-wiki-blue)](https://github.com/pounceapps/emprise-app/wiki)

> ### ⚠️ Experimental — work in progress, no warranty
>
> emprise is an experimental personal project. Expect rough edges, occasional regressions, and breaking changes between releases. **Use at your own risk.** No warranty, express or implied.
>
> **The source repository is private.** This repo (`emprise-app`) only carries the install script, brand assets, and the published builds. If a binary you need isn't in the latest release, file an [issue](https://github.com/pounceapps/emprise-app/issues) — it's a CI matter, not a "build it yourself" matter.

## Install

### CLI (macOS / Linux)

```bash
curl -fsSL https://raw.githubusercontent.com/pounceapps/emprise-app/main/install.sh | sh
emprise --setup
```

The script picks the right binary, drops it on your PATH, and runs the first-run wizard.

### CLI (Windows)

Download `emprise-windows-amd64.exe` from [Releases](https://github.com/pounceapps/emprise-app/releases/latest) and put it on `%PATH%`.

### Desktop

| Platform | Asset |
|---|---|
| macOS Apple Silicon | [`emprise-desktop-macos.dmg`](https://github.com/pounceapps/emprise-app/releases/latest) (signed + notarized) |
| Windows amd64 | [`emprise-desktop-windows-installer.exe`](https://github.com/pounceapps/emprise-app/releases/latest) (NSIS, bootstraps WebView2) |
| Linux amd64 | [`emprise-desktop-linux-amd64.tar.gz`](https://github.com/pounceapps/emprise-app/releases/latest) (needs `libwebkit2gtk-4.0-37` and `libgtk-3-0`) |

See the [Installation guide](https://github.com/pounceapps/emprise-app/wiki/Installation) for step-by-step instructions per platform.

## What's in the box

- **96+ built-in tools** across 22 categories — files, git, shell, databases (SQLite/Postgres/MySQL/SQL Server with Azure AD), web, Docker, SSH, cloud, document parsing (PDF/DOCX/XLSX/PPTX/Jupyter), image, archive, regex, networking, and more.
- **6 LLM providers** — Ollama (local), OpenAI, Anthropic, GitHub Models, Azure OpenAI, custom OAuth endpoints (device-code or auth-code flows). Per-profile auth, instant switching.
- **6-layer prompt system** — base identity → capability tier → role default → user rules → model overrides → emphasis rules → memories → attached contexts.
- **Document intelligence** — index folders into SQLite FTS5; paste up to 100 URLs into a "web context" with extracted topic/summary/embeddings; generate PDF/DOCX/HTML/MD output (Mermaid diagrams rasterized into PDFs).
- **PHI compliance** — local-only models, web tools blocked, every tool call audited, profile validation against an allowlist.
- **Persistent history** — SQLite-backed conversations with projects, search, bulk move, named sessions, per-project working folders.
- **Cross-platform** — same Go binary builds for macOS arm64, Linux amd64, Windows amd64. Desktop app packaged as signed/notarized DMG, NSIS installer, or tarball.

## Quick start (CLI)

```bash
emprise                          # Interactive chat
emprise "what is my disk usage?" # Single-shot
emprise -c                       # Continue last conversation
cat file.log | emprise "explain" # Pipe mode
```

## Slash commands

Type `/help` inside emprise for the full list. Highlights:

| Command | Behavior |
|---------|----------|
| `/profile` | Switch LLM profile |
| `/model` | Switch model on the active profile |
| `/tools` | Toggle individual tools (state persists across restarts) |
| `/dev` | Developer-environment audit |
| `/prompts` | Prompt engineering config |
| `/stats` | Usage stats — session and cumulative |
| `/phi` | Toggle PHI / compliance mode |
| `/audit` | View this session's audit log |
| `/clear` | Reset both UI and server context, save prior to history first |
| `/setup` | Add an LLM provider (wizard) |

Full list: [Commands wiki](https://github.com/pounceapps/emprise-app/wiki/Commands).

## Configuration

Both CLI and Desktop share `~/.emprise/`:

```
~/.emprise/
├── config.yaml      # profiles, prompts, tool settings
├── secrets.json     # OAuth tokens (chmod 600)
├── history.db       # SQLite conversations + projects + memories + web contexts
├── audit.jsonl      # tool-call audit log
├── backups/         # auto-snapshots taken on schema version changes
└── indexes/         # FTS5 document indexes
```

Example minimal config:

```yaml
default_profile: local
profiles:
  local:
    provider: ollama
    base_url: http://localhost:11434
    model: llama3.2:3b
    auth: { type: none }
prompts:
  user_name: Your Name
  role: developer
```

See the [Configuration guide](https://github.com/pounceapps/emprise-app/wiki/Configuration) for the full schema (OAuth, PHI mode, SQL databases, MCP servers, web contexts, etc.).

## Update

```bash
emprise update
```

Atomic in-place upgrade — emprise queries the GitHub releases API, downloads the new binary if newer, swaps it in, and restarts.

## Documentation

- **[Website](https://emprise.dev)** — feature overview, screenshots, news
- **[Wiki](https://github.com/pounceapps/emprise-app/wiki)** — installation, configuration, every command, every tool, FAQ
- **[Discussions](https://github.com/pounceapps/emprise-app/discussions)** — community, Q&A
- **[Issues](https://github.com/pounceapps/emprise-app/issues)** — bug reports, feature requests

## License

Proprietary. Built by Steve Sparks. See [LICENSE](LICENSE).

## Disclaimer

Tools that write files, run shell commands, query databases, or call cloud APIs do exactly that. The model can be wrong; tool guards are best-effort, not airtight. Always review `/audit` after a session that touched anything you care about. **You are the operator** — emprise is a power tool, not a chaperone.
