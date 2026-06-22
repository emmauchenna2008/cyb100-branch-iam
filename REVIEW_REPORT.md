# Quarterly IAM Access Review Report

**Repository:** cyb100-branch-iam  
**Review Period:** Q2 2026  
**Reviewer:** Emmanuel Uchenna  
**Date:** 2026-06-22  

---

## 1. Executive Summary

A full IAM access review was conducted on the cyb100-branch-iam repository covering eight control areas including commit signing, branch protection, secret scanning, and environment gating. Two gaps were identified and remediated during the review period, leaving the repository in a strong IAM posture with no critical outstanding risks.

---

## 2. Audit Scope and Methodology

The audit covered the following eight control areas:
- Commit signature verification on main branch
- Remote URL configuration confirming SSH-only access
- Branch protection rules on main
- Credential scanning of commit history
- GPG signature verification on release tags
- Workflow permissions block enforcement
- Pipeline actor audit via Actions run history
- Environment protection configuration for staging and production

Each control was verified using git commands, GitHub Settings pages, and GitHub Actions run logs. Evidence was captured as screenshots for each area.

---

## 3. Findings

| Severity | Finding | Evidence |
|----------|---------|----------|
| Medium | Commits on main lacked GPG signatures prior to remediation | git log --show-signature showing unsigned commits |
| Medium | Dependency audit job used continue-on-error allowing vulnerable dependencies to pass | Workflow file showing continue-on-error: true |
| Info | Safety scan reported 46 vulnerabilities in runner system packages | Actions run log for Dependency Audit job |
| Info | Gitleaks correctly detected and blocked fake Stripe secret in test PR | Actions run showing Gitleaks failure on test/secret-leak branch |

---

## 4. Remediation Actions Taken

**Gap 1 — Unsigned commits on main:**  
GPG signing was enforced globally using git config --global commit.gpgsign true with key 467348E4F35C2036. Verified by running git log --show-signature showing Good signature on all new commits.

**Gap 2 — Soft dependency gate:**  
The continue-on-error: true flag was removed from the Dependency Audit job in iam-security.yml. The safety scan command now hard-fails the workflow when vulnerabilities are detected, blocking merges until dependencies are patched.

---

## 5. Outstanding Risks and Recommendations

- The Safety scan currently flags 46 vulnerabilities in GitHub Actions runner system packages outside our control. Recommendation: pin the runner to a hardened image or use a requirements.txt to scope scanning to project dependencies only.
- CODEOWNERS file is not yet configured, meaning workflow file changes do not require security team review. Recommendation: add a CODEOWNERS file requiring review of .github/workflows/* changes.
- Dependabot is not enabled. Recommendation: enable Dependabot alerts and auto-PRs to keep dependencies patched automatically.

---

## 6. Attestation

I, Emmanuel Uchenna, confirm that this IAM access review was conducted on 2026-06-22 and that all findings and remediation actions documented above are accurate to the best of my knowledge.

**Reviewer:** Emmanuel Uchenna  
**Date:** 2026-06-22  
**Repository state commit hash:** to be updated after final commit  
