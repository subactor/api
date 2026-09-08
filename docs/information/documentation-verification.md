---
{
  "schema": "wellmanifest.docs/document/v1",
  "id": "documentation-verification",
  "kind": "information",
  "version": 1,
  "title": "Protected API documentation verification",
  "status": "proposed",
  "owner": "subactor/api",
  "created": "2026-09-08",
  "updated": "2026-09-08",
  "review_after": "2026-09-15",
  "source_revision": "6b85e810d128b229856e02ea138d38d5f8757abb",
  "affected_repositories": [
    "subactor/api"
  ],
  "evidence": [
    "https://github.com/subactor/api/issues/4",
    "https://github.com/wellmanifest/docs/commit/ebe7501063ef4f3e63ded610c2d3183010ca636e"
  ]
}
---

# Protected API documentation verification

<!-- docs:section purpose -->
## Purpose

Enforce documentation placement and metadata independently of the IDE or LLM producing the candidate.

<!-- docs:section scope -->
## Scope

API adopts Docs 0.1.1 through an immutable pin, agent instructions and documentation index. The executor appends the installed checker after its existing OpenAPI JSON syntax check. Canonical ticket worktrees are ignored; existing foreign worktrees and local files are preserved.

<!-- docs:section evidence -->
## Evidence

The source revision above binds the observed missing adoption or executor gate. API had no Docs pin and its deployed profile only parsed rest/openapi.v1.json. Local validation passed the OpenAPI JSON syntax check, 703 OneDev tests, Ruff and configuration validation. Six installed-checker cases accepted a valid document and rejected removed metadata, an unmanaged new document, a wrong pin, a symlink and candidate checker substitution. The candidate checker did not execute. Negative receipt SHA-256: `11a122643fd380fc69c19d88b4ff23e2beb903ae7ffbaed15f4bf5311a4b2bcb`. Independent source publication, deployment and real canary remain separate evidence.

<!-- docs:section content -->
## Execution contract

The installed /opt/wellmanifest-docs/docs/standard/check.py uses revision `ebe7501063ef4f3e63ded610c2d3183010ca636e` and policy SHA-256 `f6ba9c011ea1d9260e7fac3a1638a767d5ebc9f7d9b32ed51cc3aea22fe95d8c`. Protected CI binds subactor/api and the full existing base commit. Child Git mapping preserves canonical repository identity without modifying the mirror configuration. Candidate checker code is not selected.

<!-- docs:section limitations -->
## Limitations

This structural check does not certify semantic truth, all IDE/LLM sessions, every operating system or bypass permissions. The retained JSON syntax check does not establish OpenAPI semantic validity or API behavior. No API schema, endpoint, service or production data changes. Historical documents remain unchanged until their next material revision.

<!-- docs:section next_actions -->
## Publication and rollback

Publish adoption and executor source independently, deploy only the approved API command delta and verify a real canary on its exact head/current base before independent publication. Preserve the previous configuration for rollback. Global acceptance stays in the [canonical plan](https://github.com/subactor/docs/blob/main/architecture/refactoring/wellmanifest-enforcement.md).
