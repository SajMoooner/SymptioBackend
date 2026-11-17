# Endpointy – Zadanie 3

[Späť na zadanie](README.md) · [API kontrakty](API.md) · [Architektúra](ARCHITEKTURA.md)

Toto zadanie používa 32 verejných trás. Pri každej skupine je uvedená služba, na ktorú požiadavku smeruje API Gateway.

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
| `POST` | `/api/v1/files` | Nahrať obrázok pre konverzáciu alebo porovnávacie kolo a získať `file_id`. | [Kontrakt](API.md#11-nahratie-s%C3%BAboru) |
| `GET` | `/api/v1/files/{file_id}/content` | Načítať obsah nahratého súboru alebo zobraziť uložený obrázok. | [Kontrakt](API.md#12-na%C4%8D%C3%ADtanie-obsahu-s%C3%BAboru) |

## Konverzácie a používanie modelov

Vlastník je **Chat Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `POST` | `/api/v1/conversations` | Vytvoriť konverzáciu so stiahnutým modelom a voliteľným uloženým agentom. | [Kontrakt](API.md#30-vytvorenie-konverz%C3%A1cie) |
| `GET` | `/api/v1/conversations` | Zobraziť uložené konverzácie. | [Kontrakt](API.md#31-zoznam-konverz%C3%A1ci%C3%AD) |
| `GET` | `/api/v1/conversations/{conversation_id}` | Zobraziť detail konverzácie a jej priradený model a agenta. | [Kontrakt](API.md#32-detail-konverz%C3%A1cie) |
| `GET` | `/api/v1/conversations/{conversation_id}/messages` | Načítať celú uloženú históriu správ a odpovedí v správnom poradí. | [Kontrakt](API.md#33-hist%C3%B3ria-spr%C3%A1v) |
| `POST` | `/api/v1/generations` | Spustiť generovanie základného lokálneho modelu. Pri použití agenta zachytiť a vložiť jeho inštrukcie do kontextu. Nové správy sa ukladajú do príslušnej konverzácie. | [Kontrakt](API.md#34-spustenie-generovania) |
| `GET` | `/api/v1/generations/{generation_id}` | Načítať stav generovania, požiadavky na nástroje a po ukončení úplnú odpoveď alebo chybu. | [Kontrakt](API.md#35-stav-a-v%C3%BDsledok-generovania) |
| `POST` | `/api/v1/generations/{generation_id}/tool-results` | Doplniť výsledky nástrojov vykonaných klientom a umožniť pokračovanie generovania čakajúceho na tieto výsledky. | [Kontrakt](API.md#36-odovzdanie-v%C3%BDsledkov-n%C3%A1strojov) |
| `GET` | `/api/v1/conversations/{conversation_id}/usage` | Zobraziť priebežné použitie tokenov, cenu a prepočet na 1 milión tokenov pre danú konverzáciu. | [Kontrakt](API.md#37-spotreba-konverz%C3%A1cie) |

## Agentové profily

Vlastník je **Agent Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `POST` | `/api/v1/agents` | Vytvoriť a uložiť agenta s inštrukciami a priradeným modelom. | [Kontrakt](API.md#53-vytvorenie-agenta) |
| `GET` | `/api/v1/agents` | Zobraziť uložených agentov dostupných na výber. | [Kontrakt](API.md#54-zoznam-agentov) |
| `GET` | `/api/v1/agents/{agent_id}` | Načítať inštrukcie a priradený model agenta; umožniť jeho použitie cez odkaz v požiadavke. | [Kontrakt](API.md#55-detail-agenta) |
| `PATCH` | `/api/v1/agents/{agent_id}` | Upraviť uložené inštrukcie alebo priradený model pre nasledujúce použitia agenta. | [Kontrakt](API.md#56-%C3%BAprava-agenta) |

## Porovnávanie agentových prístupov

Vlastník je **Evaluation Service**.

| HTTP | Endpoint | Účel | Detail |
| --- | --- | --- | --- |
| `POST` | `/api/v1/comparisons` | Vytvoriť porovnanie stiahnutých modelov s voliteľnými agentmi a so samostatnou konverzáciou pre každého kandidáta. | [Kontrakt](API.md#57-vytvorenie-porovnania) |
| `GET` | `/api/v1/comparisons` | Zobraziť uložené porovnania modelov a agentových prístupov. | [Kontrakt](API.md#58-zoznam-porovnan%C3%AD) |
| `GET` | `/api/v1/comparisons/{comparison_id}` | Zobraziť vybrané modely, agentov a jednotlivé konverzácie daného porovnania. | [Kontrakt](API.md#59-detail-porovnania) |
| `POST` | `/api/v1/comparisons/{comparison_id}/messages` | Odoslať rovnaký text alebo obrázok vybraným modelom, načítať odkazovaného agenta a zahrnúť jeho inštrukcie do promptu. Vytvoriť porovnávacie kolo. | [Kontrakt](API.md#60-spustenie-porovn%C3%A1vacieho-kola) |
| `GET` | `/api/v1/comparisons/{comparison_id}/turns/{turn_id}` | Zobraziť stav a odpovede jednotlivých modelov v kole, aby používateľ mohol po dokončení vybrať lepší chat. | [Kontrakt](API.md#61-v%C3%BDsledky-porovn%C3%A1vacieho-kola) |
| `PUT` | `/api/v1/comparisons/{comparison_id}/turns/{turn_id}/preference` | Uložiť alebo aktualizovať používateľovu voľbu lepšieho chatu a vybranej odpovede pre dané kolo. | [Kontrakt](API.md#62-ulo%C5%BEenie-lep%C5%A1ieho-chatu) |
| `GET` | `/api/v1/comparisons/{comparison_id}/turns/{turn_id}/preference` | Načítať uloženú používateľovu voľbu lepšieho chatu a odpovede. | [Kontrakt](API.md#63-na%C4%8D%C3%ADtanie-vo%C4%BEby-lep%C5%A1ieho-chatu) |
| `GET` | `/api/v1/comparisons/{comparison_id}/usage` | Zobraziť priebežnú spotrebu tokenov a cenu vrátane prepočtu na 1 milión tokenov samostatne pre každý porovnávaný chat. | [Kontrakt](API.md#64-spotreba-porovn%C3%A1van%C3%BDch-konverz%C3%A1ci%C3%AD) |

## Navrhnuté interné endpointy

Tieto trasy zabezpečujú súkromné volania medzi službami. API Gateway smeruje verejné požiadavky cez `/api/v1`.

| HTTP a cesta | Vlastník | Volajúci | Účel |
| --- | --- | --- | --- |
| `GET /internal/v1/devices/{device_id}/hardware` | Runtime Service | Model Service | Aktuálne zdroje skutočného výpočtového zariadenia; verejný GET /api/v1/hardware ostáva fasádou Model Service. |
| `DELETE /internal/v1/runtime-models/{runtime_model_id}` | Runtime Service | Model Service | Uvoľniť a odstrániť modelové súbory po kontrole aktívnych použití a závislostí adaptérov. |
| `POST /internal/v1/files/resolve` | File Service | Chat Service, Evaluation Service | Dávkovo overiť prístup, typ a pripravenosť file_id a vrátiť metadáta bez presúvania binárneho obsahu cez broker. |
| `POST /internal/v1/generation-batches` | Chat Service | Evaluation Service | Idempotentne prijať viac generovaní s pevnými vstupmi, referenciami konverzácií a voliteľnými agent_snapshot_id; vrátiť generation_id pre každú položku. |
| `POST /internal/v1/agent-snapshots` | Agent Service | Chat Service, Evaluation Service | Vytvoriť nemenné snapshoty vybraných agentových inštrukcií pri prijatí generovania alebo spustení porovnania. |
| `GET /internal/v1/agent-snapshots/{snapshot_id}` | Agent Service | Chat Service | Načítať presne zachytené inštrukcie; neskoršia úprava profilu agenta nemení pripravený pokus. |

Interná dávka generovaní nesie ID vetvy a `agent_snapshot_id`. Snapshot zachová presné znenie inštrukcií pre dané kolo.

Asynchrónne príkazy, udalosti a ich smer sú uvedené v [architektúre](ARCHITEKTURA.md).
