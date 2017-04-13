<properties 
   pageTitle="Műszaki jellemzői StorSimple |} Microsoft Azure"
   description="Műszaki és szabályozási szabványokat megfelelőségi információk a StorSimple hardver-összetevők ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Műszaki és megfelelőség az StorSimple eszköz

## <a name="overview"></a>– Áttekintés

A műszaki, és a cikkben ismertetett szabályozási szabványokat igazodjon a Microsoft Azure StorSimple eszköz hardver összetevői. Műszaki Power és hűtési modulok (PCMs), lemezmeghajtók, tárolókapacitással rendelkezik, és letölthetés mutatják be. Megfelelőségi információk többek nemzetközi szabványok, a biztonság és a kibocsátás és a kábelek foglalkozik.


## <a name="power-and-cooling-module-specifications"></a>A Power és hűtési modul alkalmazásra vonatkozó technikai adatok  

Az eszköznek StorSimple két 100-240V kettős legyező SBB-kompatibilis Power hűtési modulok (PCMs). Ezzel a megoldással a felesleges power konfiguráció. Ha nem sikerül egy PCM, az eszköz mindaddig, amíg a sikertelen modul helyettesíti működik a többi PCM továbbra is.  

A EBOD ház egy 580 W PCM használ, és elsődleges ház egy 764 W PCM használ. Az alábbi táblázatokban felsoroljuk a PCMs társított műszaki előírások.

| Meghatározása           | 580 W PCM (EBOD)                                    | 764 W PCM (elsődleges)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Maximális kimenő teljesítménye    | 580 W                                               | 764                                                |
| Gyakoriság               | 50/60 Hz                                            | 50/60 Hz                                           |
| Feszültség kijelölt tartománnyal | Automatikus alaptartományban: 90 – 264 V AC, 47/63 Hz               | Automatikus alaptartományban: 47/63 Hz AC, V 90-264               |
| Aktuális maximális behatolása  | 20 A                                                | 20 A                                               |
| A Power tényező javítása | > névleges beviteli feszültség 95 %                          | > névleges beviteli feszültség 95 %                         |
| Harmonikus               | Megfelel a EN61000-3-2                                   | Megfelel a EN61000-3-2                                  |
| Kimenet                  | 5 v készenléti feszültség @ 2.0-s verziója A                          | 5 v készenléti feszültség @ 2.7 A                         |
|                         | + 5 v @ 42 A                                          | + 5 v @ 40 A                                         |
|                         | + 12 @ 38 A                                         | + 12 @ 38 A                                        |
| Beépíthető meleg           |  igen                                                | igen                                                |
| Kapcsolók és LED       | AC kikapcsolás kapcsoló és a négy állapotjelző LED     | AC kikapcsolás kapcsoló és hat állapotjelző LED     |
| Hűtési ház       | Tengelyirányú hűtési ventillátortól a változó ventilátor sebesség vezérlő  | Tengelyirányú hűtési ventillátortól a változó ventilátor sebesség vezérlő |

 
## <a name="power-consumption-statistics"></a>A Power felhasználási statisztikát  

Az alábbi táblázat a szokásos power felhasználási adatok (tényleges értékek eltérőek lehetnek a közzétett) StorSimple eszköz a különböző modellekhez. 
 
 Feltételek | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Ventillátortól lassú, meghajtók üresjárati | 1.45 A  |0.31 kW | 1057.76 BTU/hr | 3.19. A | 0.34 kW | 1160.13 BTU/hr 
 Ventillátortól lassú, meghajtók elérése | 1.54 A | 0.33 kW | 1126.01 BTU/hr | 3.27 A | 0.36 kW | 1228.37 BTU/hr 
 Ventillátortól gyors, meghajtók üresjárati két PSUs kapcsolva | 2.14 A | 0.49 kW  | 1671.95 BTU/hr | 4.99 A | 0.54 kW | 1842.56 BTU/hr 
 Gyors, ventillátortól meghajtók üresjárati, egy PSU hajtott egyik üresjárati | 2.05 A | 0.48 kW | 1637.83 BTU/hr | 4.58 A | 0,50 kW | 1706.07 BTU/hr 
 Ventillátortól gyors, meghajtók elérése, két PSUs kapcsolva | 2.26 PONTBAN A | 0,51 kW | 1740.19 BTU/hr | 4.95 A | 0.54 kW | 1842.56 BTU/hr 
 Gyors ventillátortól meghajtók elérése, egy PSU hajtott egyik üresjárati | 2.14 A |0.49 kW | 1671.95 BTU/hr | 4.81 A  | 0.53 kW | 1808.44 BTU/hr 

## <a name="disk-drive-specifications"></a>Meghajtón alkalmazásra vonatkozó technikai adatok  

