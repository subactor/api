# Subactor API

Publiczny szkic kontraktów dla zlecania zadań do Subactor Platform przez REST, MCP i A2A.

> Status: **draft, bez runtime'u**. Repozytorium nie udostępnia działającego endpointu ani nie jest proxy do wewnętrznego Control API.

## Zasada działania

Każdy transport mapuje do tych samych kontraktów `TaskRequest`, `TaskPlan` i `TaskReceipt`. Transport nie tworzy authority. Planowanie jest bez skutków ubocznych; wykonanie może zostać zrealizowane wyłącznie przez platformowy executor po sprawdzeniu policy, capability i grantu.

## Katalogi

- [`contracts`](contracts/) — kanoniczne JSON Schema.
- [`rest`](rest/) — draft OpenAPI.
- [`mcp`](mcp/) — zamknięty katalog narzędzi MCP.
- [`a2a`](a2a/) — szkic profilu delegacji agent–agent.
- [`llm`](llm/) — granica modeli i structured output.
- [`logs`](logs/) — zredagowane zdarzenia.
- [`policy`](policy/) — zależności Wellmanifest i granice decyzji.
- [`worktrees`](worktrees/) — wyłącznie szkic kontraktu.
- [`docs/PLATFORM-SHELL-INTERFACE-AUDIT.md`](docs/PLATFORM-SHELL-INTERFACE-AUDIT.md) — potwierdzone punkty integracji oraz obszary wymagające dalszego audytu.

Pełne rozumienie celu i pytań decyzyjnych: [`PROPOZYCJA.md`](PROPOZYCJA.md).
