# Granica LLM

Adapter LLM może przekształcić rozmowę do `TaskRequest` wyłącznie przez ograniczony structured output walidowany względem `contracts/task-request.v1.schema.json`. Nie wybiera URI wykonawczego, connectora, komendy powłoki, sekretu ani grantu. Dostawca modelu i jego credentiale pozostają poza publicznym kontraktem API.
