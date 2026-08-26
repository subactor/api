# Policy

Repozytorium ma adoptować, a nie kopiować, pakiety `wellmanifest/new-project`, `wellmanifest/poa`, `wellmanifest/policy-dsl`, `wellmanifest/logs` i `wellmanifest/git-lifecycle`. Rewizje oraz mechanizm adopcji zostaną przypięte przed implementacją runtime'u.

Żaden dokument w tym katalogu nie jest grantem wykonania. `task.submit` musi przed dispatch sprawdzić policy, capability preflight oraz grant powiązany z `plan_hash`.
