# CLAUDE.md

Guidance for AI coding agents working in this repository.

## Project

Agent Inc is a Godot 4.7 (.NET / C#) desktop game. It visualizes Claude Code and Codex sessions as workers in a 3D office by reading their lifecycle hooks. Start with `README.md` and `docs/architecture.md`.

## Rules

- Never commit secrets, personal info, absolute user paths, emails, or real hook payloads or session files. Use synthetic test data.
- **Observe-only must stay zero-cost.** The hook forwarder must print nothing to stdout, always exit `0`, finish quickly, and keep only the fields listed in `docs/privacy.md`.
- **The event format is a contract.** If you change it, update `docs/architecture.md`. Readers must ignore unknown fields, and breaking changes bump `v`.
- **Systems talk only through the event bus.** Never reference one system from another. See "Code layout" in `docs/architecture.md`.
- Use C# for all code. Edit `project.godot` and `.tscn` files through Godot when possible. If you edit them by hand, keep the diffs minimal.
- When behavior changes, update the matching doc in `docs/` in the same change.
- Machine-specific notes go in `CLAUDE.local.md`, which is gitignored and never committed.
