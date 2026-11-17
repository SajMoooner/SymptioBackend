# Funkčné požiadavky – Zadanie 2

[Späť na zadanie](README.md) · [Endpointy](ENDPOINTY.md)

Identifikátory zodpovedajú spoločnému návrhu. FR-018 pokrýva použitie základného stiahnutého modelu cez API. Evaluácia agentových inštrukcií podľa FR-032 je v [zadaní 3](../Zadanie%203%20-%20Modely%2C%20Pou%C5%BE%C3%ADvanie%2C%20Evalu%C3%A1cia%20agentov/FUNKCNE_POZIADAVKY.md).

## Stiahnutie a správa LLM modelov

| ID | Čo sa od systému požaduje | Akceptačné kritériá |
| --- | --- | --- |
| FR-001 | Umožniť stiahnutie LLM modelu z Hugging Face alebo Ollama podľa výkonu zariadenia. | Systém zohľadní výkon zariadenia pri ponuke modelov. Používateľ môže vybrať ponúkaný model z Hugging Face alebo Ollama a úspešne ho stiahnuť. |
| FR-002 | Ponúknuť dostupné varianty modelu podľa kompresie. | Pri modeli sú zobrazené dostupné varianty kompresie. Po výbere sa stiahne práve zvolený variant. |
| FR-003 | Zablokovať stiahnutie variantu, ktorý na zariadení nespĺňa podmienky použiteľnosti. | Ak systém vyhodnotí výkon zariadenia ako nedostatočný pre daný model, stiahnutie zablokuje a používateľovi zobrazí dôvod. |
| FR-004 | Spravovať stiahnuté modely v knižnici vrátane stiahnutia a odstránenia. | Úspešne stiahnutý model sa zobrazí v knižnici. Používateľ môže model odstrániť a po odstránení už nie je dostupný medzi stiahnutými modelmi. |

## Používanie LLM modelov

| ID | Čo sa od systému požaduje | Akceptačné kritériá |
| --- | --- | --- |
| FR-018 | Sprístupniť použitie stiahnutého základného modelu cez API Evalva. | Cez API možno odoslať vstup vybranému stiahnutému modelu. Odpoveď je priradená k modelu a príslušnej požiadavke. |
| FR-019 | Vytvoriť konverzáciu s vybraným modelom z knižnice. | Používateľ môže vybrať model z knižnice a vytvoriť konverzáciu. Následné správy sa odosielajú zvolenému modelu. |
| FR-020 | Umožniť výmenu správ s modelom a uchovať úplnú históriu konverzácie. | Používateľ môže opakovane odosielať správy a prijímať odpovede. Po opätovnom otvorení konverzácie sú všetky uložené správy a odpovede dostupné v správnom poradí. |
| FR-021 | Vrátiť výsledok alebo konkrétnu chybu každej prijatej požiadavky. | Každá prijatá požiadavka má po ukončení výsledok alebo konkrétnu chybu. Zlyhaná požiadavka nezostane zobrazená ako úspešná odpoveď. |
| FR-022 | Umožniť pridať k textovému vstupu obrázok pri podporovaných modeloch. | Používateľ môže pridať k textu obrázok. Pri modeli podporujúcom obrazové vstupy sa odošle text aj priložený obrázok; nepodporovaný vstup má zobrazenú chybu. |
| FR-023 | Sledovať generovanie až do celej odpovede alebo konečnej chyby. | Počas generovania je požiadavka označená ako spracúvaná. Za dokončenú sa označí až po prijatí celej odpovede alebo konečnej chyby. |
| FR-024 | Použiť pri odpovedi kontext danej konverzácie. | Kontrola vstupu odoslaného modelu pri ďalšej správe potvrdí prítomnosť predchádzajúcich správ a odpovedí danej konverzácie v správnom poradí. |
| FR-025 | Priebežne počítať spotrebu a cenu prepočítanú na milión tokenov v každej konverzácii. | Po každej odpovedi sa aktualizujú tokeny konverzácie. Pri dostupnom cenovom podklade sa zobrazí prepočet na milión tokenov. Bez podkladu má cena stav neznáma. |

## Evaluácia podľa úlohy

| ID | Čo sa od systému požaduje | Akceptačné kritériá |
| --- | --- | --- |
| FR-026 | Vytvoriť a uložiť evaluáciu pre zvolenú úlohu. | Používateľ môže vytvoriť samostatnú evaluáciu, ktorá sa uloží a možno ju následne otvoriť a priradiť ku konverzáciám. |
| FR-027 | Vytvoriť samostatné konverzácie s rôznymi modelmi a rovnakým vstupom. | Pre vybrané modely vzniknú samostatné konverzácie s rovnakým vstupom. Odpovede zostanú oddelené a priradené k príslušným modelom. |
| FR-028 | Priradiť viaceré konverzácie k jednej evaluácii. | K jednej evaluácii možno priradiť viac konverzácií. Po jej opätovnom otvorení zostanú všetky priradenia zachované. |
| FR-029 | Uložiť testovací vstup spolu s očakávaným výstupom. | Ku každému testovaciemu vstupu možno uložiť očakávaný výstup. Pri načítaní evaluácie sú oba údaje dostupné a správne spárované. |
| FR-030 | Po ukončení spracovania porovnať odpoveď s očakávaným výstupom. | Vyhodnotenie prebehne po ukončení priradených konverzácií. Pre každú získanú odpoveď sa zobrazí skutočný výstup, očakávaný výstup a výsledok ich porovnania. |
| FR-031 | Importovať testovacie vstupy a očakávané výstupy zo súboru CSV so stĺpcami `input` a `výsledok`. | Platné CSV s poľami input a výsledok vytvorí testovacie prípady zo všetkých dátových riadkov. Každý vstup sa vyhodnotí voči výsledku z rovnakého riadku; neplatný formát má zobrazenú chybu. |
| FR-033 | Porovnať spotrebu tokenov a cenu za rovnaký vstup alebo sadu vstupov. | Pre každý model a porovnateľný vstup alebo sadu sú dostupné tokeny a náklady, ak existuje cenový podklad. Neznáma cena je označená pri príslušnom modeli. |
