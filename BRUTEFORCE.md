# Generovanie outfitov pomocou hrubej sily #

Hrubá sila je najzákladnejšia metóda riešenia problémov, ktorá využíva testovanie všetkých možných kombinácii pre nájdenie jedného či viacerých správnych riešenie. 

Náš program pre generovanie a navrhovanie outfitov pracuje na uživateľskej úrovni, veľké množstvo dát nie je predpokladané - priemerná žena vlastní cca 150 kúskov oblečenia vo svojom šatníku, preto je použitie hrubej sily jednou z uskutočniteľných možností. Použitie evolučného algoritmu sme zamietli, pretože pri nízkom počte dát je kontraproduktívne. Evolučný algoritmus by sa vyplatil napríklad v prípade, že by sme chceli uživateľom doporučovať, aký kúsok z eshopu si do šatníka dokúpiť, aby sa hodil k čo najväčšiemu počtu už vlastnených vecí. 

### Reprezentácia outfitu ###
Jednotlivé oblečenie je reprezentované pomocou vektoru [ID, kategória, sub-kategória, farba, potisk, materiál, štýl, sezónnosť]. 
Každý outfit je reprezentovaný k-ticou ‘kúskov’/vektorov. Počet k závisí od použitia týchto kúskov, napr. letný outfit, kde sú použité šaty má nižší počet kúskov oblečenia ako zimný outfit s tričkom, nohavicami a svetrom.
- Kategória: vršok 1. vrstva, vršok 2. vrstva, spodok, full-body oblečenie, topánky, kabelka, doplnky
- Sub-kategória: tielko, blúzka, rolák, mikina, sveter, bunda, džíny, šortky, sukňa, šaty, overal…
- Potisk: čistý, kvetinový, prúžkovaný, kockovaný, bodkovaný…
- Materiál: šifón, bavlna, denim, čipka, koža, satén, flaner, flitrované, úplet…
- Štýl: elegantné, plážové, casual, business…
- Sezónnosť: letné, zimné, celoročné, prechodné

### Návrh algoritmu ###
1. Generovanie všetkých možných kombinácií kusov oblečenia z uživateľovho virtuálneho šatníka na základe všeobecného pravidla: každá kategória sa môže vyskytovať iba raz, kategórie vršok 1. vrstva + spodok nemôžu byť použité naraz s kategóriou full-body oblečenie.
Toto je dobrý východiskový bod pre generovanie outfitov, pretože zaisťuje, že každý outfit je jedinečný a každý kúsok je použitý aspoň raz. 
Tak sa vyhneme problému, že pri pridaní nového kúsku do šatníka nebude algoritmus navrhovať kombinácie s jeho využitím iba preto, že nemá záznam o tom, ako veľmi ho uživateľ preferuje.
2. Ukladanie jednotlivých kombinácií = outfitov s každým kúskom reprezentovaným vektorom veľkosti 8 (dĺžka vyššie spomenutého vektoru) – tento prístup zabezpečuje, že každý outfit je uložený ako jedinečná kombinácia kúskov oblečenia. Dvojice (ID, fotka) si budeme pre rýchlejšiu prácu ukladať zvlášť. 
3. Okamžité generovanie nových kombinácií, keď používateľ nahrá nový predmet do virtuálneho šatníka zaisťuje, že šatník používateľa je vždy aktuálny.
4. Hodnotenie každého oblečenia na základe viacúčelovej fitness funkcie zabezpečuje, že každý outfit spĺňa určité kritériá, ako je celková kompatibilita (na vyhodnocovanie budeme používať model MMFashion), sezónnosť (budeme sa dívať na poslednú zložku vektoru jednotlivých použitých kúskov a či sa zhoduje s aktuálnou sezónou) a personalizácia.
5. Ponúknutie približne top 10 (ak je to možné) najlepšie ohodnotených kúskov uživateľovi. Aby sme zabezpečili rozmanitosť, každý kúsok oblečenia sa vo vybranej top 10 môže nachádzať maximálne dvakrát. V prípade, že to veľkosť uživateľovho šatníka nedovoľuje je toto pravidlo ignorované. 
6. Ak sa rozhodne uživateľ outfit úplne zamietnuť, kombinácia bude pridaná do zakázaného zoznamu, teda nebude možné ju už nikdy v budúcnosti ponúknuť. V prípade, že zoznam zakázaných kombinácii (ZZK) bude obsahovať viacero kombinácii, algoritmus bude hľadať najdlhší možný prienik (teda najdlhšiu spoločnú kombináciu). Vygenerovaným outfitom s kombináciou, ktorá sa v ZZK nachádza častejšie (napr. koženné nohavice a kvetinový sveter, plesové šaty a tenisky, tepláky a sako), bude znížená fitness funkcia. Takto zabezpečíme personalizáciu pri navrhovaní ďalších outfitov.
7. Zoznam obľúbených kombinácii tvoriť nebudeme. Chceme sa totiž vyhnúť prípadu, kedy sa jeden kúsok bude opakovať príliš často iba preto, že ho algoritmus bude brať ako populárny. Navyše uživateľ bude mať v aplikácii mmožnosť uložiť si outfity do zoznamu v prípade, že sa k nim bude chcieť vrátiť neskôr.
8. Jednou z možností ako algoritmus zefektívniť je, že budeme generovať všetky možné kombinácie iba niktorých kategórii, ako sú vršok 1. vrstva, vršok 2. vrstva, spodok, full-body oblečenie a v druhom kroku k nim hľadať vhodnú obuv a doplnky. 
