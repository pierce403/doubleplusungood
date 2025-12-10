# Notepad++ auto-update timeline

Focus: how the built-in updater (WinGUp/gup.exe) and its security posture evolved.

## Key milestones (chronological)
- 2007-11-06 — 0898192c6: groundwork to integrate the auto-updater module.
- 2007-11-10 — a89f6010d: preference toggle added to enable/disable the updater.
- 2007-11-21 — c5b219340: v4.6 release notes mention restricting auto-updater on legacy Win9x/Me/NT.
- 2009-08-12 — ff1d754ad: added “snooze” support (skip auto-updater for 15 days).
- 2011-10-22 — 9a2f0eeb7 (`PowerEditor/bin/updater/gup.xml`): updater URL switched to notepad-plus-plus.org (HTTP).
- 2015-05-16 — 5c272a881 (`PowerEditor/bin/updater/gup.xml`, Notepad++ 6.7.8): updater InfoUrl moved to HTTPS (`https://notepad-plus-plus.org/update/getDownloadUrl.php`); gup.xml notes both HTTP/HTTPS support.
- 2012-11-05 — cb383ab1b: removed Win95/98/ME gating for updater.
- 2013-02-17 — 1e10e5e4d: added “Set Updater proxy…” command.
- 2015-05-22 — d3c7ade18 (`PowerEditor/src/NppCommands.cpp`): blocked updater on Windows XP due to obsolete security APIs once downloads moved to SSL; prompts users to download manually.
- 2016-05-07 — 6c4f9a64d: new API to disable auto-updater.
- 2016-05-21 — e3c18f61e: “Never” button added to the auto-updater prompt dialog.
- 2016-09-20 — 53ca639b1: adapted to WinGUp 4.1 to distinguish 32-bit vs 64-bit updates.
- 2017-11-25 — dd6101ea1: upgraded WinGUp to fix connection problems (likely TLS-related).
- 2018-10-12 — 45812764c: WinGUp updated to v5.0.3.
- 2018-11-24 — 61402a354: elevation support for WinGUp when plugins install under `%PROGRAMDATA%`.
- 2018-12-22 — 6eabece7a: Plugin Admin excluded on XP because WinGUp requires modern SSL.
- 2019-01-18 — b9ce84888: EU-FOSSA fix to prevent EXE hijacking of gup.exe launched by Notepad++.
- 2019-02-13 — 501980782 (`PowerEditor/src/NppCommands.cpp`): verify gup.exe’s certificate before launching (release builds).
- 2022-05-29 — b5479bb9b (`PowerEditor/src/WinControls/PluginsAdmin/pluginsAdmin.cpp`): fixed certificate-check issue that hid Plugin Admin.
- 2018-05-18 — 07b765316: removed bundled updater binaries (including gup.xml) from the repo; updater config moved into code/build artifacts later.
- 2025-10-30 — e6739c0ab (`PowerEditor/src/resource.h`): Notepad++ now passes the updater info URL directly (HTTPS).
- 2025-11-16 — 3daa59326 (`PowerEditor/src/resource.h`): force the download domain prefix for updates to prevent hijacked URLs.
- 2025-12-01 — bcf2aa68e: security enhancement to verify certificate & signature on the update installer.

## Current behaviors to note
- Updater binaries live under `updater/gup.exe`; before launch, release builds verify the signature/known hashes (`PowerEditor/src/NppCommands.cpp`, `PowerEditor/src/MISC/Common/verifySignedfile.*`).
- XP is blocked for the updater because Notepad++ update downloads require SSL; users are redirected to manual downloads.
- The app now injects the `INFO_URL` (HTTPS) and forces a trusted download domain, mitigating URL tampering.
- Plugins Admin also uses WinGUp for plugin updates and inherits the same certificate checks and XP exclusions.
- Code-signing and release-key materials live in-repo: `README.md` documents the Notepad++ code-signing certificate (name, fingerprint, expiry) and `nppGpgPub.asc` contains the GPG release key; updater modules are checked against embedded hashes/certificates before execution.
