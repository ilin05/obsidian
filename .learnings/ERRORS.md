# Errors

Command failures and integration errors.

---

## [ERR-20260508-004] stale_operating_guide_links

**Logged**: 2026-05-08T15:53:00+08:00
**Priority**: low
**Status**: resolved
**Area**: docs

### Summary
`index.md` referenced `CACHE.md`, `reviewer.md`, and `writer.md`, but those root files were absent when starting the ANNS system guide task.

### Resolution
Created the missing files as general ANNS system guides and updated `index.md` operating-guide labels from CXL-Vector-specific to ANNS-system-specific.

---

## [ERR-20260508-003] shell_pattern_backtick_quote

**Logged**: 2026-05-08T15:43:00+08:00
**Priority**: low
**Status**: resolved
**Area**: docs

### Summary
An `rg` verification command failed because the shell pattern was double-quoted while containing a markdown backtick, causing zsh quote parsing to break.

### Resolution
Rerun shell searches that contain markdown backticks or literal special characters with single-quoted patterns.

---

## [ERR-20260508-002] moving_inbox_file_after_external_rename

**Logged**: 2026-05-08T15:08:00+08:00
**Priority**: low
**Status**: resolved
**Area**: docs

### Summary
Moving `raw/inbox/p1964-wang.pdf` failed because the inbox contents changed and the file was no longer present under that name.

### Resolution
Rechecked `raw/inbox/`, identified the same PVLDB graph-ANNS survey as `survey-on-anns.pdf`, and moved that actual file to `raw/sources/papers/graph-based-anns-survey-2021.pdf`.

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
