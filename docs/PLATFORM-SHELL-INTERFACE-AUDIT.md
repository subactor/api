# Audyt granic `platform` i `shell` — 2026-08-26

## Potwierdzone lokalnie

### Shell

- `shell/src/subactor_shell/control.py` tworzy JSON-RPC MCP endpoint `/mcp/accounts/{account_id}/providers/{provider}/tools/{tool_id}`.
- Shell domyślnie wymaga dokładnej allowlisty `cli.status`, `cli.plan`, `cli.execute` i odmawia `cli.execute` bez jawnego `allow_execute`.
- `shell/docs/INTEGRATION.md` wskazuje, że bridge jest instalowany obok Control, nie na tej samej trasie MCP; katalog i capability preflight pozostają po stronie platformy.

### Platform / Control

- `core/services/control/src/routes/system.mjs` udostępnia m.in. `GET /health`, `GET /api/system/dashboard`, `GET /api/tickets` i `GET /api/tickets/{id}/timeline`.
- `core/services/control/src/routes/ticket-lifecycle.mjs` udostępnia kontrolowane `POST /api/tickets/lifecycle/reconcile`, `POST /api/tickets/lifecycle/run` oraz readiness ticketu.
- `core/services/control/src/routes/llm-account-hub.mjs` rozdziela plan, autoryzację i wykonanie CLI: `/api/llm-account-hub/cli/plan`, `/authorize`, `/execute`.
- `core/services/control/src/routes/process-packs.mjs`, `connectors.mjs` i `plans.mjs` potwierdzają, że procesy, connectory i lifecycle planu mają własne granice.
- `platform/bin/subactor` jest cienkim wrapperem CLI do `platform/packages/founder-cli`; nie jest stabilnym publicznym API kontraktowym.

## Wniosek projektowy

Publiczne API nie może bezpośrednio re-eksportować obecnych tras Control. Są to trasy operacyjne, częściowo founderskie albo wewnętrzne. `subactor/api` definiuje mniejszy, stabilny zbiór `catalog`, `plan`, `submit`, `status`, `receipt`, a adapter do platformy musi jawnie mapować każdą z nich do potwierdzonego kontraktu wewnętrznego.

## Otwarte czynności przed runtime'em

1. Zweryfikować payloady i autoryzację konkretnych tras w testach Control, bez publikowania ich jako część publicznego API.
2. Potwierdzić docelowy issuer grantów, lifetime i wiązanie z `plan_hash`.
3. Ustalić model identity oraz publiczny base URL.
4. Wybrać i przypiąć wersję specyfikacji A2A.
5. Przypiąć rewizje Wellmanifest przez jego proces adopcji.
