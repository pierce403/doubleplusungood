# AGENTS.md

## Self-improvement directive
- Follow recurse.bot guidance: update this file with new, actionable learnings for coding agents—wins to repeat, pitfalls to avoid, collaborator preferences, and reliable commands.

## Mission
- Analyze the vendored `notepad-plus-plus/` codebase to build a security-focused timeline narrative (key security-relevant changes, practices, or incidents).
- Keep working drafts in `notes/` (create Markdown files as needed, e.g., `notes/security-timeline.md`) and cite sources (file paths/commits/docs) for each entry.

## Repo layout
- This top-level repo ignores the `notepad-plus-plus/` directory (upstream code). Work inside it for analysis, but do not commit its contents here.
- Root files (e.g., this AGENTS.md, `.gitignore`, and notes you add) are tracked in this repo.

## Build/test references for Notepad++
- Visual Studio 2022 (17.5+, v143): open `notepad-plus-plus/PowerEditor/visual.net/notepadPlus.sln`; build Debug/Release for Win32/x64/ARM64.
- MinGW-w64: from `notepad-plus-plus/PowerEditor/gcc`, run `mingw32-make` (add `DEBUG=1` for debug). Outputs to `bin.i686` or `bin.x86_64`.

## Conventions and tips
- Respect upstream coding style from `notepad-plus-plus/CONTRIBUTING.md` (tabs, brace style, avoid sweeping reformatting).
- Prefer read-only analysis and note-taking; avoid modifying upstream sources unless explicitly tasked.
- Keep AGENTS.md concise; summarize new insights rather than duplicating large docs.
