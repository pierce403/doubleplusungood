# Notepad++ signing & SSL timeline

Goal: track how update URLs moved to HTTPS, how signing/verification changed, and whether installers ever injected trust anchors.

## Milestones (chronological)
- 2011-10-22 — 9a2f0eeb7 (`PowerEditor/bin/updater/gup.xml`): updater InfoUrl switched from SourceForge to `http://notepad-plus-plus.org/...` (HTTP).
- 2015-05-16 — 5c272a881 (`PowerEditor/bin/updater/gup.xml`, Notepad++ 6.7.8): InfoUrl moved to HTTPS (`https://notepad-plus-plus.org/update/getDownloadUrl.php`); gup.xml notes HTTP/HTTPS support.
- 2016-02-20 — 877e10f70 (`PowerEditor/installer/packageAll.bat`): added signtool signing for binaries (notepad++.exe, SciLexer, shell DLLs, updater binaries gup/gpup/libcurl, bundled plugins). Signing uses `%NPP_CERT%`—installer scripts expect a code-signing cert but do not install any root CA.
- 2018-05-18 — 07b765316: removed bundled updater binaries (including gup.xml) from the repo; update config moves into build artifacts/code.
- 2019-01-18 — b9ce84888: EU-FOSSA fix prevents gup.exe hijacking when launched by Notepad++.
- 2019-02-13 — 501980782 (`PowerEditor/src/NppCommands.cpp`): release builds verify gup.exe certificate before launching updater.
- 2025-07-03 — 03063ebf4 (`PowerEditor/installer/packageAll.bat`): switched signing macro to SHA-512/timestamp.acs.microsoft.com; comment notes using a self-signed certificate. Still uses signtool with `%NPP_CERT%`; no trust-store injection.
- 2025-10-30 — e6739c0ab (`PowerEditor/src/resource.h`): Notepad++ passes the updater info URL directly (HTTPS).
- 2025-11-16 — 3daa59326 (`PowerEditor/src/resource.h`): forces download domain prefix to GitHub to prevent hijacked update URLs.
- 2025-12-01 — bcf2aa68e: security enhancement to verify certificate & signature on the update installer.
- 2025-12-09 — b5ce090bf (Notepad++ 8.8.9 release): shipped after reports that the updater delivered malware; release framed as a corrective update.
- 2025-12-09 — README.md: documents current Notepad++ code-signing certificate (name/fingerprint/expiry). `nppGpgPub.asc` in repo holds the GPG release key.

## Findings about trust anchors
- No installer script calls certutil/certmgr/addstore or similar; there is no evidence of installing a root CA into the system trust store. Signing is applied to shipped binaries; verification is done by Windows trust and in-app checks (gup hash/cert validation).

## Related files
- Signing scripts: `PowerEditor/installer/packageAll.bat`, `PowerEditor/installer/sign-installers.bat`.
- Update URL constants: `PowerEditor/src/resource.h` (current), historic `PowerEditor/bin/updater/gup.xml` (removed in 2018).
- Updater verification: `PowerEditor/src/NppCommands.cpp`, `PowerEditor/src/MISC/Common/verifySignedfile.*`.
- Release keys: `README.md` (code-signing cert details), `nppGpgPub.asc` (GPG public key).
