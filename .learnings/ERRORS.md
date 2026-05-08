# Errors

Command failures and integration errors.

---

## [ERR-20260508-001] git_branch_rename_sandbox

**Logged**: 2026-05-08T11:20:00+08:00
**Priority**: low
**Status**: resolved
**Area**: infra

### Summary
`git branch -m main` failed under default sandbox permissions because it could not create `.git/HEAD.lock`.

### Resolution
Retried the branch rename with approved elevated permissions after `git init` succeeded. Repository branch is now `main`.

---
