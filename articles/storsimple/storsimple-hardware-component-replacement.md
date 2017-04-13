<properties 
   pageTitle="StorSimple hardver összetevő helyettesítő |} Microsoft Azure"
   description="Ismerteti, hogyan biztonságosan cseréje a PCMs, elem, vezérlő modulokat, EBOD vezérlők, lemezmeghajtók és váz StorSimple eszköz."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="storsimple-hardware-component-replacement"></a>StorSimple hardver összetevő cseréje

## <a name="overview"></a>– Áttekintés

A hardver-összetevő helyettesítő oktatóprogramok írja le a Microsoft Azure StorSimple 8000 sorozat és szükséges eltávolítania és cserélje le azokat a lépéseket a hardver összetevői. Ez a cikk ismerteti a biztonsági ikonok, itt mutat hivatkozás a részletes oktatóanyagok és sorolja fel, amelyek cserélhető összetevők.

>[AZURE.IMPORTANT] Mielőtt megnyitná eltávolítása vagy áthelyezése minden StorSimple összetevő, győződjön meg arról, hogy tekintse át a [biztonsági ikon szabályokat](#safety-icon-conventions) és más [biztonságra](storsimple-safety.md).
 
### <a name="safety-icon-conventions"></a>Biztonsági ikon szabályok

Az alábbi táblázat ismerteti az oktatóanyagokban használt biztonsági ikonok. Figyeljen biztonsági ikonokra végig, és cserélje ki az eszközt összetevők menet közben.

| Ikon | Szöveg | További információk |
|:---- |:---- |:-----------|
|![Figyelmeztető ikon](./media/storsimple-hardware-component-replacement/Warning.png)| **VESZÉLYT!** | Azt jelzi, hogy a veszélyes helyzet végződő, ha nem kerülni, halállal vagy károkozás. A jel szót a rendkívüli helyzetekben korlátozódik.|
|![Figyelmeztető ikon](./media/storsimple-hardware-component-replacement/Warning.png)| **FIGYELMEZTETÉS!** | Azt jelzi, hogy a veszélyes helyzet, amely, ha nem kerülni, eredményezhet halála vagy károkozás.|
|![Figyelmeztetés ikon](./media/storsimple-hardware-component-replacement/Caution.png)| **FIGYELMEZTETÉS!** |Azt jelzi, hogy a veszélyes helyzet, amely, ha nem kerülni, eredményezhet kisebb vagy mérsékelt károkozás.|
|![Értesítés ikon](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **MEGJEGYZÉS:** | Azt jelzi, hogy tekinteni, fontos, de nem veszélyeztetik kapcsolatos információk.|
|![Elektromos sokkoló ikon](./media/storsimple-hardware-component-replacement/Electric.png) | **Elektromos sokkoló veszélyt** | Azt jelzi, hogy nagyfeszültségű.|
|![Nehéz súly ikon](./media/storsimple-hardware-component-replacement/Weight.png)| **Nehéz tömeg**| |
|![Nem felhasználói javítható kijelzők ikon](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **Nem felhasználói javítható részei** | Nem fér hozzá kivéve, ha képzett megfelelően.|
|![Olvasható útmutatást ikon](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**Először olvassa el az összes utasításokat**| |
|![Tipp: a kockázat ikon](./media/storsimple-hardware-component-replacement/TipHazard.png)|**Tipp: a kockázat**| |

### <a name="before-you-begin"></a>Első lépések

Megismerkedhet a ebben az oktatóanyagban használt eszköz és biztonsági ikonok biztonsági információt. Nyissa meg a [biztonságos telepítése, és az StorSimple eszköz működtetéséhez](storsimple-safety.md) átfogó információt. Ügyeljen arra, tekintse át a [biztonságra](storsimple-safety.md#handling-precautions) , mielőtt kezelje a StorSimple eszközére. 

Mielőtt megkísérelne összetevő cseréje, vegye figyelembe az alábbi információkat.

![Figyelmeztető ikon](./media/storsimple-hardware-component-replacement/Warning.png) ![elektromos sokkoló ikon](./media/storsimple-hardware-component-replacement/Electric.png) **Figyelmeztetés!** 

- Szabad saját maga megfelelően kisülés vagy elektromos mat használatával modulok és StorSimple eszköze összetevői kezelésekor.

- Bármely áramkört érintetlenül. A megadott fogópontok és segédvonalak használata összetevőket, előfordulhat, hogy ki van téve áramkört kezelése során.

![Figyelmeztető ikon](./media/storsimple-hardware-component-replacement/Warning.png) ![ikon figyelje](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **értesítés:**

Amikor cserélje le a modul **Soha ne hagyja a hátsó, a ház egy üres fiókba**. Szerezze be egy új vagy üres modul probléma rész eltávolítása előtt.

## <a name="hardware-component-replacement-procedures"></a>Hardveres összetevő helyettesítő eljárások

Az elsődleges és/vagy EBOD letölthetés több beépülő modulokat StorSimple 8000 sorozat eszköze áll. A 8100 tartalmaz egy egyetlen elsődleges ház, mivel a 8600 egy elsődleges ház és EBOD tokba kettős ház eszközt.

Az alábbi táblázatban soroljuk fel a fő hardver-összetevő az eszközön. Kattintson a **Csere eljárás** oszlopában nyissa meg a társított oktatóprogram hivatkozásra.

|Összetevők|# Bemutató tartása|Beépülő modul?|Helyettesítő eljárás
|:---------|:--------|:--------------|:---------------------|
| Váz|1|nem|[A váz StorSimple eszközén cseréje](storsimple-chassis-replacement.md) |
|A vezérlők elsődleges|2|igen| [A vezérlő modul StorSimple eszközén cseréje](storsimple-controller-replacement.md) |
|764W Power és hűtési modulok (PCMs)|2|igen| [A Power és hűtési modul cseréje StorSimple mobileszközön](storsimple-power-cooling-module-replacement.md) |
|Biztonsági másolat elem|2|igen| [A biztonsági mentés elem modul StorSimple eszközén cseréje](storsimple-battery-replacement.md) |
|Lemezmeghajtók|12|igen| [A lemezmeghajtó StorSimple eszközén cseréje](storsimple-disk-drive-replacement.md) |

**Tartalomjegyzék 1** Az elsődleges ház a hardver-összetevő

Az elsődleges ház és a EBOD ház különböznek ezek I/O modulokat. Ezenkívül a PCMs kell különböző teljesítményt. Az elsődleges helyiségben PCMs 764 W, mivel a EBOD ház a 580 w Az elsődleges helyiségben PCMs is tartalmazhatnak egy biztonsági elem modulra.

|Összetevők|# Bemutató tartása|Beépülő modul?| Helyettesítő eljárás
|:---------|:--------|:--------------|:---------------------|
|Váz|1|nem| [A váz StorSimple eszközén cseréje](storsimple-chassis-replacement.md) |
|EBOD vezérlők|2|igen| [Egy EBOD vezérlő StorSimple eszközén cseréje](storsimple-ebod-controller-replacement.md) |
|580W Power és hűtési modulok (PCMs)|2|igen| [A Power és hűtési modul cseréje StorSimple mobileszközön](storsimple-power-cooling-module-replacement.md) |
|Lemezmeghajtók|12|igen| [A lemezmeghajtó StorSimple eszközén cseréje](storsimple-disk-drive-replacement.md) |

**Táblázat 2** A EBOD ház a hardver-összetevő

A beépülő modul modulokat az eszközre az alábbi első és hátsó diagramok megjelenik kiemelve. Hol található a különféle beépülő modulokat határozza meg, ha egy másikat szükség a létrehozott diagramokat is használhatja. Az első ábrán lemezmeghajtók és a hátsó diagramokat készíthet a EBOD ház és az elsődleges ház megjelenítése a beépülő modulokat.

![Frontplane lemezmeghajtók eszköz](./media/storsimple-hardware-component-replacement/IC741028.png)

**Ábra 1** A készülék elülső

|Címke|Leírás|
|:----|:----------|
|0 - 11-es|Lemezmeghajtók (összesen 12)|

Az elsődleges ház és a EBOD ház is van meghajtó carrier modulokat. A váz otthont ad tizenkét 3.5-ös "lemezmeghajtók 3, 4-es formátumban vannak rendezve.

![Az eszköz elsődleges ház modulok hátlap](./media/storsimple-hardware-component-replacement/IC740994.png)

**Ábra 2** Az elsődleges ház oldalán

|Címke|Leírás|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|Vezérlő 0|
|4|1 vezérlő|

![Az eszköz EBOD ház beépülő modulokat hátlap](./media/storsimple-hardware-component-replacement/IC769599.png)

**Ábra 3** A EBOD ház oldalán

|Címke|Leírás|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|0 EBOD vezérlő|
|4|1 EBOD vezérlő|

## <a name="field-replaceable-units"></a>Cserélhető mennyiség mező

A következő mező egységek (részegysége) StorSimple mobileszközére érhetők el:

- Váz (beleértve a beépített műveletek ablaktábla)

- 764 W AC PCM

- 580 W AC PCM

- Merevlemez-meghajtó meghajtó carrier modul

- Vezérlő modul

- EBOD vezérlő modul

- Biztonsági másolat elem modul

- Állvány vasúti csomag csatlakoztatása

Adja meg, [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) rendelés e helyettesítő egységek közül.

## <a name="next-steps"></a>Következő lépések

Tekintse át az összes [biztonsági információt](storsimple-safety.md) , mielőtt megkísérelne StorSimple hardver összetevő cseréje.
