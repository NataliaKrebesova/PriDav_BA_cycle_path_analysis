# Bratislavské cyklotrasy

Analýza správania cyklystov v Bratislave vrámci projektu pre 1-DAV-302/20 - Princípy dátovej vedy.

## 1. Výskumné otázky a cieľ projektu

Cieľom projektu je analyzovať správanie cyklistov v Bratislave na základe dát z cyklosčítačov a meteorologických meraní, identifikovať časové vzory využívania cyklotrás a preskúmať vplyv vybraných poveternostných podmienok na intenzitu cyklistickej dopravy.

### Výskumné otázky a hypotézy

**1. Líši sa denná vyťaženosť jednotlivých cyklotrás v Bratislave?**  
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

**1. Líši sa denná vyťaženosť jednotlivých cyklotrás v Bratislave?**

Relevantný kód je v súboroch `denne_cyklo.ipynb` a `hypothesis1.ipynb`. Pred vykonaním testu sme dáta pre každú cyklotrasu najprv upravili na spoločný dátum, odstránili neúplné záznamy, zlúčili niektoré trasy, atď., detaily sú v súbore `denne_cyklo.ipynb`.

Hypotézy:

H₀: Medzi jednotlivými cyklotrasami neexistujú významné rozdiely v priemernej dennej vyťaženosti. (Očakávaný počet cyklistov za deň je rovnaký pre všetky cyklotrasy, parameter trasa nemá významný vplyv na strednú hodnotu.)

H₁: Medzi jednotlivými cyklotrasami existujú významné rozdiely v priemernej dennej vyťaženosti. (Očakávaný počet cyklistov za deň sa líši medzi aspoň dvoma cyklotrasami.)

### Voľba štatistického modelu pre analýzu dennej vyťaženosti cyklotrás

Výber vhodného štatistického modelu je dôležitý pre validitu záverov výskumu. V našej analýze pracujeme s dennými počtami cyklistov (daily_total) na jednotlivých trasách, čo je diskrétna nezáporná celočíselná premenná reprezentujúca počet udalostí (prejazdov bicyklov) za časovú jednotku (deň). Tento typ dát vylučuje použitie klasických metód predpokladajúcich spojitú a normálne rozdelenú závislú premennú.

Metódy ako ANOVA (analýza rozptylu) alebo lineárna regresia predpokladajú: normálne rozdelenie rezíduí, konštantný rozptyl a spojitosť závislej premennej.

Tieto predpoklady sú pri počtových dátach systematicky porušované:

- distribučná asymetria: počty sú často pravostranne šikmé s veľkým podielom nízych hodnôt,

- závislosť rozptylu na priemere: v počtových procesoch zvyčajne platí, že vyšší priemer znamená vyšší rozptyl,

