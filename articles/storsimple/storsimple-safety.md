<properties 
   pageTitle="Biztonsági a StorSimple eszköz |} Microsoft Azure"
   description="Biztonsági szabályokat, irányelveket és szempontok, és ismerteti biztonságosan telepítéséhez és StorSimple eszköze működnek."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="safely-install-and-operate-your-storsimple-device"></a>Biztonságos telepítése és működtetéséhez StorSimple eszköze

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png)
![elolvasott biztonsági értesítés ikon](./media/storsimple-safety/IC740885.png) **olvassa el a biztonság és a RENDSZERÁLLAPOT ADATAIT**

Ez a cikk a Microsoft Azure StorSimple eszközre alkalmazott összes biztonsági és állapot információt olvasni. A nyomtatott segédvonalak a StorSimple eszközzel jövőbeli hivatkozási teljesült megtartani. Kövesse az utasításokat és megfelelően állíthat be, és a termék növelje a kockázat károkozás vagy elhalálozása, vagy az eszközt vagy eszközöket kárt érdekli hiba. [Ez az útmutató letölthető verziója](http://www.microsoft.com/download/details.aspx?id=44233) érhető el.

## <a name="safety-icon-conventions"></a>Biztonsági ikon szabályok

Az alábbiakban megtalálja a biztonságra, figyelembe kell venni, amikor állíthatja be, és a Microsoft Azure StorSimple eszköz futtatása megtekintésekor ikonok.

| Ikon  | Leírás  |
|:------|:-------------| 
|![Veszélyt ikon](./media/storsimple-safety/IC740879.png) **veszélyt!**|Azt jelzi, hogy a veszélyes helyzet végződő, ha nem kerülni, halállal vagy károkozás. A jel szót, hogy a rendkívüli helyzetekben korlátozódik.| 
|![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png) **Figyelmeztetés!**|Azt jelzi, hogy a veszélyes helyzet, amely, ha nem kerülni, eredményezhet halála vagy károkozás.|
|![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png) **Figyelmeztetés!**|Azt jelzi, hogy a veszélyes helyzet, amely, ha nem kerülni, eredményezhet kisebb vagy mérsékelt károkozás.|
|![Figyelje meg, ikon](./media/storsimple-safety/IC740881.png) **értesítés:**|Azt jelzi, hogy tekinteni, fontos, de nem veszélyeztetik kapcsolatos információk.|
|![Elektromos sokkoló ikon](./media/storsimple-safety/IC740882.png) **elektromos sokkoló veszélyt** |Nagyfeszültségű|
|![Nehéz súly ikon](./media/storsimple-safety/IC740883.png) **nehéz tömeg**| |
|![Senki javítható részeinek ikon](./media/storsimple-safety/IC740879.png) **nincsenek felhasználói javítható részei**|Nem fér hozzá kivéve, ha képzett megfelelően.|
|![Olvassa el a biztonsági értesítés ikon](./media/storsimple-safety/IC740885.png)**először olvassa el az összes utasításokat**| |
|![Tipp: a kockázat ikon](./media/storsimple-safety/IC740886.png) **veszélyt tipp:**| |


## <a name="handling-precautions"></a>Kezelési óvintézkedések

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png) ![nehéz súly ikon](./media/storsimple-safety/IC740883.png) **Figyelmeztetés!** 


Károkozás a kockázatok csökkentésére:

- A teljesen konfigurált ház is összehasonlítani legfeljebb 32 kg (70 dkg); Próbálja meg saját maga által emelje fel.
- Mielőtt a ház mindig győződjön meg arról, hogy elérhetők-e két személy kezelheti a vastagság parancsot. Ne feledje, hogy emelje fel a vastagság kísérel meg egy személy is fenntartása sérülések.
- Nem emelje fel a ház fogópontja a teljesítmény és hűtési modulok (PCMs) található, a hátsó egység szerinti. Ezek nem készült készítése a vastagság parancsot.

## <a name="connection-precautions"></a>Kapcsolat óvintézkedések

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png) ![elektromos sokkoló ikon](./media/storsimple-safety/IC740882.png) **Figyelmeztetés!**

Csökkentheti a károkozás, elektromos sokkoló vagy halál valószínűsége:

- Hajtott több AC forrásból, amikor leválasztja az összes ellátási power teljes leválasztáshoz.

- Véglegesen húzza ki a beállítást, akkor az áthelyezése előtt, vagy ha úgy gondolja, hogy semmilyen módon megsérült.