Az StorSimple eszköz 12 3,5 hüvelykes űrlap tényező soros csatolt SCSI (Társítások) lemezmeghajtók támogatja. A tényleges meghajtók kombinálja a félvezető meghajtók (SSD) vagy a merevlemez-meghajtók (HDDs), attól függően, hogy a termék konfigurációja is lehet. A 12 meghajtón helyek a 3, 4 konfiguráció, a ház elé találhatók. A EBOD ház egy másik 12 lemezmeghajtók további tárterületet tesz lehetővé. Az alábbiakban mindig HDDs.  

## <a name="storage-specifications"></a>Tárterület alkalmazásra vonatkozó technikai adatok
StorSimple eszközök kombinálja a merevlemez-meghajtók és félvezető meghajtók a 8100 és 8600 is telepítve van. A 8100 és 8600 használható teljes kapacitásának, hogy körülbelül 15 TB és 38 TB a kurzor. Az alábbi táblázat a dokumentum SSD, a merevlemez és a felhő kapacitás StorSimple megoldás kapacitásának környezetében részleteit.

| Eszköz modell / kapacitás                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| A merevlemez-meghajtók (HDDs) száma              |   8                                                     |  19                                                     |
| Meghajtók (SSD) félvezető száma            |   4                                                     |  5                                                      |
| Egyetlen merevlemez-kapacitás                            |   4 TB                                                  |  4 TB                                                   |
| Egyetlen SSD kapacitás                            |   400 GB                                                |  800 GB                                                 |
| Szabad kapacitással                                 |   4 TB                                                  | 4 TB                                                    |
| Használható merevlemez-kapacitás                            | 14 TB                                                   | 36 TB                                                   |
| Használható SSD kapacitás                            | 800 GB                                                  | 2 TB                                                    |
| Teljes használható kapacitás *                         | ~ 15 TB                                                 | ~ 38 TB                                                 |
| Maximális megoldás kapacitás (beleértve a felhőben)    | 200 TB                                                  | 500 TB                                                  |


<sup> * </sup> -  *Használható teljes kapacitásának tartalmazza a rendelkezésre álló kapacitás adatok, a metaadat-alapú és a buffers.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Ház dimenziók és a vastagság alkalmazásra vonatkozó technikai adatok  

Az alábbi táblázatokban felsoroljuk a különböző ház specifikációi dimenziók és a vastagság parancsot.  

### <a name="enclosure-dimensions"></a>Ház méretek
Az alábbi táblázat a méretét, a ház milliméterre és hüvelyk.

|Ház |Milliméterre |Hüvelyk |
|----------|------------|-------| 
| Magasság |87.9 | 3.46 |
| Szélesség bordában keresztül | 483 | 19.02 |
| Szélesség ház törzsében keresztül | 443-as | 17.44 |
| Az első bordában ház szervezet részén a z | 577 | 22.72 |
| A művelet panelen ház legtávolabbi részén a z | 630.5 | 24.82 |
| A bordában ház legtávolabbi részén a z |   603 | 23.74 |

### <a name="enclosure-weight"></a>Súly ház  

Attól függően, hogy a konfigurálás egy teljesen betöltött elsődleges ház 33 kgs 21-től mérjük is, és kezelje, akkor két személy igényel. 
 
| Ház | Súly |
|-----------|--------| 
| Maximális súly (konfigurációjától függ) |30 kg – 33 kg |
| Üres (nincs felszerelve meghajtó) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Ház környezet alkalmazásra vonatkozó technikai adatok  

Ebben a szakaszban a leírásokat, a ház környezettel kapcsolatos sorolja fel. A hőmérsékleti, páratartalom, magasság, shock, rezgő, tájolás, biztonság és elektromágneses kompatibilitás (EMC) ebbe a kategóriába szerepelnek.  

### <a name="temperature-and-humidity"></a>Hőmérséklet és páratartalom

