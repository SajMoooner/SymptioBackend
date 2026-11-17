# Zadanie 1 – Modely, fine-tuning a používanie

[Prehľad platformy](../README.md)

Návrh toku od výberu variantu modelu po jeho používanie. Model vybraný z katalógu sa najprv overí pre cieľové zariadenie. Podporovaný lokálny základ možno fine-tunovať a výsledok uložiť do tej istej knižnice. Chat pracuje so základným modelom, výsledkom tréningu aj s API modelom z pripojeného účtu.

## Dokumentácia

| Dokument | Obsah |
| --- | --- |
| [Funkčné požiadavky](FUNKCNE_POZIADAVKY.md) | 25 požiadaviek s pôvodnými ID a akceptačnými kritériami. |
| [Architektúra](ARCHITEKTURA.md) | Mikroservisný diagram, zodpovednosti, dátové úložiská a komunikácia medzi službami. |
| [Endpointy](ENDPOINTY.md) | 37 verejných endpointov a navrhnuté interné volania pre tento tok. |
| [API kontrakty](API.md) | Parametre, príklady vstupov a výstupov, sekvenčné diagramy a chybové stavy. |

## Hlavný tok

1. Overiť zariadenie cez `GET /api/v1/hardware`, vybrať variant a spustiť `POST /api/v1/model-downloads`.
2. Pri tréningu nahrať JSON alebo JPG cez `POST /api/v1/files`, pripraviť úlohu v `POST /api/v1/fine-tuning/jobs` a spustiť ju cez `POST /api/v1/fine-tuning/jobs/{job_id}/start`.
3. Pri vzdialenom modeli vytvoriť pripojenie cez `POST /api/v1/provider-connections` a otestovať ho.
4. Vytvoriť konverzáciu a spustiť generovanie. Stav, odpovede a spotrebu načítať cez príslušné `GET`.

Tréning a pripojenie k poskytovateľovi sú samostatné toky. S bežným stiahnutým modelom možno začať konverzáciu hneď po jeho zaradení do knižnice.
