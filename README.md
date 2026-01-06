# Bratislavské cyklotrasy

Analýza správania cyklystov v Bratislave vrámci projektu pre 1-DAV-302/20 - Princípy dátovej vedy.

## 1. Výskumné otázky a cieľ projektu

Cieľom projektu je analyzovať správanie cyklistov v Bratislave na základe dát z cyklosčítačov a meteorologických meraní, identifikovať časové vzory využívania cyklotrás a preskúmať vplyv vybraných poveternostných podmienok na intenzitu cyklistickej dopravy.

### Výskumné otázky a hypotézy

**1. Líši sa vyťaženosť jednotlivých cyklotrás v Bratislave?**  
- H₀: Medzi jednotlivými cyklotrasami neexistujú významné rozdiely v priemernej dennej vyťaženosti.  
- H₁: Medzi jednotlivými cyklotrasami existujú významné rozdiely v priemernej dennej vyťaženosti.

**2. Existujú rozdiely v správaní cyklistov medzi pracovnými dňami a víkendmi?**  
- H₀: Priemerný počet cyklistov počas pracovných dní a víkendov je rovnaký.  
- H₁: Priemerný počet cyklistov sa medzi pracovnými dňami a víkendmi líši.

**3. Vykazuje cyklistická doprava v Bratislave sezónne správanie?**  
- H₀: Vyťaženosť cyklotrás je počas roka rovnomerná a bez výrazných sezónnych rozdielov.  
- H₁: Vyťaženosť cyklotrás sa medzi jednotlivými ročnými obdobiami významne líši.

**4. Ovplyvňujú zrážky počet cyklistov v Bratislave?**  
- H₀: Množstvo denných zrážok nemá vplyv na počet cyklistov.  
- H₁: Zvýšené množstvo denných zrážok vedie k poklesu počtu cyklistov.

**5. Existuje vzťah medzi priemernou dennou teplotou a počtom cyklistov?**  
- H₀: Medzi priemernou dennou teplotou a počtom cyklistov neexistuje vzťah.  
- H₁: Medzi priemernou dennou teplotou a počtom cyklistov existuje vzťah.

**6. Je možné na základe časových vzorov využiť dáta na rozlíšenie rôznych typov cyklotrás?**  
- H₀: Všetky cyklotrasy majú podobný časový profil využitia.  
- H₁: Cyklotrasy je možné rozdeliť do skupín s odlišným časovým profilom využitia.

**7. Existujú významné rozdiely v správaní cyklistov pred otvorením a po otvorení cyklocesty na Vajanského nábreží, ktoré sa uskutočnilo 6.9.2023?**
- H₀: Neexistujú významné rozdiely v správaní cyklistov pred otvorením a po otvorení cyklocesty na Vajanského nábreží.  
- H₁: Existujú významné rozdiely v správaní cyklistov pred otvorením a po otvorení cyklocesty na Vajanského nábreží.

## 2. Použité technológie

### Programovacie jazyky a knižnice

#### Jazyky

- **Python**

#### Knižnice

1. Získanie a spracovanie dát:
   - **requests**, **pandas**
2. Analýza dát:
   - ?
3. Vizualizácia dát:
   - ?

## 3. Získanie a spracovanie dát

### Dátové zdroje

#### 1. Dátový portál Hlavného mesta SR Bratislavy