- Adja meg a forrás vezeték elektromos föld biztonságos kapcsolatot. Győződjön meg arról, hogy a résidőalapban, a ház megfelel nemzeti és a helyi power alkalmazása előtt.

- Győződjön meg arról, hogy a power kapcsolat mindig le van választva ki egy PCM eltávolítása előtt.

- Tekintve, hogy a beépülő modul a az elektromos ellátási hálózathoz az elsődleges eszköz leválasztása, biztosítása, hogy a szoftvercsatorna csatlakozó aljzatokkal a készülék mellett található, és könnyen hozzáférhető.

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png) ![elektromos sokkoló ikon](./media/storsimple-safety/IC740882.png) **Figyelmeztetés!**

Annak valószínűségét, hogy túlmelegedését csökkentése, és az elektromos kapcsolatok fire:

- Adja meg a megfelelő power forrás elektromos túlterhelés védelemmel műszaki előírásának részletes követelményeknek.

- Ne használjon elágaztatott vezeték ("I" érdeklődő).

- Alkalmazandó biztonsági, a kibocsátás és a hőmérsékleti követelmények ahhoz, hogy nincs fedőlap eltávolítása után, és minden öblök és beépülő modulok kell feltölteni, illetve meghajtó üres cellákat.

- Győződjön meg arról, hogy a készülék a gyártó által megadott módon használatos. Ha a berendezések a gyártójuk nem meghatározott módon használja, lehet, hogy a készülék által biztosított védelem látók.

![Figyelje meg, ikon](./media/storsimple-safety/IC740881.png) **értesítés:**

Megfelelő működéséhez a berendezések és termék sérülések elkerülése:

- A RJ45 hátulján az eszközön található legyen az egy Ethernet-kapcsolaton. Ezek kell nem csatlakoznia távközlési hálózaton.

- Ügyeljen arra, hogy az eszköz telepítése egy állványon, amely az első hátra hűtési látványterv rendezhetők.

- Az összes beépülő modulok és üres táblák részei a rendszer adott területét. Ezek csak el kell távolítani egy másikat hozzáadása azonnal is. A rendszer nem kell futtatni modulok vagy a hely üres értékek nélkül.

## <a name="rack-system-precautions"></a>Állvány rendszer óvintézkedések

Amikor csatlakoztatja az eszközt, kabinet állvány figyelembe kell venni az alábbi biztonsági követelményeknek.

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png) ![veszélyt ikon tipp](./media/storsimple-safety/IC740886.png) **Figyelmeztetés!**
 
Annak valószínűségét, hogy a ezt a tippet kárt csökkentése keresztül:

- Állvány tervezésére támogatniuk kell a telepített letölthetés teljes vastagságának és kell tartalmazniuk, stabilizátort szolgáltatások megfelelő meg, hogy a állvány kiengedési, vagy éppen tolni keresztüli telepítés vagy normál használata közben.

- Állvány betöltésekor töltse ki a állvány lentről, és a felülről lefelé üres.

- Nem húzza ki a állvány egynél több ház elkerülése érdekében a toppling állvány veszély egyszerre.

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png) ![elektromos sokkoló ikon](./media/storsimple-safety/IC740882.png) **Figyelmeztetés!**

Csökkentheti a károkozás, elektromos sokkoló vagy halál valószínűsége:

- A állvány a megbízható elektromos terjesztési rendszert kell rendelkeznie. Biztosítania kell a ház túlzott aktuális védelmét, és azt nem lehet telepíteni letölthetés száma szerint többszörösen. Az elektromos felhasználási energiafogyasztást jelenik meg a névleges kell figyelembe venni.

- Az eloszlás elektromos rendszer kell adnia egy megbízható alapokról a állvány az egyes ház.

- A tervező a elektromos rendszer figyelembe kell venni a teljes alapokról szivárgás aktuális az összes power készleteket az összes melléklet. Ne feledje, hogy az egyes ház minden egyes kiemelt ellátási alapokról szivárgás jelenlegi 1.0 mA legfeljebb 60 Hz, a 264 volt. A állvány előírhatja címkézése "magas SZIVÁRGÁS aktuális. Kapcsolat alapokról (föld) elengedhetetlen egy ellátási csatlakozás előtt."

- A állvány konfigurálásakor a letölthetés, meg kell felelnie a biztonsági követelményeknek UA 60950-1 és 60950-1/EN IEC 60950-1.

![Figyelje meg, ikon](./media/storsimple-safety/IC740881.png) **értesítés:**

