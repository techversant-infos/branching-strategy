# 🧭 Techversant Git Workflow
**Version:** 1.0  
**Maintained by:** Center of Excellence (CoE)

A unified Git workflow for all engineering teams — Backend, Frontend, Mobile, and AI/ML.

## ⚙️ Core Principles
- main is always deployable.
- Every change starts in a short-lived feature branch.
- PR review + CI validation are mandatory before merging.
- Build once → promote through environments (no env-specific branches).
- Every production deployment must be tagged.
- Follow Conventional Commits for clarity.
- Never commit sensitive data — use .gitignore or .nocommit.
- Keep .editorconfig consistent across repos.

## 🌿 Branching Model
| Purpose | Naming Convention | Example |
|----------|-------------------|----------|
| New feature | feature/add-<JIRA-ID>-<desc> | feature/add-ICF-102-login |
| Bug fix | feature/fix-<JIRA-ID>-<desc> | feature/fix-AGL-881-rounding |
| Optimization | feature/optimize-<JIRA-ID>-<desc> | feature/optimize-AI-22-cache |
| Documentation | feature/doc-<JIRA-ID>-<desc> | feature/doc-PLAT-17-api-guide |
| Refactor | feature/refactor-<JIRA-ID>-<desc> | feature/refactor-SPARK-221-service |
| Hotfix | feature/hotfix-<JIRA-ID>-<desc> | feature/hotfix-OPS-999-null-pointer |

**Tips:** Always branch off main, keep branches small, and include JIRA ID.

## 📝 Commit Standard
Use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/). Example:

```
feat(auth): add OTP-based login

- Added mobile-based verification
- Updated auth controller for token generation

Fixes #ICF-102
```

Allowed types: feat, fix, docs, style, refactor, perf, test, chore, ci

## 🧑‍💻 Developer Workflow
```
git checkout main && git pull
git checkout -b feature/add-ICF-102-login
git add . && git commit -m "feat(auth): add OTP-based login"
git fetch origin && git rebase origin/main
git push origin feature/add-ICF-102-login
```
→ Open PR → CI runs → Review → Merge to main

## 🚀 Deployment Flow
| Environment | Trigger | Validation |
|--------------|----------|-------------|
| Dev | Merge to main | Unit + Integration tests |
| Staging | Manual promotion | QA & UAT |
| Prod | Tag release (vX.Y.Z) | Approval + Smoke tests |

## 🔁 Rollback & Migration
- Tag every release; tags are immutable.
- Rollback using tags (not manual DB edits).
- Automate rollback (Helm/Jenkins).
- Use Expand → Migrate → Contract for DB changes.
- Keep scripts idempotent & version-controlled.

## 🧩 Governance
| Area | Responsibility | Owner |
|-------|----------------|-------|
| Branch Protection | Merge rules, CI checks | DevOps Lead |
| Tag & Release | Prod approvals | PM / Tech Lead |
| CI/CD Config | Pipelines | DevOps |
| Security | Secrets, audit | Architect |
| Policy | Audits | CoE |

## 🧼 Hygiene Checklist
✅ Delete merged branches weekly  
✅ Clean unused tags quarterly  
✅ Rotate tokens every 90 days  
✅ Document rollbacks  
✅ Keep .env & .nocommit updated  
✅ Maintain consistent .editorconfig  

> “Branching isn’t about control — it’s about clarity, rhythm, and flow.”
