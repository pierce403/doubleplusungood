# Evilgrade 2008 / CVE-2008-3436 (Notepad++ updater)

What happened
- August 2008: Attackers could spoof update responses to GUP (Notepad++ auto-updater) via DNS poisoning/HTTP MITM and deliver arbitrary executables. Publicly demonstrated with the Evilgrade toolkit. citeturn0search1turn0search6
- Root cause: GUP ≤4.8.0 did not verify authenticity of downloaded update metadata/binaries.
- Impact: Remote code execution on clients checking for updates.
- Fix: Notepad++ 4.8.1 patched GUP to validate downloads; advisory referenced as CVE-2008-3436. citeturn0search6

Relevance to 2025 incident
- Pattern rhymes with 2025 MITM malware case: weak validation of updater payloads; shows a long-standing risk area in the project’s updater pipeline.
- Difference: 2008 issue predated HTTPS adoption and later signature/domain pinning; 2025 case involved hijacked download URLs and missing signature enforcement, now addressed in 8.8.8/8.8.9.

Artifacts to keep in mind
- Affected versions: Notepad++ ≤4.8.0 (GUP before authenticity checks).
- Mitigation lineage: 4.8.1 added validation; later hardening includes HTTPS (6.7.8), domain pinning (8.8.8), and signature enforcement (8.8.9).