| Ház        | Környezeti hőmérsékleti tartomány  | Környezeti relatívpáratartalom | Maximális nedves bura   |
|------------------|----------------------------|---------------------------|--------------------|
| Műveleti      | 5 OC - 35 OC (41° F - 95° F)    | 80 %-os 20 %-os nem-kondenzációs- | 28 OC (82° F)        |
| Nem működő  | -40 OC - 70 OC (40° F - 158° F) | 100 %-os 5 %-os nem kicsapódó  | 29 OC (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Légmozgás, magasság, shock, rezgő, tájolás, biztonság és EMC
 
| Ház          | Műveleti alkalmazásra vonatkozó technikai adatok                                                |
|--------------------|---------------------------------------------------------------------------| 
| A légmozgás            | Rendszer légmozgás elülső hátsó. Rendszerkövetelmé egy alacsony nyomású, hátsó-kipufogógáz telepítés kell működtetni. Állvány az ajtók és akadálya által létrehozott nyomása nem haladhatja meg a 5 pascalban (0,5 mm vízoszlop). |
| Magasság, működési  | az 5 oC feletti 7000 láb vonja minősítette maximális működési hőmérsékleti 3045 méter (10 000 láb-100 láb) való méter-30 szűrőt. |
| Magasság, nem működik  | -305 méter és a 12,192 méter (40 000 láb-1,000 láb) |
| Műveleti Shock  | 5g 10 ms ½ szinusza |
| Nem működő Shock  | 30g 10 ms ½ szinusza |
| Műveleti rezgés  | 0.21g RMS 5-500 Hz véletlen |
| Nem működő rezgés  | Tartalomvédelmi 1.04-es g 2-200 Hz véletlen |
| Rezgés, áthelyezés  | 3g 2-200 Hz szinusza |
| Tájolás és csatlakoztatása  | 19" állvány csatlakoztatási (2 EIA egység) |
| Állvány sínek  | Minimális 700 mm (31.50 hüvelyk) mélység igazítása állványon IEC 297 kompatibilis |
| Biztonság és engedélyek  |   CE és UA EN 61000-3-as, IEC 61000-3-as, UA 61000-3-as |
| EMC  | EN55022 (CISPR – A), FCC A |

## <a name="international-standards-compliance"></a>Nemzetközi szabványok megfelelőség
A Microsoft Azure StorSimple eszköz megfelel-e a következő nemzetközi követelmények:  

- CE - EN 60950-1  
- Központi banki IEC 60950-1-jelentés  
- UA és cUL való UA 60950-1  

## <a name="safety-compliance"></a>Biztonság, megfelelőség  

A Microsoft Azure StorSimple eszköz megfelel-e a következő biztonsági minősítések:  

- Rendszer termék típus jóváhagyási: UA, cUL, CE  
- Biztonság, megfelelőség: UA 60950 IEC 60950, EN 60950  

## <a name="emc-compliance"></a>EMC megfelelőség 

A Microsoft Azure StorSimple eszköz megfelel-e a következő EMC minősítések.  

### <a name="emissions"></a>Kibocsátás

Az eszköz nem EMC-kompatibilis vezetett és kisugárzott kibocsátás szintek.  

- Vezetett kibocsátás szintek korlátozni: CFR 47 rész 15B osztály A EN55022 osztály A CISPR osztály A  
- Kisugárzott kibocsátás szintek korlátozni: CFR 47 rész 15B osztály A EN55022 osztály A CISPR osztály A   

### <a name="harmonics-and-flicker"></a>Harmonikus és Villódzás  

Az eszköz megfelel-e EN61000-3-2 és 3.  

### <a name="immunity-limit-levels"></a>Mentesség korlát szintek  
Az eszköz megfelel-e EN55024.  

## <a name="ac-power-cord-compliance"></a>AC power vezeték megfelelőség
  
A beépülő modul és a teljes power vezeték-összeállítás meg kell felelnie az ország, amelyben az eszköz használatban van szükség, és biztonsági jóváhagyásokat, amelyek az adott országban elfogadhatók kell rendelkezniük. Az alábbi táblázatokban felsoroljuk az Amerikai Egyesült Államok és Európa szabványoknak.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC vezeték - Amerikai Egyesült Államok (felsorolt NRTL kell használni)

| Összetevő       | Meghatározása                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Vezeték-típus       | ÜKtgE vagy SVT, a 18 AWG minimum, a 3 vezető, 2.0-s méter maximális hossza |
| Beépülő modul            | NEMA 5-15P résidőalapban típusú mellékletet beépülő minősítette 120 V, 10 A; vagy IEC 320 14, 250 V, A 10 |
| Szoftvercsatorna          | IEC 320 C-13, 250 V, 10 A                                         |

### <a name="ac-power-cords---europe"></a>AC vezeték - Európa

| Összetevő       | Meghatározása                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Vezeték-típus       | Összehangolt, H05-VVF-3G1.0                                         |
| Szoftvercsatorna          | IEC 320 C-13, 250 V, 10 A                                         |

## <a name="supported-network-cables"></a>Támogatott hálózati kábel  

A 10 GbE hálózati kapcsolatok adatok 2 és 3 adatok tekintse át a [támogatott hálózati kábel és modulok listáját](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Következő lépések

Most már készen áll a adatközpontban StorSimple eszköz telepítése. További tudnivalókért olvassa el a [a helyszíni eszköz telepítése](storsimple-deployment-walkthrough-u2.md)című témakört.  
