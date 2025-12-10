# Incident Response Playbook – Notepad++ updater compromise (Dec 2025)

Scope: Suspected compromise of the Notepad++ updater (WinGUp/gup.exe) delivering malware via MITM or tampered binaries. Applies to versions prior to 8.8.9 (signature enforcement) and especially pre‑8.8.8 (no domain pinning).

## 1) Identification
- Confirm installed Notepad++ version. Flag anything < 8.8.8 (no domain pinning) or < 8.8.9 (no installer signature enforcement).
- Validate updater binary:
  - gup.exe must be signed by GlobalSign (current cert) and match shipped hash; legacy hash to flag: `5eb90daf1cad88ad33bceed04b0d01cb5aaf3883f991516ffc9e4b99a1c413de` (old self-signed).
  - Reject unsigned or self-signed gup.exe.
- Network indicators: gup.exe should only reach `notepad-plus-plus.org`, `github.com`, `release-assets.githubusercontent.com`. Any other domain is suspicious.
- Filesystem indicators: malicious droppers often land in `%TEMP%` as `update.exe` or `AutoUpdater.exe` (names Notepad++ never uses).
- Process tree: gup.exe should only spawn `explorer.exe` and the signed Notepad++ installer; other child processes are suspect.

## 2) Containment
- Disconnect host from network; block egress to domains contacted by gup.exe outside the allowed set.
- Preserve volatile data: collect running processes, network connections, gup.exe binary, and any `%TEMP%/update*.exe` artifacts.
- Quarantine gup.exe if unsigned/self-signed or hash mismatched; do not delete before imaging.

## 3) Eradication
- Remove malicious droppers from `%TEMP%` (e.g., `update.exe`, `AutoUpdater.exe`).
- Replace gup.exe with the verified version from Notepad++ 8.8.9 or later.
- Remove any lingering Notepad++ self-signed root cert files if present in installation folders; ensure system trust store not modified (installer scripts do not add roots by default).

## 4) Recovery
- Install/upgrade to Notepad++ 8.8.9+ from the official download (HTTPS, pinned domain).
- Verify updater signature and run an on-demand AV scan.
- Reconnect network; monitor gup.exe network traffic to ensure it only hits allowed domains.

## 5) Lessons / Hardening
- Keep Notepad++ updated; avoid skipping minor releases that contain updater hardening (8.8.8 domain pinning, 8.8.9 signature verification).
- Prefer offline/manual updates on high-value systems; disable auto-update if egress controls are strict.
- Add egress rules to restrict gup.exe to `notepad-plus-plus.org`, `github.com`, and `release-assets.githubusercontent.com`.
- Retire legacy installer packages that carried self-signed updater binaries.

## References
- News coverage and IOCs: `notes/news-coverage-malware-updater.md`.
- Technical timelines: `notes/auto-update-timeline.md`, `notes/cert-and-ssl-timeline.md`.
