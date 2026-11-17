# Zadanie 2 – Modely, používanie a evaluácia jednej úlohy

[Prehľad platformy](../README.md)

Používateľ stiahne kompatibilné základné modely a vyskúša ich v bežných konverzáciách. Pri evaluácii jednej úlohy pripraví testovacie vstupy a referenčné výstupy ručne alebo z CSV. Systém odošle rovnaké vstupy vybraným modelom, vyhodnotí odpovede a porovná počet tokenov a dostupné náklady. Používajú sa stiahnuté váhy bez fine-tuningu.

## Dokumentácia

| Dokument | Obsah |
| --- | --- |
| [Funkčné požiadavky](FUNKCNE_POZIADAVKY.md) | 19 požiadaviek s pôvodnými ID a akceptačnými kritériami. |
| [Architektúra](ARCHITEKTURA.md) | Mikroservisný diagram, zodpovednosti, dátové úložiská a komunikácia medzi službami. |
| [Endpointy](ENDPOINTY.md) | 35 verejných endpointov a navrhnuté interné volania pre tento tok. |
| [API kontrakty](API.md) | Parametre, príklady vstupov a výstupov, sekvenčné diagramy a chybové stavy. |

## Hlavný tok

1. Vybrať a stiahnuť kompatibilné modely, potom ich podľa potreby vyskúšať v konverzácii.
2. Vytvoriť evaluáciu cez `POST /api/v1/evaluations`. Pridať testovacie prípady ručne alebo importovať CSV so stĺpcami `input` a `výsledok`.
3. Spustiť `POST /api/v1/evaluations/{evaluation_id}/runs`. Režim `generate` vykoná rovnaké vstupy pre kandidátov. Režim `existing_conversations` vyhodnotí mapované odpovede z uložených konverzácií.
4. Načítať vyhodnotené prípady a porovnanie kvality, tokenov a dostupných cien cez `/api/v1/evaluation-runs/{run_id}` a jeho podzdroje.

Referenčné odpovede zostávajú v Evaluation Service. Skóre a použitie zdrojov sa priraďujú ku konkrétnemu vstupu a kandidátovi.
