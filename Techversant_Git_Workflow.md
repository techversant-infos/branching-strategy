
# 🧭 Techversant Git Workflow
**Version:** 1.3  
**Maintained by:** Center of Excellence (CoE)  
*(Updated: October 2025)*  

A unified Git workflow for all engineering teams — Backend, Frontend, Mobile, and AI/ML.

---

## ⚙️ Core Principles
- `main` is always **production-ready** and deployable.  
- Every change starts in a **short-lived feature branch**.  
- **PR review + CI validation** are mandatory before merging.  
- **Build once → promote** through environments (Dev → Staging → Production).  
- Every **production deployment must be tagged**.  
- Follow **Conventional Commits** for clarity and automation.  
- Never commit sensitive data — use `.gitignore` or `.nocommit`.  
- Keep `.editorconfig` consistent across repositories.  
- Regularly **sync `main` into long-lived branches** to minimize merge conflicts and ensure freshness.  
- Prefer **feature flags** instead of keeping incomplete work unmerged.  

---

## 🌿 Branching Model

| Purpose | Naming Convention | Example |
|----------|-------------------|----------|
| New feature | feature/add-<JIRA-ID>-<desc> | feature/add-ICF-102-login |
| Bug fix | feature/fix-<JIRA-ID>-<desc> | feature/fix-AGL-881-rounding |
| Optimization | feature/optimize-<JIRA-ID>-<desc> | feature/optimize-AI-22-cache |
| Documentation | feature/doc-<JIRA-ID>-<desc> | feature/doc-PLAT-17-api-guide |
| Refactor | feature/refactor-<JIRA-ID>-<desc> | feature/refactor-SPARK-221-service |
| Hotfix | feature/hotfix-<JIRA-ID>-<desc> | feature/hotfix-OPS-999-null-pointer |

**Tips:**  
✅ Always branch off `main`.  
✅ Keep branches small and focused (< 500 LOC).  
✅ Include JIRA ID in branch names.  
✅ Avoid long-running branches; merge early and often.  
✅ Keeps commits atomic.
✅ Regular pulls before push.

---

## 📝 Commit Standard

Follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).  

**Example:**
```bash
feat(auth): add OTP-based login

- Added mobile-based verification
- Updated auth controller for token generation

Fixes #ICF-102
```

**Allowed types:** `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`, `ci`  
✅ Write meaningful commit messages.  
✅ Avoid commits like “minor changes” or “updated files.”  
✅ Use multi-line commits for clarity.  

---

## 🧑‍💻 Developer Workflow (Multi-Stage Promotion Model)

```bash
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

✅ Ensures `main` always contains tested, deployable code.  

---

## 🚀 Environment Flow

| Environment | Trigger | Validation |
|--------------|----------|-------------|
| **Dev** | Merge to `dev` | Unit + Integration + Smoke tests |
| **Staging** | Manual promotion from `dev` | QA & UAT sign-off |
| **Prod** | Merge verified commit to `main` + Tag (vX.Y.Z) | Approval + Smoke tests |

---

## 🔁 Rollback & Database Script Management

- Tag every release; **tags are immutable**.  
- Rollback using **tags**, not manual DB edits.  
- Automate rollback with CI/CD pipelines (Helm, Jenkins, GitHub Actions).  
- Maintain database migration scripts under `db/migrations/`.  
- Follow **Expand → Migrate → Contract** for schema changes:  
  - **Expand:** Add new columns/tables safely.  
  - **Migrate:** Backfill or transform data incrementally.  
  - **Contract:** Remove old schema after one stable release cycle.  
- Keep migration scripts **idempotent**, **transactional**, and **version-controlled**.  
- Separate schema (`db/migrations`) and data (`db/seeds`) scripts.  
- Validate pre- and post-deployment for consistency.  
- Store executed migration logs for audit and rollback.  
- Always back up production data before large migrations.  
- Use **rollback scripts** for safe reversions.  

---

## 🧭 Rebase Safety Guidelines

- **Never rebase shared or pushed branches.**  
- Use `--force-with-lease` (not `--force`) to prevent overwriting others’ work.  
- Announce when rebasing if multiple developers are collaborating.  
- **Before rebase:**
  ```bash
  git fetch origin
  git rebase origin/main
  ```
- **If something goes wrong:**
  ```bash
  git rebase --abort
  ```
- Enable Git rerere to auto-reuse previous conflict resolutions:
  ```bash
  git config --global rerere.enabled true
  ```
- Use rebasing only to **clean up local history**, not rewrite shared history.  

---

## 🧩 Governance

| Area | Responsibility | Owner |
|-------|----------------|-------|
| Branch Protection | Merge rules, CI checks | DevOps Lead |
| Tag & Release | Prod approvals | PM / Tech Lead |
| CI/CD Config | Pipelines | DevOps |
| Security | Secrets, audit | Architect |
| Policy | Audits | CoE |

---

## 🧼 Hygiene Checklist

✅ Delete merged branches weekly  
✅ Clean unused tags quarterly  
✅ Rotate tokens every 90 days  
✅ Document rollbacks  
✅ Maintain & test DB migration scripts regularly  
✅ Keep `.env` & `.nocommit` updated  
✅ Maintain consistent `.editorconfig`  
✅ Regularly **sync long-lived branches** with `main`  
✅ Validate .gitattributes consistency (line endings, LF/CRLF settings).
✅ Periodically verify tag naming conventions (semantic versioning compliance).

---

> “Branching isn’t about control — it’s about clarity, rhythm, and flow.”  

---

### 💡 Optional Enhancements (for future automation)
- **Feature Flags:** Merge early, toggle off until stable.  
- **Automated Versioning:** Use Semantic Release or standard-version for changelog & tagging.  
- **CI Guardrails:** Block merges to `main` unless from `staging` or verified commit hash.  
- **Audit Trails:** Store deployment metadata (tag, committer, build ID) for traceability.
