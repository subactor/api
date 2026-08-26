# Subactor API — propozycja do decyzji

## Cel

Utworzyć osobne, publiczne repozytorium `subactor/api`, które opisuje i z czasem udostępnia zunifikowany interfejs do zlecania zadań platformie Subactor przez ludzi, programy i dowolne modele LLM.

Intencja nie jest taka, aby model dostał dostęp do terminala, dowolnych endpointów albo sekretów. Model ma zlecić zadanie w tym samym sensie, w którym człowiek zleca je przez `subactor/shell`: podaje cel, odbiera pytania doprecyzowujące, widzi plan i wynik, a wykonanie przechodzi przez istniejące bramy platformy.

## Jak rozumiem obecny podział odpowiedzialności

- `subactor/shell` jest trwałym klientem rozmowy i ACP. Normalizuje język naturalny do ograniczonej intencji, pracuje z katalogiem dozwolonych narzędzi i dla mutacji wymaga wyraźnego wykonania.
- `subactor/platform` jest złożeniem wdrożeniowym: posiada stan operacyjny, tickety, preflight capability, polityki, granty, connectory i logi wykonania.
- `subactor/orchestrator` składa plan oraz deleguje wykonanie do runtime i nazwanych connectorów.
- `subactor/api` ma być stabilną, publiczną fasadą kontraktów i adapterów. Nie kopiuje implementacji platformy i nie staje się drugim execution engine.

## Docelowy przepływ

```text
człowiek / aplikacja / LLM
  -> REST, MCP albo A2A
  -> typowany TaskRequest / Intent
  -> walidacja schematu i policy
  -> plan + capability preflight
  -> istniejący platform / orchestrator / connector
  -> grant wymagany dla mutacji
  -> receipt, status i zredagowane zdarzenia
```

Żaden transport nie może ominąć tego przepływu. LLM może stworzyć propozycję i zlecić zadanie, ale nie może sam nadać sobie uprawnień, wybrać arbitralnego polecenia shell, URI, connectora, sekretu lub worktree.

## Planowany szkic katalogów

```text
api/
  mcp/          # kontrakt serwera MCP i dozwolone narzędzia
  a2a/          # Agent Card, delegacja asynchroniczna, statusy i callbacki
  rest/         # OpenAPI oraz komendy i zapytania HTTP
  contracts/    # wspólne JSON Schema: TaskRequest, Plan, GrantRef, Receipt, Problem
  llm/          # adaptery vendor-neutralne i structured output; bez kluczy modeli
  logs/         # redagowane eventy, korelacja i odnośniki do pełnych logów
  policy/       # adopcja polityk, bez własnego silnika uprawnień
  worktrees/    # na razie tylko kontrakt i granice; bez runtime'u
  docs/         # decyzje architektoniczne oraz instrukcje integracji
  examples/     # przykłady klientów REST/MCP/A2A
  tests/        # conformance transportów względem wspólnych kontraktów
```

## Pierwszy minimalny zakres po zatwierdzeniu

1. Przyjąć `wellmanifest/new-project` jako governance projektu oraz przypiąć jego rewizję.
2. Zdefiniować wspólne, wersjonowane kontrakty żądania, planu, statusu, błędu i receiptu.
3. Udostępnić tylko bezpieczny pionowy przekrój:
   - `task.catalog` / odczyt katalogu możliwości,
   - `task.plan` / planowanie bez skutków ubocznych,
   - `task.submit` / utworzenie delegacji,
   - `task.status` i `receipt.get` / obserwacja wyniku.
4. Zaprojektować REST jako kontrakt referencyjny, a MCP i A2A jako równoważne adaptery do tych samych komend i zapytań.
5. Zintegrować API z istniejącymi bramami platformy, zamiast wystawiać na zewnątrz bazę danych, connector server czy powłokę.
6. Przetestować przykładem: LLM planuje publikację strony w trybie dry-run, dostaje brakujące wymagania lub receipt, ale nie może wykonać mutacji bez właściwego grantu.

## Granice bezpieczeństwa

- REST, MCP i A2A są transportami, a nie źródłami władzy wykonania.
- Każda operacja zmieniająca stan wymaga policy, aktualnego capability preflight i grant/approval związanych z konkretnym planem.
- API nie zwraca tokenów, haseł ani pełnych logów; zwraca zredagowane zdarzenia i stabilne referencje.
- Zapytania odczytowe oraz komendy zmieniające stan muszą pozostać rozdzielone.
- Worktree może zostać utworzony wyłącznie przez platformowy executor po przejściu polityki; w pierwszej wersji będzie jedynie opisany kontraktem.
- LLM nie wybiera dowolnego modelu wykonawczego, komendy systemowej, pliku recipe ani endpointu wewnętrznego.

## Zależności Wellmanifest

- `wellmanifest/new-project`: bootstrap, ticket, intent, governance i publikacja.
- `wellmanifest/poa`: rozdzielenie request -> plan -> grant -> receipt.
- `wellmanifest/policy-dsl`: polityki jako wersjonowany, deterministycznie walidowany kontrakt.
- `wellmanifest/logs`: typowane logi, błędy i obserwowalność.
- `wellmanifest/wellm` oraz DSL: przenośne manifesty i kontrakty procesów.
- `wellmanifest/git-lifecycle`: lifecycle repozytoriów i przyszłe granice worktree.

## Poza zakresem pierwszego etapu

- Nie publikujemy `subactor/platform`, jego sekretów, wewnętrznych adresów ani surowych logów.
- Nie przenosimy do tego repo istniejącego runtime'u, connectorów ani systemu ticketów.
- Nie budujemy uniwersalnego agenta ani autonomicznego dostępu LLM do komputera.
- Nie wdrażamy jeszcze operacji worktree.

## Decyzje potrzebne od właściciela produktu

1. Czy `subactor/api` ma być tylko specyfikacją/SDK, czy również publicznie hostowaną usługą API?
2. Kto jest pierwszym odbiorcą: zewnętrzni developerzy, własne LLM-y/agentów, czy oba segmenty?
3. Czy pierwsza wersja ma dopuszczać jakąkolwiek mutację, czy wyłącznie catalog, plan, status i receipts?
4. Jaki model tożsamości ma być publicznym standardem: OAuth/OIDC, klucze usługowe, czy federacja agentów A2A?
5. Czy `subactor/shell` ma zostać jedynie publicznym klientem referencyjnym, czy ma również używać nowego API jako backendu?

## Stan tego repozytorium

Na moment utworzenia repozytorium zawiera ono wyłącznie ten dokument do konsultacji. Nie ma implementacji API, integracji z platformą, sekretów, konfiguracji produkcyjnej ani zmian w widoczności `subactor/shell`.