- rozptyl môže výrazne prevyšovať priemer (tzv. overdispersion https://en.wikipedia.org/wiki/Overdispersion),

- diskrétnosť: hodnoty sú celé čísla, nie spojité veličiny.

Použitie nevhodných metód môže viesť k závažným chybám: skresleným odhadom, nesprávnym štandardným chybám a falošne významným p-hodnotám (zvýšená chyba I. typu).

Pre takéto typy dát je vhodné použiť generalizované lineárne modely (GLM, https://en.wikipedia.org/wiki/Generalized_linear_model) s distribúciou priamo určenou pre počty. Základný model pre analýzu počtov je Poissonova regresia, ktorá predpokladá, že závislá premenná pochádza z Poissonovho rozdelenia. Takéto modelovanie zabezpečuje, že predikované hodnoty sú vždy nezáporné. Model priamo odhaduje očakávanú dennú frekvenciu cyklistov pre každú trasu, čo zodpovedá nášmu výskumnému cieľu porovnať priemernú dennú vyťaženosť trás.

V praxi však niekedy pozorujeme, že variabilita pozorovaných denných počtov cyklistov prevyšuje priemer (tzv. overdispersion). V takýchto prípadoch Poissonov model podceňuje chyby odhadov a môže viesť k nesprávnym záverom (napr. falošne významné rozdiely). Preto sa ako alternatíva používa negatívna binomická regresia, ktorá obsahuje ďalší parameter rozptylu a umožňuje modelovať situácie, kde variancia presahuje priemer, čím poskytuje spoľahlivejšie odhady a testovanie hypotéz. (https://link.springer.com/article/10.1186/s12966-023-01460-y, https://stats.oarc.ucla.edu/stata/dae/negative-binomial-regression/)

Test sa vykoná pomocou likelihood ratio test, ktorý porovnáva model s trasami voči nulovému modelu. V analýze sme postupovali nasledovne:

Najprv sme spravili Poissonov model, ktorý slúžil ako diagnostický nástroj na detekciu overdispersion.
Disperziu sme vypočítali ako pomer Pearson chi squared a residual degrees of freedom (https://www.askpython.com/python-modules/statsmodel/statsmodels-generalized-linear-models).
Pomer vyšiel výrazne väčší ako 1, preto sme spravili aj negatívny binomický model, lebo v prípade overdispersion Poissonov model podceňuje štandardné chyby, čo vedie k príliš úzkym intervalom spoľahlivosti a zvýšenému riziku falošnej významnosti. (https://www.reddit.com/r/statistics/comments/ttgrxl/q_why_does_overdispersion_make_coefficient/)

Použitím negatívnej binomickej regresie rozšírime Poissonov model o dodatočný parameter rozptylu, čo umožňuje rozptylu rásť nezávisle od priemeru.

Kľúčovým predpokladom Poissonovej regresie je, že rozptyl odpovedajúcej premennej sa rovná jej priemeru. Na posúdenie tohto predpokladu sme vypočítali pomer rozptylu k priemeru pre denný počet cyklistov na každej trase. Pre všetky trasy rozptyl denných počtov výrazne prekročil priemer (pomer rozptylu k priemeru > 50), čo naznačuje podstatnú nadmernú disperziu v porovnaní s Poissonovým predpokladom. Taký veľký pomer disperzie naznačuje, že Poissonov model by podhodnocoval skutočnú variabilitu a viedol by k skresleným štandardným chybám a nespoľahlivým p-hodnotám. Preto sme sa rozhodli použiť aj negatívnu binomickú regresiu, ktorá je zovšeobecnením Poissonovej regresie, ktorá pridáva parameter disperzie, aby vhodnejšie modelovala vyššiu variabilitu v údajoch a poskytovala lepšie štatistické závery.

Na posúdenie rozdielov v očakávanom dennom počte cyklistov na jednotlivých trasách sme najprv použili Poissonov model. Diagnostika tohto modelu odhalila extrémnu overdisperziu (280.99), čo znamená, že variabilita v dátach výrazne prevyšovala variabilitu predpokladanú Poissonovým rozdelením, kde stredná hodnota = rozptyl. V takomto prípade Poissonov model podceňuje štandardné chyby odhadov, čo vedie k nespoľahlivým štatistickým testom a zvýšenému riziku falošnej významnosti (chyba I. typu).

Preto sme ako vhodnejší prístup použili negatívnu binomickú regresiu. Tento model zavádza dodatočný parameter rozptylu, ktorý explicitne modeluje nadmernú variabilitu (overdisperziu). Po jeho aplikácii sa disperzia znížila na 0.57, čo potvrdilo, že negatívny binomický model adekvátne zachytáva variabilitu v našich dátach a je pre ďalšiu inferenciu štatisticky správnou voľbou.


Koeficienty oboch modelov sa interpretujú rovnako: predstavujú rozdiel v logaritme očakávaného denného počtu (log(počet)) medzi danou trasou a referenčnou trasou. Ich exponenciála (exp(koeficient)) má multiplikatívny efekt, teda koľkokrát je očakávaný počet na trase vyšší alebo nižší v porovnaní s referenčnou trasou. V našej analýze mal približný denný počet na referenčnej trase hodnotu 635 cyklistov.

Príklad interpretácie: Ak model pre konkrétnu trasu odhadne koeficient 0.97, znamená to, že log(očakávaný_počet) je 0.97 krát vyšší ako na referenčnej trase. Prevod exp(0.97) = 2.64 znamená, že očakávaný denný počet na tejto trase je približne 2.64-násobkom počtu na referenčnej trase, čo predstavuje približne 635 * 2.64 = 1676 cyklistov za deň. Záporný koeficient naopak značí nižšiu vyťaženosť v porovnaní s referenčnou trasou.

Pri porovnaní modelov sme pozorovali, že bodové odhady koeficientov (tj. samotné hodnoty efektov trás) boli v Poissonovom aj negatívnom binomickom modeli veľmi podobné. To je očakávané, pretože oba modely cielené odhadujú rovnakú strednú hodnotu (priemerný denný počet).

Kľúčový a rozhodujúci rozdiel medzi modelmi sa týka istoty týchto odhadov, ktorú vyjadrujú štandardné chyby. Poissonov model, ktorý nútene predpokladá nízku variabilitu, produkuje umelo nízke štandardné chyby. To vedie k extrémne vysokým absolútnym hodnotám testových štatistík (z-hodnôt) a následne k neprimerane nízkym p-hodnotám, ktoré by mohli nesprávne naznačovať štatistickú významnosť. Negatívny binomický model, ktorý korektne zohľadňuje overdisperziu prítomnú v dátach, poskytuje realisticky vyššie a spoľahlivejšie štandardné chyby. V dôsledku toho sú z-hodnoty, p-hodnoty a intervaly spoľahlivosti z tohto modelu spoľahlivejšie a tvoria základ pre naše závery o rozdieloch medzi trasami.

### Porovnanie s nulovým modelom

Na formálne overenie štatistickej významnosti rozdielov medzi trasami sme vykonali LRT (Likelihood Ratio Test). Táto metóda porovnáva negatívny binomický model, ktorý obsahuje trasu ako kategorickú premennú (daily_total ~ nazov), s jednoduchým nulovým modelom (daily_total ~ 1), ktorý predpokladá, že všetky trasy majú rovnakú priemernú dennú vyťaženosť. Logika testu spočíva v posúdení, či komplexnejší model prináša zlepšenie v schopnosti vysvetliť variabilitu v dátach.

LRT sme vypočítali ako dvojnásobok rozdielu medzi logaritmami vierohodnosti (log-likelihood) oboch modelov: LR = 2 * (llf_nb_model - llf_null_nb_model). Logaritmus vierohodnosti (llf) je mierou toho, ako dobre konkrétny model vysvetľuje pozorované dáta; vyššia hodnota znamená lepší výsledok. Podľa Wilksovej vety (https://en.wikipedia.org/wiki/Wilks%27_theorem) má táto testovacia štatistika, za platnosti nulovej hypotézy a pri dostatočne veľkej vzorke, asymptoticky chí-kvadrát rozdelenie s počtom stupňov voľnosti rovným rozdielu v počte parametrov medzi modelmi.

Výsledok testu bol jednoznačný, hodnota LRT štatistiky dosiahla 1844.78 pri 17 stupňoch voľnosti (čo zodpovedá 17 nezávislým porovnaniam medzi 18 trasami). Zodpovedajúca p-hodnota bola menšia ako 0.001, v praxi v podstate nulová. Tento výsledok prevyšuje kritickú hodnotu pre štandardnú hladinu významnosti 0.05 a poskytuje dôvod na zamietnutie nulovej hypotézy. Zahrnutie premennej trasa do modelu poskytuje štatisticky významne lepšie vysvetlenie variabilít v denných počtoch cyklistov. Existujú štatisticky významné rozdiely v priemernej dennej vyťaženosti medzi analyzovanými cyklotrasami, čo potvrdzuje našu alternatívnu hypotézu H1.

Výsledky štatistického testu sú pozorovateľné aj graficky, na grafe vidíme veľké rozdiely v priemernom dennom počte cyklistov. Najfrekventovanejšia trasa Dolnozemská má mnohonásobne vyšší priemerný denný počet cyklistov ako najmenej frekventovaná Devínska cesta.

![Daily average cyclist count by route](Images/daily_avg_count.png)

**2. Existujú rozdiely v správaní cyklistov medzi pracovnými dňami a víkendmi?**

**3. Vykazuje cyklistická doprava v Bratislave sezónne správanie?**

**4. Ovplyvňujú zrážky počet cyklistov v Bratislave?**

**5. Existuje vzťah medzi priemernou dennou teplotou a počtom cyklistov?**

**6. Je možné na základe časových vzorov využiť dáta na rozlíšenie rôznych typov cyklotrás?**
