## Generovanie outfitov pomocou evolučného algoritmu ##
1. **Definovanie cieľov algoritmu** (čo chceme optimalizovať): 
- _personalizácia_ - ako veľmi vygenerovaný outfit zodpovedá uživateľovým preferenciám, 
- _rozmanitosť_ - či je outfit dostatočne rozmanitý, 
- _kompatibilita_ - či má outfit koherentný štýl a farebnosť (môžeme využiť O’Donovan-ov Color Compatibility Predictor), 
- _sezónnosť_ - či je outfit vhodný pre konkrétnu sezónu (ročné obdobie / počasie).
2. **Reprezentácia outfitu**: každý outfit musí byť reprezentovateľný nejakou postupnosťou ‘kúskov’. Jednotlivé oblečenie je reprezentované pomocou vektorov _[kategória, sub-kategória, farba, potisk, materiál, štýl, sezónnosť]_. Každý outfit je reprezentovaný maticou, ktorá sa skladá z týchto vektorov. Každá kategória sa môže vyskytovať iba raz.
- Kategória: vršok 1. vrstva, vršok 2. vrstva, spodok, full-body oblečenie, kabelka, doplnky
- Sub-kategória: tielko, blúzka, rolák, mikina, sveter, bunda, džíny, šortky, sukňa, šaty, overal…
- Potisk: čistý, kvetinový, prúžkovaný, kockovaný, bodkovaný…
- Materiál: šifón, bavlna, denim, čipka, koža, satén, flaner, flitrované, úplet…
- Štýl: elegantné, plážové, casual, business…
- Sezónnosť: letné, zimné, celoročné, prechodné
3. **Definovanie fitness funkcie**: fitness funkcia vyhodnocuje kvalitu každého outfitu na základe definovaných cieľov. V tomto prípade by to bol napríklad priemer jednotlivých cieľových ohodnotení.
4. **Generovanie prvej populácie** (rodičov): prebieha náhodne, avšak je obmedzená heuristikou, napr. kategória sa vyskytuje iba raz, sezónnosť je prepojená s počasím/dátumom, zakázané kombinácie (koženné nohavice a kvetinový sveter, plesové šaty a tenisky, tepláky a sako). Následne je vypočítaná ‘fitness’ každého vygenerovaného outfitu. Uživateľovi je následne ponúknutý určitý počet (napr. 10) outfitov s najväčším fitness.
5. **Ohodnotenie a vznik potomkov**: na základe uživateľovho ohodnotenia sa upraví fitness outfitu (pretože nie každý outfit, ktorého fitness bola vysoká, sa bude aj reálne uživateľovi páčiť). Vyselektujú sa outfity s najvyšším hodnotením a aplikujú sa genetické operátory - kríženie a mutácia, ktorej cieľom je zlepšenie rôznorodosti. Vznikne nová generácia, ktorá bude uživateľovi znova ponúknutá. Generovanie novej generácie sa opakuje po obmedzenom počte uživateľom ohodnotených outfitov (1 pre úspešný outfit, 0 pre neúspešný outfit)

#### O’Donovan: Color Compatibility From Large Datasets ####

## Získavanie atribútov z fotografií uživateľa ##
#### MMFashion: An Open-Source Toolbox for Visual Fashion Analysis ####

#### DeepFashion: Powering Robust Clothes Recognition and Retrieval with Rich Annotations ####

