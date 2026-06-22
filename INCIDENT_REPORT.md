# Incident Response Report

**Incident Reference:** SI-2024-01
**Date Detected:** 2026-06-22
**Severity:** High

## Summary

On 2026-06-22, commit 013854e introduced a plaintext credential string (FAKE_API_KEY=sk_live_testonly123) into config.env on the incident/api-key-leak branch. The offending commit was detected during a routine audit using git log -S to search commit diffs for credential patterns. Revert commit 06b46ff was immediately applied by Emmanuel Uchenna, removing the exposed key from the working codebase while preserving the full audit trail in git history. The affected credential was rotated immediately following detection and the old key was invalidated. To prevent recurrence, pre-commit hooks using tools such as git-secrets or trufflehog should be enforced across all branches to block credential strings from being committed, alongside a mandatory secrets management policy requiring all API keys to be stored in environment variable managers rather than config files.
