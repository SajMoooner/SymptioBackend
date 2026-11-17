# Endpointy – Zadanie 1

[Späť na zadanie](README.md) · [API kontrakty](API.md) · [Architektúra](ARCHITEKTURA.md)

Toto zadanie používa 37 verejných trás. Pri každej skupine je uvedená služba, na ktorú požiadavku smeruje API Gateway.

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
| `GET` | `/api/v1/models` | Zobraziť knižnicu modelov dostupných v Evalve vrátane stiahnutých, fine-tunovaných a pripojených API modelov. | [Kontrakt](API.md#08-kni%C5%BEnica-modelov) |
| `GET` | `/api/v1/models/{model_id}` | Zobraziť detail modelu z knižnice a jeho podporované možnosti používania a fine-tuningu. | [Kontrakt](API.md#09-detail-modelu-kni%C5%BEnice) |
| `DELETE` | `/api/v1/models/{model_id}` | Odstrániť stiahnutý alebo fine-tunovaný lokálny model z knižnice a úložiska. | [Kontrakt](API.md#10-odstr%C3%A1nenie-lok%C3%A1lneho-modelu) |

## Súbory

Vlastník je **File Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `POST` | `/api/v1/files` | Nahrať tréningový JSON alebo obrázok a získať `file_id`. | [Kontrakt](API.md#11-nahratie-s%C3%BAboru) |
| `GET` | `/api/v1/files/{file_id}/content` | Načítať obsah nahratého súboru alebo zobraziť uložený obrázok. | [Kontrakt](API.md#12-na%C4%8D%C3%ADtanie-obsahu-s%C3%BAboru) |

## Fine-tuning LLM modelov

Vlastník je **Training Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `POST` | `/api/v1/fine-tuning/jobs` | Vytvoriť pripravený tréning so zvoleným stiahnutým modelom, tréningovými súbormi a parametrami. | [Kontrakt](API.md#13-pr%C3%ADprava-tr%C3%A9ningu) |
| `GET` | `/api/v1/fine-tuning/jobs` | Zobraziť zoznam pripravených, bežiacich a ukončených tréningov. | [Kontrakt](API.md#14-zoznam-tr%C3%A9ningov) |
| `GET` | `/api/v1/fine-tuning/jobs/{job_id}` | Zobraziť konfiguráciu, aktuálny stav a priebeh konkrétneho tréningu. | [Kontrakt](API.md#15-stav-tr%C3%A9ningu) |
| `PATCH` | `/api/v1/fine-tuning/jobs/{job_id}` | Upraviť podporované parametre tréningu pred jeho spustením. | [Kontrakt](API.md#16-%C3%BAprava-parametrov-tr%C3%A9ningu) |
| `POST` | `/api/v1/fine-tuning/jobs/{job_id}/start` | Spustiť pripravený tréning. Po úspešnom dokončení sa výsledný model automaticky zaradí do knižnice. | [Kontrakt](API.md#17-spustenie-tr%C3%A9ningu) |
| `POST` | `/api/v1/fine-tuning/jobs/{job_id}/cancel` | Zastaviť bežiaci tréning alebo zrušiť tréning čakajúci na spustenie. | [Kontrakt](API.md#18-zastavenie-tr%C3%A9ningu) |
| `GET` | `/api/v1/fine-tuning/jobs/{job_id}/metrics` | Zobraziť priebežne zaznamenané výsledky a metriky tréningu. | [Kontrakt](API.md#19-priebe%C5%BEn%C3%A9-metriky-tr%C3%A9ningu) |
| `GET` | `/api/v1/fine-tuning/jobs/{job_id}/result` | Zobraziť záverečný výsledok tréningu a odkaz na vytvorený model v knižnici alebo chybu tréningu. | [Kontrakt](API.md#20-v%C3%BDsledok-tr%C3%A9ningu) |

## Pripojenie API poskytovateľov

Vlastník je **Provider Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `GET` | `/api/v1/providers` | Zobraziť podporovaných poskytovateľov, najmä OpenAI a Anthropic pre modely Claude. | [Kontrakt](API.md#21-podporovan%C3%AD-poskytovatelia) |
| `POST` | `/api/v1/provider-connections` | Vytvoriť pripojenie k poskytovateľovi a uložiť jeho prístupové údaje. | [Kontrakt](API.md#22-vytvorenie-pripojenia) |
| `GET` | `/api/v1/provider-connections` | Zobraziť uložené pripojenia k poskytovateľom. | [Kontrakt](API.md#23-zoznam-pripojen%C3%AD) |
| `GET` | `/api/v1/provider-connections/{connection_id}` | Zobraziť nastavenia a stav pripojenia s maskovanými prístupovými údajmi. | [Kontrakt](API.md#24-detail-pripojenia) |
| `PATCH` | `/api/v1/provider-connections/{connection_id}` | Aktualizovať nastavenia alebo prístupové údaje existujúceho pripojenia. | [Kontrakt](API.md#25-%C3%BAprava-pripojenia) |
| `DELETE` | `/api/v1/provider-connections/{connection_id}` | Odstrániť uložené pripojenie poskytovateľa z Evalva. | [Kontrakt](API.md#26-odstr%C3%A1nenie-pripojenia) |
| `POST` | `/api/v1/provider-connections/{connection_id}/test` | Overiť pripojenie a platnosť prístupových údajov; vrátiť výsledok kontroly alebo chybu. | [Kontrakt](API.md#27-test-pripojenia) |
| `GET` | `/api/v1/provider-connections/{connection_id}/models` | Zobraziť dostupné modely pripojeného poskytovateľa, podporované úrovne premýšľania, obrazové vstupy a nástroje. | [Kontrakt](API.md#28-modely-poskytovate%C4%BEa) |
| `GET` | `/api/v1/models/{model_id}/pricing` | Zobraziť cenník vybraného API modelu za 1 milión tokenov vrátane meny a príslušných vstupných a výstupných sadzieb. | [Kontrakt](API.md#29-cenn%C3%ADk-modelu) |

## Konverzácie a používanie modelov

Vlastník je **Chat Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `POST` | `/api/v1/conversations` | Vytvoriť konverzáciu s modelom z knižnice. | [Kontrakt](API.md#30-vytvorenie-konverz%C3%A1cie) |
| `GET` | `/api/v1/conversations` | Zobraziť uložené konverzácie. | [Kontrakt](API.md#31-zoznam-konverz%C3%A1ci%C3%AD) |
| `GET` | `/api/v1/conversations/{conversation_id}` | Načítať detail konverzácie a priradený model. | [Kontrakt](API.md#32-detail-konverz%C3%A1cie) |
| `GET` | `/api/v1/conversations/{conversation_id}/messages` | Načítať celú uloženú históriu správ a odpovedí v správnom poradí. | [Kontrakt](API.md#33-hist%C3%B3ria-spr%C3%A1v) |
| `POST` | `/api/v1/generations` | Spustiť generovanie vybraného lokálneho, API alebo fine-tunovaného modelu. Pri konverzácii načítať jej kontext a uložiť nové správy. | [Kontrakt](API.md#34-spustenie-generovania) |
| `GET` | `/api/v1/generations/{generation_id}` | Načítať stav generovania, požiadavky na nástroje a po ukončení úplnú odpoveď alebo chybu. | [Kontrakt](API.md#35-stav-a-v%C3%BDsledok-generovania) |
| `POST` | `/api/v1/generations/{generation_id}/tool-results` | Doplniť výsledky nástrojov vykonaných klientom a umožniť pokračovanie generovania čakajúceho na tieto výsledky. | [Kontrakt](API.md#36-odovzdanie-v%C3%BDsledkov-n%C3%A1strojov) |
| `GET` | `/api/v1/conversations/{conversation_id}/usage` | Zobraziť priebežné použitie tokenov, cenu a prepočet na 1 milión tokenov pre danú konverzáciu. | [Kontrakt](API.md#37-spotreba-konverz%C3%A1cie) |

## Navrhnuté interné endpointy

Tieto trasy zabezpečujú súkromné volania medzi službami. API Gateway smeruje verejné požiadavky cez `/api/v1`.

| HTTP a cesta | Vlastník | Volajúci | Účel |
| --- | --- | --- | --- |
| `GET /internal/v1/devices/{device_id}/hardware` | Runtime Service | Model Service, Training Service | Aktuálne zdroje skutočného výpočtového zariadenia; verejný GET /api/v1/hardware ostáva fasádou Model Service. |
| `DELETE /internal/v1/runtime-models/{runtime_model_id}` | Runtime Service | Model Service | Uvoľniť a odstrániť modelové súbory po kontrole aktívnych použití a závislostí adaptérov. |
| `POST /internal/v1/models/registrations` | Model Service | Training Service, Provider Service | Idempotentne zaregistrovať hotový tréningový artefakt alebo API model a vrátiť kanonické model_id. |
| `POST /internal/v1/files/resolve` | File Service | Chat Service, Training Service, Evaluation Service | Dávkovo overiť prístup, typ a pripravenosť file_id a vrátiť metadáta bez presúvania binárneho obsahu cez broker. |

Training Service používa internú registráciu pri publikovaní hotového modelu. Provider Service ju používa pri pridaní API modelu. Údaje o pripojení ostávajú vo vlastníctve Provider Service.

Asynchrónne príkazy, udalosti a ich smer sú uvedené v [architektúre](ARCHITEKTURA.md).
