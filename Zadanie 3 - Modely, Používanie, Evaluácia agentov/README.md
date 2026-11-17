# Zadanie 3 – Modely, používanie a evaluácia agentov

[Prehľad platformy](../README.md)

Používateľ pracuje so stiahnutými modelmi a uloženými agentmi. Kandidát je konkrétna kombinácia modelu, inštrukcií a parametrov. Všetky vetvy dostanú spoločný vstup a zachovajú si vlastnú históriu. Používateľ po porovnaní vyberie lepší chat a uloží odpoveď. Váhy modelov zostávajú v tomto toku základné.

## Dokumentácia

| Dokument | Obsah |
| --- | --- |
| [Funkčné požiadavky](FUNKCNE_POZIADAVKY.md) | 21 požiadaviek s pôvodnými ID a akceptačnými kritériami. |
| [Architektúra](ARCHITEKTURA.md) | Mikroservisný diagram, zodpovednosti, dátové úložiská a komunikácia medzi službami. |
| [Endpointy](ENDPOINTY.md) | 32 verejných endpointov a navrhnuté interné volania pre tento tok. |
| [API kontrakty](API.md) | Parametre, príklady vstupov a výstupov, sekvenčné diagramy a chybové stavy. |

## Hlavný tok

1. Stiahnuť kompatibilné modely, pripraviť agentové profily cez `POST /api/v1/agents` a podľa potreby ich použiť v bežnej konverzácii.
2. Vytvoriť kandidátov cez `POST /api/v1/comparisons`. Každý kandidát dostane samostatnú konverzáciu.
3. Odoslať spoločný text alebo obrázok cez `POST /api/v1/comparisons/{comparison_id}/messages`. Pri prijatí kola sa uloží použitá verzia agentových inštrukcií.
4. Načítať odpovede kola, uložiť preferenciu cez `PUT /api/v1/comparisons/{comparison_id}/turns/{turn_id}/preference` a skontrolovať spotrebu jednotlivých vetiev.

Toto zadanie ukladá používateľovu preferenciu. Na automatické porovnanie s očakávaným výstupom možno spojiť agentové profily s [taskovou evaluáciou](../Zadanie%202%20-%20Modely%2C%20Pou%C5%BE%C3%ADvanie%2C%20Evalu%C3%A1cia%20%C3%BAlohy/README.md).
