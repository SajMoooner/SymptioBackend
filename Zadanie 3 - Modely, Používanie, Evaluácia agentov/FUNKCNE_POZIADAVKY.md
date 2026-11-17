# Funkčné požiadavky – Zadanie 3

[Späť na zadanie](README.md) · [Endpointy](ENDPOINTY.md)

Identifikátory zodpovedajú spoločnému návrhu. FR-018 je tu obmedzená na stiahnuté základné modely. FR-032 pokrýva porovnanie modelov s agentovými inštrukciami.

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

## Agentová evaluácia

| ID | Čo sa od systému požaduje | Akceptačné kritériá |
| --- | --- | --- |
| FR-032 | Vyhodnotiť viaceré modely s agentovými inštrukciami a porovnať ich výsledky. | Agentovú evaluáciu možno spustiť nad viacerými zvolenými modelmi. Výsledky sú dostupné vedľa seba s označením modelu a použitých agentových inštrukcií. |

## Evaluácia agentových prístupov

| ID | Čo sa od systému požaduje | Akceptačné kritériá |
| --- | --- | --- |
| FR-034 | Vytvárať a ukladať agentov s inštrukciami a priradeným modelom. | Používateľ môže vytvoriť viac agentov s odlišnými inštrukciami a priradiť im model. Po uložení a opätovnom otvorení sa zobrazia správne inštrukcie a priradenie. |
| FR-035 | Použiť uloženého agenta vo vybranej konverzácii. | Vo vybranej konverzácii možno zvoliť uloženého agenta. Systém načíta obsah práve tohto agenta a sprístupní ho pre danú konverzáciu. |
| FR-036 | Vytvoriť porovnanie s viacerými modelmi a oddelenými konverzáciami. | V rámci porovnávacej konverzácie možno vybrať viac modelov. Ich odpovede na spoločný vstup sa zobrazujú oddelene s označením modelu. |
| FR-037 | Odoslať vstup vo forme textu alebo obrázka s odkazom na uloženého agenta. | Vstup umožňuje pridať text alebo podporovaný obrázok spolu s odkazom na uloženého agenta. Systém správne priradí vstup aj odkaz k danej požiadavke. |
| FR-038 | Pred generovaním načítať uložené inštrukcie agenta a vložiť ich do promptu. | Pred odoslaním požiadavky sa odkaz na agenta vyhodnotí a jeho inštrukcie sa zahrnú do promptu. Kontrola skutočne odoslaného vstupu potvrdí prítomnosť obsahu vybraného agenta. |
| FR-039 | Po dokončení porovnania umožniť označiť lepší chat. | Po dokončení porovnávaných odpovedí systém ponúkne používateľovi možnosť označiť lepší chat. |
| FR-040 | Uložiť používateľovu voľbu lepšieho chatu a vybranej odpovede. | Vybraná odpoveď a označený lepší chat sa uložia k porovnaniu. Po opätovnom otvorení je viditeľná rovnaká používateľova voľba. |
| FR-041 | Zobraziť spotrebu a cenu za milión tokenov samostatne pre každý porovnávaný chat. | Po odpovedi modelu sa aktualizuje spotreba jeho vetvy. Vetva má vlastný prepočet ceny na milión tokenov, ak má cenový podklad. Bez podkladu má cena stav neznáma. |
