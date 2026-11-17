# Funkčné požiadavky – Zadanie 1

[Späť na zadanie](README.md) · [Endpointy](ENDPOINTY.md)

Rozsah tvoria prvé štyri oblasti požiadaviek Evalva. Identifikátory zodpovedajú spoločnému návrhu.

## Stiahnutie a správa LLM modelov

| ID | Čo sa od systému požaduje | Akceptačné kritériá |
| --- | --- | --- |
| FR-001 | Umožniť stiahnutie LLM modelu z Hugging Face alebo Ollama podľa výkonu zariadenia. | Systém zohľadní výkon zariadenia pri ponuke modelov. Používateľ môže vybrať ponúkaný model z Hugging Face alebo Ollama a úspešne ho stiahnuť. |
| FR-002 | Ponúknuť dostupné varianty modelu podľa kompresie. | Pri modeli sú zobrazené dostupné varianty kompresie. Po výbere sa stiahne práve zvolený variant. |
| FR-003 | Zablokovať stiahnutie variantu, ktorý na zariadení nespĺňa podmienky použiteľnosti. | Ak systém vyhodnotí výkon zariadenia ako nedostatočný pre daný model, stiahnutie zablokuje a používateľovi zobrazí dôvod. |
| FR-004 | Spravovať stiahnuté modely v knižnici vrátane stiahnutia a odstránenia. | Úspešne stiahnutý model sa zobrazí v knižnici. Používateľ môže model odstrániť a po odstránení už nie je dostupný medzi stiahnutými modelmi. |

## Fine-tuning LLM modelov

| ID | Čo sa od systému požaduje | Akceptačné kritériá |
| --- | --- | --- |
| FR-005 | Umožniť fine-tuning podporovaného stiahnutého modelu. | Používateľ môže vybrať stiahnutý model podporujúci fine-tuning a spustiť nad ním tréning s vloženými dátami. |
| FR-006 | Umožniť nastaviť parametre fine-tuningu. | Používateľ môže pred spustením zmeniť dostupné tréningové parametre. Spustený proces používa zvolené hodnoty. |
| FR-007 | Prijať textové tréningové dáta v definovanom formáte JSON. | Systém prijme platný JSON v určenom tréningovom formáte a načíta jeho príklady do tréningu. Neplatný formát odmietne s vysvetlením chyby. |
| FR-008 | Prijať JPG súbory na fine-tuning podporovaného obrazového modelu. | Používateľ môže pridať JPG súbory do obrazového tréningu a spustiť fine-tuning modelu podporujúceho obrazové vstupy. Vložené súbory sú súčasťou tréningových dát. |
| FR-009 | Zobrazovať priebežný stav a výsledky fine-tuningu. | Počas tréningu sú dostupné aktuálny stav a priebežne aktualizované výsledky procesu. Zobrazené údaje patria k vybranému tréningu. |
| FR-010 | Zobraziť výsledok ukončeného fine-tuningu. | Po ukončení sa zobrazí výsledok a stav dokončenia tréningu. Pri úspešnom dokončení je dostupný výsledný model; pri zlyhaní sa zobrazí chyba. |
| FR-011 | Umožniť zastavenie prebiehajúceho fine-tuningu. | Používateľ môže zastaviť bežiaci tréning. Proces sa ukončí a jeho stav sa zobrazí ako zastavený. |
| FR-012 | Po úspešnom fine-tuningu uložiť model do knižnice. | Po úspešnom tréningu sa výsledný model zobrazí v knižnici a možno ho vybrať na následné používanie. |

## Pripojenie API poskytovateľov

| ID | Čo sa od systému požaduje | Akceptačné kritériá |
| --- | --- | --- |
| FR-013 | Umožniť pripojenie k API poskytovateľov, napríklad OpenAI a Anthropic pre modely Claude. | Po zadaní platných prístupových údajov možno nadviazať spojenie s OpenAI alebo Anthropic. Neúspešný pokus vráti chybu pripojenia. |
| FR-014 | Umožniť vybrať API model a podporovanú úroveň premýšľania. | Používateľ môže vybrať dostupný typ modelu a podporovanú úroveň premýšľania. Pri volaní sa použije zvolený model a nastavenie. |
| FR-015 | Umožniť textovú komunikáciu a použitie podporovaných nástrojov API modelu. | Vybranému modelu možno odoslať text a používať jeho podporované nástroje. Textová odpoveď aj výsledok nástroja sú dostupné v komunikácii. |
| FR-016 | Zobraziť cenník vybraného API modelu za milión tokenov. | Pri vybranom modeli je dostupná cena za 1 milión tokenov vrátane meny a rozlíšenia sadzieb, ak poskytovateľ účtuje vstup a výstup odlišne. |

## Používanie LLM modelov

| ID | Čo sa od systému požaduje | Akceptačné kritériá |
| --- | --- | --- |
| FR-017 | Volateľným API sprístupniť odpoveď alebo chybu vybraného poskytovateľa. | Platná požiadavka sa odošle API vybraného poskytovateľa a odpoveď sa vráti používateľovi. Chyba poskytovateľa sa zobrazí ako chyba danej požiadavky. |
| FR-018 | Sprístupniť použitie základných aj fine-tunovaných modelov cez API Evalva. | Cez API možno odoslať vstup bežnému aj fine-tunovanému modelu. Odpoveď pochádza zo zvoleného modelu a je priradená k príslušnej požiadavke. |
| FR-019 | Vytvoriť konverzáciu s vybraným modelom z knižnice. | Používateľ môže vybrať model z knižnice a vytvoriť konverzáciu. Následné správy sa odosielajú zvolenému modelu. |
| FR-020 | Umožniť výmenu správ s modelom a uchovať úplnú históriu konverzácie. | Používateľ môže opakovane odosielať správy a prijímať odpovede. Po opätovnom otvorení konverzácie sú všetky uložené správy a odpovede dostupné v správnom poradí. |
| FR-021 | Vrátiť výsledok alebo konkrétnu chybu každej prijatej požiadavky. | Každá prijatá požiadavka má po ukončení výsledok alebo konkrétnu chybu. Zlyhaná požiadavka nezostane zobrazená ako úspešná odpoveď. |
| FR-022 | Umožniť pridať k textovému vstupu obrázok pri podporovaných modeloch. | Používateľ môže pridať k textu obrázok. Pri modeli podporujúcom obrazové vstupy sa odošle text aj priložený obrázok; nepodporovaný vstup má zobrazenú chybu. |
| FR-023 | Sledovať generovanie až do celej odpovede alebo konečnej chyby. | Počas generovania je požiadavka označená ako spracúvaná. Za dokončenú sa označí až po prijatí celej odpovede alebo konečnej chyby. |
| FR-024 | Použiť pri odpovedi kontext danej konverzácie. | Kontrola vstupu odoslaného modelu pri ďalšej správe potvrdí prítomnosť predchádzajúcich správ a odpovedí danej konverzácie v správnom poradí. |
| FR-025 | Priebežne počítať spotrebu a cenu prepočítanú na milión tokenov v každej konverzácii. | Konverzácia zobrazuje použitie tokenov a cenu prepočítanú na 1 milión tokenov podľa cenníka použitého modelu. Údaje sa aktualizujú po ďalšej odpovedi. |