- Dáta získané cez voľne dostupné API – kód na získanie dát sa nachádza v súbore **`cyklotrasy_data.ipynb`**
- Link k API dokumentácii: [data.Bratislava – API dokumentácia](https://data.bratislava.sk/pages/api-rozhranie)
- Link k online datasetu: [Cyklotrasy v BA – Dataset](https://data.bratislava.sk/datasets/097b7c8bc4b94560a7e34c9e64139a5e_0/explore)
- Autor: Hlavné mesto Slovenskej republiky Bratislava – Oddelenie dát, Sekcia digitalizácie
- Časové rozmedzie získaných údajov: **2014-07-18T07:00:00+00:00** – dva dni pred natiahnutím dát pomocou kódu, **23:00**
- Obsah dát:
  - **`nazov`** – názov cyklotrasy
  - **`zemepisna_sirka`** – súradnicový údaj o polohe snímača
  - **`zemepisna_dlzka`** – súradnicový údaj o polohe snímača
  - **`smer_do`** – smer, do ktorého snímač zaznamenáva cyklistov
  - **`smer_z`** – smer, z ktorého snímač zaznamenáva cyklistov
  - **`datum_a_cas`** – časový údaj záznamu
  - **`pocet_do`** – počet cyklistov zaznamenaných v smere **SMER_DO**
  - **`pocet_z`** – počet cyklistov zaznamenaných v smere **SMER_Z**

#### 2. Meteostat

- Dáta získané pomocou exportu z webstránky Meteostatu
- Link: [Počasie v BA](https://meteostat.net/en/place/sk/bratislava?s=11816&t=2023-01-01/2025-12-20)
- Autor: **Meteostat**
- Časové rozmedzie získaných údajov: **2023-01-01 – 2025-12-20**
- Obsah dát:
  - **`date`** – dátum, na ktorý sa predpoveď viaže
  - **`tavg`** – priemerná teplota vzduchu v stupňoch Celzia
  - **`tmin`** – minimálna teplota v stupňoch Celzia
  - **`tmax`** – maximálna teplota v stupňoch Celzia
  - **`prcp`** – celkové zrážky v milimetroch
  - **`snow`** – hĺbka snehu v milimetroch
  - **`wdir`** – smer vetra v stupňoch
  - **`wspd`** – priemerná rýchlosť vetra v kilometroch za hodinu
  - **`wpgt`** – najsilnejšie nárazy vetra v kilometroch za hodinu
  - **`pres`** – tlak vzduchu na hladine mora v hektopascaloch
  - **`tsun`** – celkové trvanie slnečného svitu v minútach

### Proces zberu dát

#### 1. Predpoveď počasia v BA

- Dáta sa exportujú zo stránky Meteostatu priamo do CSV súboru, ktorý bol premenovaný na **`pocasie_data_raw.csv`**
- Súbor obsahuje **1085 záznamov**
- Každý záznam reprezentuje jeden deň
- Dáta poskytujú informácie o počasí a poveternostných podmienkach v Bratislave od začiatku roku 2023 do **20.12.2025**

#### 2. Cyklotrasy v BA

Súbor **`cyklotrasy_data.ipynb`** extrahuje z data.Bratislava API endpointu dáta do súboru **`cyklotrasy_data_bratislava.csv`**.

**Technická implementácia:**
- **Request:**
  - používa sa HTTP GET s nastaveným parametrom **`resultRecordCount=1000`** – veľkosť batchu (maximum dovolené cez daný endpoint)
- **Stránkovanie (paginácia):**
  - v cykle sa nastavuje **`resultOffset`** na základe spracovaného počtu dát
  - z každej odpovede sa čítajú **`features`** a pridávajú do zoznamu **`all_records`**
  - cyklus sa ukončí, keď počet načítaných záznamov v poslednom batchi je menší než **`resultRecordCount`**
- **Transformácia:**
  - zoznam **`all_records`** sa transformuje na tabuľku pomocou **`pandas.json_normalize`**
  - výsledkom je DataFrame so stĺpcami z **`features`** (t. j. **`attributes.*`**)
- **Export:**
  - DataFrame sa uloží do CSV súboru **`Data/cyklotrasy_data_bratislava_raw.csv`**

### Čistenie dát

Cieľom čistenia a predspracovania dát bolo zjednotiť časový rozsah oboch datasetov a pripraviť jednotný dataset vhodný na ďalšiu analýzu správania cyklistov v Bratislave.

#### Kvalita dát

Prvým krokom bolo zmenšenie cyklo datasetu na časové rozhranie datasetu s počasím. Dáta z cyklosčítačov obsahovali časový údaj vo formáte UTC na hodinovej báze. Meteorologické dáta obsahovali denné záznamy s dátumom bez časovej zložky. Do dát o cyklosčítačoch bol teda pridaný stĺpec **`datetime`**, ktorý je nastavený na bratislavskú časovú zónu a bude sa napájať na dáta o počasí v správny deň. Po vyfiltrovaní údajov s nechcenými dátumami sa už mohla kontrolovať samotná kvalita dát.

Ako prvé sme sa pozreli na samotné názvy cyklotrias a ich smery, počas čoho sme zistili, že niektoré stĺpce obsahujú navyše charakter '\n'. Všetky textové stĺpce boli kvôli tomu zo strán očistené pomocou funkcie `strip()`.

Pri kontrole kvality cyklo dát sa ukázalo, že pre každý snímač chýbajú časové záznamy. Tieto medzery mali rôzne dĺžky, niektoré len jednu hodinu, niektoré takmer celý deň. Tento problém bol vyriešený neskôr v časri 'Predspracovanie'. Zvyšné dáta boli plne vyplnené a neobsahovali žiadne 'podozrivé' hodnoty, ktoré by sa mohli rovnať prázdnej alebo nulovej hodnote. Odchýlky v rámci jednotlivých stĺpcov boli adekvátne ich obsahu.

Pri kontrole dát o počasí sa ukázalo, že dáta neobsahujú žiadne údaje o hĺbke snehu ani o smere vetra. Stĺpcu **`tsun`** chýbala približne tretina záznamov a stĺpcu **`prcp`** jeden záznam. Štandardná odchýlka nenaznačovala prítomnosť žiadnych vyplnených 'chýbajúcich' hodnôt na štýl `-1`.

Celkovo boli oba zdroje dát vyhodnotené ako použiteľné a nedostatky boli adresované počas predspracovania.

#### Predspracovanie

V cyklo dátach boli podstatné stĺpce premenované na lepšie čitateľné a použiteľné názvy. Následne boli odstránené stĺpce **`attributes.ObjectId`** a **`attributes.DATUM_A_CAS`**, keďže jeden nebol potrebný pre ďalšie kroky a druhý bol nahradený stĺpcom **`datetime`**.

Problém chýbajúcich časových úsekov bol vyriešený tak, že sa pre každý snímač doplnili všetky hodinové záznamy od jeho prvého sčítania po posledné. Do týchto záznamov sa zapísali hodnoty pre statické stĺpce (napr. názov, smery a počasie). Ak bol chýbajúci úsek dlhý iba 1-2 hodiny, tak boli počty cyklistov doplnené pomocou interpolácie - vypočítajú sa na základe okolitých hodnôt. Takto vypočítané hodnoty dostali hodnotu `True` v stĺpci **`was_imputed`**. Ak bola medzera väčšia, tak zostali počty prázdne. Týmto sme zabezpečili kontinuitu dát pre vizualizáciu, ale neprehnali sme to s 'hádaním' chýbajúcich dát.

Ďalej boli vytvorené stĺpce, ktoré by mohli byť užitočné pre zodpovedanie výskumných otázok:
- **`pocet_total`** – súčet stĺpcov **`pocet_do`** a **`pocet_z`**
- **`date`** – čisto dátumová časť zo stĺpca **`datetime`**
- **`weekday`** – číselné hodnoty zodpovedajúce dňu v týždni (0 – pondelok, …, 6 – nedeľa)
- **`is_weekend`** – 1, ak je daný deň sobota alebo nedeľa, inak 0
- **`month`** – číselná hodnota mesiaca
- **`year`** – rok
- **`season`** – ročné obdobie

V dátach o počasí boli odstránené prázdne stĺpce **`snow`** a **`wdir`**. Pre chýbajúcu hodnotu v stĺpci **`prcp`** bolo rozhodnuté, že keďže sa jedná iba o jeden záznam, bude nahradená hodnotou 0.

Cyklistické a meteorologické dáta boli spojené na základe dátumu pomocou ľavého spojenia (left join), čím sa zachovali všetky cyklistické záznamy a k nim boli priradené príslušné meteorologické údaje. Po spojení sa overilo vyplnenie všetkých údajov, kde sa potvrdilo, že chýbajú iba údaje v stĺpcoch, kde to bolo vopred akceptované a očakávané.

#### Skladovanie dát

Výsledný predspracovaný dataset bol uložený do súboru **`final_data.csv`**, ktorý slúži ako vstup pre ďalšie časti projektu. Pôvodné surové dáta zostali zachované v samostatných súboroch.

## 4. Analýza a výsledky

## 5. Záver

**1. Líši sa vyťaženosť jednotlivých cyklotrás v Bratislave?**

**2. Existujú rozdiely v správaní cyklistov medzi pracovnými dňami a víkendmi?**

**3. Vykazuje cyklistická doprava v Bratislave sezónne správanie?**

**4. Ovplyvňujú zrážky počet cyklistov v Bratislave?**

**5. Existuje vzťah medzi priemernou dennou teplotou a počtom cyklistov?**

**6. Je možné na základe časových vzorov využiť dáta na rozlíšenie rôznych typov cyklotrás?**
