# Endpointy – Zadanie 2

[Späť na zadanie](README.md) · [API kontrakty](API.md) · [Architektúra](ARCHITEKTURA.md)

Toto zadanie používa 35 verejných trás. Pri každej skupine je uvedená služba, na ktorú požiadavku smeruje API Gateway.

## Stiahnutie a správa LLM modelov

Vlastník je **Model Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `GET` | `/api/v1/hardware` | Zistiť výpočtové zdroje zariadenia, na ktorom sa majú lokálne modely spúšťať. | [Kontrakt](API.md#01-zistenie-hardv%C3%A9ru) |
| `GET` | `/api/v1/model-catalog` | Vyhľadať modely z Hugging Face a Ollama a filtrovať ich podľa zdroja, kompresie a vhodnosti pre zariadenie. | [Kontrakt](API.md#02-vyh%C4%BEadanie-modelov-v-katal%C3%B3gu) |
| `GET` | `/api/v1/model-catalog/{catalog_model_id}` | Zobraziť detail modelu dostupného na stiahnutie. | [Kontrakt](API.md#03-detail-katal%C3%B3gov%C3%A9ho-modelu) |
| `GET` | `/api/v1/model-catalog/{catalog_model_id}/variants` | Zobraziť dostupné varianty kompresie vybraného modelu. | [Kontrakt](API.md#04-varianty-kompresie) |
| `GET` | `/api/v1/model-catalog/{catalog_model_id}/variants/{variant_id}/compatibility` | Overiť vhodnosť variantu pre zariadenie a vrátiť dôvod prípadného blokovania. | [Kontrakt](API.md#05-kompatibilita-variantu) |
| `POST` | `/api/v1/model-downloads` | Spustiť sťahovanie zvoleného modelu a variantu. Pred začatím vynútiť kontrolu výkonu zariadenia a nevhodný model zablokovať. | [Kontrakt](API.md#06-spustenie-s%C5%A5ahovania) |
| `GET` | `/api/v1/model-downloads/{download_id}` | Zobraziť stav a priebeh sťahovania; po dokončení vrátiť odkaz na model v knižnici alebo chybu. | [Kontrakt](API.md#07-stav-s%C5%A5ahovania) |
| `GET` | `/api/v1/models` | Zobraziť základné stiahnuté modely v knižnici. | [Kontrakt](API.md#08-kni%C5%BEnica-modelov) |
| `GET` | `/api/v1/models/{model_id}` | Načítať detail stiahnutého modelu a jeho podporované vstupy a možnosti. | [Kontrakt](API.md#09-detail-modelu-kni%C5%BEnice) |
| `DELETE` | `/api/v1/models/{model_id}` | Odstrániť stiahnutý model z knižnice a zo zariadenia. | [Kontrakt](API.md#10-odstr%C3%A1nenie-lok%C3%A1lneho-modelu) |

## Súbory

Vlastník je **File Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `POST` | `/api/v1/files` | Nahrať obrázok do konverzácie alebo CSV s testovacími prípadmi a získať `file_id`. | [Kontrakt](API.md#11-nahratie-s%C3%BAboru) |
| `GET` | `/api/v1/files/{file_id}/content` | Načítať obsah nahratého súboru alebo zobraziť uložený obrázok. | [Kontrakt](API.md#12-na%C4%8D%C3%ADtanie-obsahu-s%C3%BAboru) |

## Konverzácie a používanie modelov

Vlastník je **Chat Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `POST` | `/api/v1/conversations` | Vytvoriť konverzáciu so stiahnutým modelom. | [Kontrakt](API.md#30-vytvorenie-konverz%C3%A1cie) |
| `GET` | `/api/v1/conversations` | Zobraziť uložené konverzácie. | [Kontrakt](API.md#31-zoznam-konverz%C3%A1ci%C3%AD) |
| `GET` | `/api/v1/conversations/{conversation_id}` | Načítať detail konverzácie a priradený model. | [Kontrakt](API.md#32-detail-konverz%C3%A1cie) |
| `GET` | `/api/v1/conversations/{conversation_id}/messages` | Načítať celú uloženú históriu správ a odpovedí v správnom poradí. | [Kontrakt](API.md#33-hist%C3%B3ria-spr%C3%A1v) |
| `POST` | `/api/v1/generations` | Spustiť generovanie základného lokálneho modelu a podľa konverzácie použiť jej kontext. | [Kontrakt](API.md#34-spustenie-generovania) |
| `GET` | `/api/v1/generations/{generation_id}` | Načítať stav generovania, požiadavky na nástroje a po ukončení úplnú odpoveď alebo chybu. | [Kontrakt](API.md#35-stav-a-v%C3%BDsledok-generovania) |
| `POST` | `/api/v1/generations/{generation_id}/tool-results` | Doplniť výsledky nástrojov vykonaných klientom a umožniť pokračovanie generovania čakajúceho na tieto výsledky. | [Kontrakt](API.md#36-odovzdanie-v%C3%BDsledkov-n%C3%A1strojov) |
| `GET` | `/api/v1/conversations/{conversation_id}/usage` | Zobraziť priebežné použitie tokenov, cenu a prepočet na 1 milión tokenov pre danú konverzáciu. | [Kontrakt](API.md#37-spotreba-konverz%C3%A1cie) |

## Evaluácia podľa úlohy

Vlastník je **Evaluation Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `POST` | `/api/v1/evaluations` | Vytvoriť evaluáciu konkrétnej úlohy s vybranými lokálnymi modelmi. | [Kontrakt](API.md#38-vytvorenie-evalu%C3%A1cie) |
| `GET` | `/api/v1/evaluations` | Zobraziť uložené evaluácie. | [Kontrakt](API.md#39-zoznam-evalu%C3%A1ci%C3%AD) |
| `GET` | `/api/v1/evaluations/{evaluation_id}` | Zobraziť nastavenia evaluácie, vybrané modely a priradené konverzácie. | [Kontrakt](API.md#40-detail-evalu%C3%A1cie) |
| `PATCH` | `/api/v1/evaluations/{evaluation_id}` | Upraviť zadanie a výber modelov pre ďalší beh evaluácie. | [Kontrakt](API.md#41-%C3%BAprava-evalu%C3%A1cie) |
| `POST` | `/api/v1/evaluations/{evaluation_id}/test-cases` | Pridať testovací vstup a jeho očakávaný výstup. | [Kontrakt](API.md#42-pridanie-testovacieho-pr%C3%ADpadu) |
| `GET` | `/api/v1/evaluations/{evaluation_id}/test-cases` | Zobraziť uložené testovacie vstupy a očakávané výstupy. | [Kontrakt](API.md#43-zoznam-testovac%C3%ADch-pr%C3%ADpadov) |
| `PATCH` | `/api/v1/evaluations/{evaluation_id}/test-cases/{test_case_id}` | Upraviť vstup alebo očakávaný výstup testovacieho prípadu pre ďalší beh. | [Kontrakt](API.md#44-%C3%BAprava-testovacieho-pr%C3%ADpadu) |
| `POST` | `/api/v1/evaluations/{evaluation_id}/imports` | Načítať prípady z nahratého CSV s poľami input a výsledok do evaluácie; každý dátový riadok vytvorí testovací prípad. | [Kontrakt](API.md#45-spustenie-importu-csv) |
| `GET` | `/api/v1/evaluations/{evaluation_id}/imports/{import_id}` | Zobraziť stav importu, počet načítaných prípadov a prípadné chyby CSV. | [Kontrakt](API.md#46-stav-importu-csv) |
| `PUT` | `/api/v1/evaluations/{evaluation_id}/conversations/{conversation_id}` | Priradiť existujúcu konverzáciu k evaluácii; rovnakú evaluáciu možno priradiť viacerým konverzáciám. | [Kontrakt](API.md#47-priradenie-konverz%C3%A1cie) |
| `POST` | `/api/v1/evaluations/{evaluation_id}/runs` | Spustiť beh s rovnakým vstupom pre všetky zvolené modely alebo nad uloženými konverzáciami. Výstupy sa porovnajú s referenciami. | [Kontrakt](API.md#48-spustenie-evalua%C4%8Dn%C3%A9ho-behu) |
| `GET` | `/api/v1/evaluations/{evaluation_id}/runs` | Zobraziť históriu behov konkrétnej evaluácie. | [Kontrakt](API.md#49-hist%C3%B3ria-evalua%C4%8Dn%C3%BDch-behov) |
| `GET` | `/api/v1/evaluation-runs/{run_id}` | Zobraziť aktuálny stav a priebeh vyhodnocovania jednotlivých konverzácií a prípadov. | [Kontrakt](API.md#50-stav-evalua%C4%8Dn%C3%A9ho-behu) |
| `GET` | `/api/v1/evaluation-runs/{run_id}/results` | Zobraziť vstup, skutočný a očakávaný výstup, výsledok porovnania a chyby pre jednotlivé prípady a modely. | [Kontrakt](API.md#51-v%C3%BDsledky-testovac%C3%ADch-pr%C3%ADpadov) |
| `GET` | `/api/v1/evaluation-runs/{run_id}/comparison` | Porovnať úspešnosť modelov, spotrebu tokenov a dostupné náklady. | [Kontrakt](API.md#52-porovnanie-v%C3%BDsledkov-kandid%C3%A1tov) |

## Navrhnuté interné endpointy

Tieto trasy zabezpečujú súkromné volania medzi službami. API Gateway smeruje verejné požiadavky cez `/api/v1`.

| HTTP a cesta | Vlastník | Volajúci | Účel |
| --- | --- | --- | --- |
| `GET /internal/v1/devices/{device_id}/hardware` | Runtime Service | Model Service | Aktuálne zdroje skutočného výpočtového zariadenia; verejný GET /api/v1/hardware ostáva fasádou Model Service. |
| `DELETE /internal/v1/runtime-models/{runtime_model_id}` | Runtime Service | Model Service | Uvoľniť a odstrániť modelové súbory po kontrole aktívnych použití a závislostí adaptérov. |
| `POST /internal/v1/files/resolve` | File Service | Chat Service, Evaluation Service | Dávkovo overiť prístup, typ a pripravenosť file_id a vrátiť metadáta bez presúvania binárneho obsahu cez broker. |
| `POST /internal/v1/generation-batches` | Chat Service | Evaluation Service | Prijať dávku pevných vstupov s kandidátmi a vrátiť identifikátory generovaní. |

Interná dávka generovaní prenáša model, vstup a ID kandidáta pre každú položku. Výsledok sa mapuje cez `generation_id`. Očakávané odpovede drží Evaluation Service.

Asynchrónne príkazy, udalosti a ich smer sú uvedené v [architektúre](ARCHITEKTURA.md).
