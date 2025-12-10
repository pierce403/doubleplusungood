# Security Timeline Notes

This repository exists to drive a security-focused timeline narrative for the vendored `notepad-plus-plus/` codebase. Upstream sources live in `notepad-plus-plus/` but are ignored here; only our notes and helper docs are tracked.

## High-level findings (so far)
- Auto-update was introduced in 2007 and hardened incrementally; HTTPS pinning landed in 8.8.8 and signature enforcement in 8.8.9.
- The Dec 2025 incident: malicious payloads were served via MITM on the updater; 8.8.9 is the maintainer’s fix with domain pinning + signature checks.
- No evidence that Notepad++ installers ever install a root CA—scripts only sign binaries; verification relies on OS trust plus in-app hash/cert checks.
- We added response materials: full git timeline, auto-update/cert timelines, news roundup with IOCs, and an incident-response playbook.
- Historical precursor: 2008 Evilgrade/CVE-2008-3436 showed updater spoofing (pre-4.8.1) via DNS/HTTP MITM; echoes the 2025 issue and informs the hardening trail.

## Layout
- `AGENTS.md`: instructions and mission context for coding agents.
- `notes/`: add Markdown drafts (e.g., `security-timeline.md`) with sourced entries.
- Key notes: `notes/auto-update-timeline.md`, `notes/cert-and-ssl-timeline.md`, `notes/news-coverage-malware-updater.md`, `notes/incident-response-playbook-notepadpp-updater.md`.

## Working guidelines
- Keep references to specific files/commits/docs when adding timeline entries.
- Avoid modifying upstream code unless explicitly tasked; treat `notepad-plus-plus/` as read-only.
- Keep changes small and documented; follow `AGENTS.md` for style and process.

## Methods used in this repo
- Code/history: inspected the vendored `notepad-plus-plus/` git log, specific files, and blame with `git log`, `git show`, `git blame`.
- Search: used `rg` to locate updater, SSL, and signing code paths and installer scripts.
- External context: fetched news articles and archives via `curl` from the Codex CLI to summarize press coverage and IOCs.
- Notes and playbooks: captured findings in `notes/` Markdown files; no upstream code modified, only documentation in this repo.