A megfelelő hűtési állvány rendszerünk:

- Győződjön meg arról, hogy a állvány tervezés figyelembe veszi a maximális ház működési környezeti hőmérséklet 35 Celsius-fokban (95 Celsius fok).

- A rendszer üzemeltetik alacsony nyomású, hátsó-kipufogógáz telepítés (nyomása állvány az ajtók és legfeljebb 5 Pascal [0,5 mm vízoszlop] akadálya által létrehozott).

## <a name="power-cooling-module-pcm-precautions"></a>A Power hűtési modul (PCM) óvintézkedések

Az eszköz két PCMs működő lett tervezve. A PCMs mindegyikének egy power ellátási és egy kettős tengely ventilátorral. Kritikus feltétel során a rendszer az egyik power ellátás folytatódó szokásos tevékenységek közben hibát tesz lehetővé. Két PCMs (és ezért power éppen a Kellékek) telepíteni kell az mindig. Egy egyetlen PCM nem nyújt a felesleges power. Emiatt a hiba, akár egy PCM legrövidebb leállás vagy adatvesztést vezethet.

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png) ![elektromos sokkoló ikon](./media/storsimple-safety/IC740882.png) **Figyelmeztetés!**

Csökkentheti a károkozás, elektromos sokkoló vagy halál valószínűsége:

- Ne távolítsa el a magában foglalja a PCM. Nincs áramütés belül veszélyt jelent. Vissza a PCM, és egy másikat, [lépjen kapcsolatba a Microsoft terméktámogatási](https://msdn.microsoft.com/library/azure/dn757750.aspx)beszerzése.

![Figyelje meg, ikon](./media/storsimple-safety/IC740881.png) **értesítés:**

Megfelelő működéséhez a berendezések és termék sérülések elkerülése:

- A sikertelen PCM ezt kell lecserélnie a 24 órán belül. Miután egy PCM törlődik a helyettesítő, a csere eltávolítása után 10 percen belül kell elvégezni.

- Ne távolítsa el a PCM, kivéve, ha egy másikat azonnal telepíthető. A ház nem kell üzemeltetni helyen minden modul nélkül.

## <a name="electrostatic-discharge-esd-precautions"></a>Kisülés (szoftverletöltéssel foglalkozó) óvintézkedések

![Figyelje meg, ikon](./media/storsimple-safety/IC740881.png) **értesítés:**

Tekintse át az szoftverletöltéssel foglalkozó kapcsolatos alábbi figyelmeztetéseket.

- Győződjön meg arról, hogy telepítette és ellenőrizni a megfelelő elektromos körkörös vagy bokája elé.

- Tekintse át az összes hagyományos szoftverletöltéssel foglalkozó óvintézkedések modulok és -összetevők kezelésekor.

- Kerülje a partner hátlap összetevők és a modul összekötők.

- Jótállás szoftverletöltéssel foglalkozó sérülések nem vonatkozik.

## <a name="battery-disposal-precautions"></a>Elem elvezető óvintézkedések

A power ellátási egy speciális elem használja a memória tartalmának védelme ideiglenes, rövid érvényességi idejű áramkimaradásokat során. Az elem van a PCM az ülő. Tartsa szem előtt, az elem kapcsolatban az alábbi információkat.

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png) **Figyelmeztetés!**

Nadrágok, fire, kihúzása, károkozás vagy halál a kockázatok csökkentésére:

- Nemzeti/regionális szabályok szerint használt elemek megszabadulni.

- Nem nem fejtheti vissza, őröljük, vagy a hőtérkép feletti 60 Celsius-fokban (140 Celsius fok) vagy hamvasszuk. Cserélje ki a PCM elem egy megadott elem. Egy másik elem használatát a kockázatot fire vagy alábontási.

- Ha ezek a power ellátási törlődjenek védelmi lezáró használja az elemeket.

![Figyelje meg, ikon](./media/storsimple-safety/IC740881.png) **értesítés:**

Amikor szállítási vagy az egyéb esetben a elemek légi, a IATA lítium elem útmutatást dokumentum követése a [http://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx](http://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx)

Ezeket a biztonsági hirdetményeket ellenőrzését követően a következő lépésekkel nem kicsomagolása, állvány és kábelmodemek az eszközön.

## <a name="next-steps"></a>Következő lépések

- 8100-alkalmazás nyissa meg [a StorSimple 8100 eszköz](storsimple-8100-hardware-installation.md)telepítése.

- Egy 8600 eszközön lépjen [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md).

