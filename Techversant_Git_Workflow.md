# 🧭 Techversant Git Workflow
**Version:** 1.2  
**Maintained by:** Center of Excellence (CoE)

A unified Git workflow for all engineering teams — Backend, Frontend, Mobile, and AI/ML.

## ⚙️ Core Principles
- main is always production-ready and deployable.
- Every change starts in a short-lived feature branch.
- PR review + CI validation are mandatory before merging.
- Build once → promote through environments (Dev → Staging → Production).
- Every production deployment must be tagged.
- Follow Conventional Commits for clarity and automation.
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
| (Optional) Release Branch | release/<version> | release/v2.5.0 |

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

## 🧑‍💻 Developer Workflow (Multi-Stage Promotion Model)
```
git checkout main && git pull
git checkout -b feature/add-ICF-102-login
git add . && git commit -m "feat(auth): add OTP-based login"
git fetch origin && git rebase origin/main
git push origin feature/add-ICF-102-login
```
→ Open PR → CI runs → Review → Merge to **dev**

### 🔁 Promotion Flow
1. **Merge PR to `dev`** → triggers deployment to **Development environment**.
2. **Validate in Dev** → run integration, smoke, and regression tests.
3. **Promote to Staging** → run QA/UAT validation.
4. **After QA sign-off**, merge verified commit to **main**.
5. **Tag the release** (e.g., `v2.4.0`) → triggers Production deployment.

This ensures that main always contains only tested, deployable code.

## 🚀 Environment Flow
| Environment | Trigger | Validation |
|--------------|----------|-------------|
| Dev | Merge to dev | Unit + Integration + Smoke tests |
| Staging | Manual promotion from dev | QA & UAT sign-off |
| Prod | Merge verified commit to main + Tag (vX.Y.Z) | Approval + Smoke tests |

### (Optional) Release Branch Usage
- For larger coordinated releases, create a temporary branch: `release/vX.Y.Z`.
- Stabilize QA/UAT changes on the release branch.
- Tag production release from it, then merge back to main.
- Delete the branch post-deployment.

## 🔁 Rollback & Database Script Management
- Tag every release; tags are immutable.
- Rollback using tags (not manual DB edits).
- Automate rollback with CI/CD pipelines (Helm, Jenkins, GitHub Actions).
- Maintain database migration scripts under `db/migrations/`.
- Follow the **Expand → Migrate → Contract** pattern for schema changes.
  - **Expand:** Add new columns/tables safely.
  - **Migrate:** Backfill or transform data incrementally.
  - **Contract:** Remove old columns/tables after at least one stable release cycle.
- Keep migration scripts **idempotent**, **transactional**, and **version-controlled**.
- Separate schema (`db/migrations`) and data (`db/seeds`) scripts.
- Validate scripts pre- and post-deployment for consistency.
- Store executed migration logs for audit and rollback.
- Backup production data before applying major migrations.
- Use rollback scripts (reverse migrations) for safe reverts if necessary.

## 🧩 Governance
| Area | Responsibility | Owner |
|-------|----------------|-------|
| Branch Protection | Manage merge rules, CI checks | DevOps Lead |
| Tag & Release | Prod approvals | PM / Tech Lead |
| CI/CD Config | Pipelines | DevOps |
| Security | Secrets, audit | Architect |
| Policy | Audits | CoE |

## 🧼 Hygiene Checklist
✅ Delete merged branches weekly  
✅ Clean unused tags quarterly  
✅ Rotate tokens every 90 days  
✅ Document rollbacks  
✅ Maintain and test DB migration scripts regularly  
✅ Keep .env & .nocommit updated  
✅ Maintain consistent .editorconfig  

> “Branching isn’t about control — it’s about clarity, rhythm, and flow.”
