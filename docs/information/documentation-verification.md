---
{
  "schema": "wellmanifest.docs/document/v1",
  "id": "documentation-verification",
  "kind": "information",
  "version": 2,
  "title": "Protected API documentation verification",
  "status": "proposed",
  "owner": "subactor/api",
  "created": "2026-09-08",
  "updated": "2026-09-08",
  "review_after": "2026-09-15",
  "source_revision": "70ed9db6d73b90d4b9f20f0c163c6e636ffd5cf0",
  "affected_repositories": [
    "subactor/api"
  ],
  "evidence": [
    "https://github.com/subactor/api/issues/4",
    "https://github.com/wellmanifest/docs/commit/ebe7501063ef4f3e63ded610c2d3183010ca636e",
    "https://github.com/subactor/api/pull/5",
    "https://github.com/subactor/onedev-agent/pull/250",
    "receipt:api-docs-gate-20260908/deployment-verified.json#sha256:49833bc183732bbf5e50209fe47d52042c6826a19bbe73f08d921ee1b4a3bfa3"
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


## Version 2 — deployed protected documentation gate

API #5 was independently merged as `70ed9db6d73b90d4b9f20f0c163c6e636ffd5cf0`, review `5146749055`, tested/merged tree `07aba2ea2c3e74073802bfe6afa47c561242e0ab`. OneDev #250 was independently merged as `49c0588a284a0cdc37891537d2005ee31fab9c26`, review `5146755820`, tested/merged tree `3c3f70a102883a1a600a48e3ef37fb26e490dd58`. Source CI passed the API JSON syntax check and five OneDev gates including 703 tests.

Deployment changed only the API commands from one gate to two: existing OpenAPI JSON syntax validation and installed Docs. Both services read back configuration SHA-256 `3deb6420d2a5ae5661042d9a84bdadcea1c2acc068c92a00a2b12bf53a3f89a9`; previous configuration: `111713746a65e45bdf85c585bf6639c1fe9c53fa6fd2410612d724621a9028ca`. Image `sha256:c8ca169ab452bc2d0ae484c4f32dc488d47ba6bd417b5e3fbb5c255d0e1d50c2`, environment, other mounts, networks, timeout and other profiles were preserved. The switch observed empty pending/running queues. Previous Compose selection remains available for rollback.

This material documentation revision is the real post-deployment canary. Both protected gates must succeed on its exact head merged with the current base before independent publication; terminal head/base/tree and review bindings are recorded by external publication receipts. The existing protected Validator profile requires onedev/local-verify and has scheduled_scan=false; publication therefore uses the explicit trusted local adapter under session authorization. This change does not modify the protected registry, timers or publication checks.

Docs itself needed no source change for the observed gap. API adoption and executor wiring required correction. Structural document validation does not certify semantic truth, all IDE/LLM sessions, other profiles, untested operating systems or bypass permissions. API syntax validation is not semantic OpenAPI validation or an endpoint behavior test. Those wider criteria remain open in the canonical plan.
