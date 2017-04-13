<properties 
   pageTitle="Cserélje le a PCM StorSimple eszközén |} Microsoft Azure"
   description="Megtudhatja, hogyan távolítsa el, és cserélje le a Power és hűtési modul (PCM) StorSimple mobileszközön"
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>A Power és hűtési modul cseréje StorSimple mobileszközön

## <a name="overview"></a>– Áttekintés

A Power és hűtési modul (PCM) a Microsoft Azure StorSimple eszköz áll egy power ellátási és hűtési, amely az elsődleges és a EBOD letölthetés vezérelni ventillátortól. Van, amely az egyes ház igazolt PCM csak egy mintája. Az elsődleges ház egy 764 W PCM hitelesített van, és a EBOD ház hitelesített van egy 580 W PCM. Habár a PCMs az elsődleges ház és a EBOD ház különböző, a csere eljárás megegyezik.

Ebből az oktatóanyagból megtudhatja, hogy miként:

- Egy PCM eltávolítása
- A csere PCM telepítése

>[AZURE.IMPORTANT] Előtt eltávolítja, és egy PCM cseréje, tekintse át a biztonsági [StorSimple hardver összetevő](storsimple-hardware-component-replacement.md)helyett.

## <a name="before-you-replace-a-pcm"></a>Mielőtt egy PCM cseréje

Tartsa szem előtt a következő fontos problémákat, mielőtt a PCM lecseréli:

- Ha nem sikerül a PCM power ellátására, hagyni a hibás modul telepítve van, de távolítsa el az elektromos hálózathoz. A ventilátor power fogadása ki, és adja meg a megfelelő hűtési továbbra is továbbra is. Ha nem sikerül a ventilátor, a PCM ki azonnal kell cserélni.

- Mielőtt eltávolítana a PCM, a power a PCM (ha van ilyen) a Váltás a fő kikapcsolásával vagy leválasztása az elektromos hálózathoz fizikailag eltávolításával. Ezzel a megoldással figyelmeztetés, hogy a rendszer, hogy a power leállítása rövidesen.

- Győződjön meg róla, hogy a többi PCM funkcionális folyamatos rendszer működéséhez a hibás PCM cseréje előtt. A hibás PCM helyettesíteni kell egy teljesen műveleti PCM minél korábban beállítást.

- PCM modul helyettesítő csak néhány percig tart, de a sikertelen PCM túlmelegedését megakadályozására eltávolítása 10 percen belül kell elvégezni.

- Figyelje meg, hogy a csere 764 W PCM modulok gyárilag teljesült nem tartalmaz a biztonsági mentés elem modulra. Az elem eltávolítása a hibás PCM, és helyezze a csere modulba a csere végrehajtása előtt kell. További tudnivalókért lásd: [eltávolítása](storsimple-battery-replacement.md)és a biztonsági mentés elem modul beszúrása.


## <a name="remove-a-pcm"></a>Egy PCM eltávolítása

Ha egy kiemelt és hűtési modul (PCM) eltávolítása a Microsoft Azure StorSimple eszközről készen áll, kövesse az alábbi utasításokat.

>[AZURE.NOTE] Mielőtt a PCM, ellenőrizheti, hogy a helyes-e helyettesítő (764 az elsődleges ház W) vagy 580 W esetében a EBOD ház.

#### <a name="to-remove-a-pcm"></a>Ha el szeretne távolítani egy PCM

1. Az Azure klasszikus portálon kattintson az **eszközök** > **karbantartási** > **Hardveres állapotát**. A **Megosztott összetevők** azonosítása, amelyek azért nem sikerült PCM PCM összetevők állapotának ellenőrzése:

     - Ha egy kiemelt ellátásának PCM 0 nem sikerült, **Power adja meg a PCM 0** állapotának piros lesz.

     - Ha egy kiemelt ellátási PCM 1 nem sikerült, **Power adja meg az PCM 1** állapotának piros lesz.

     - Ha nem sikerült a ventilátor PCM 1, **hűtési PCM 0 0** vagy **1 hűtési PCM 0** állapotának piros lesz.

2. Keresse meg a sikertelen PCM az elsődleges ház hátulján. Ha egy 8600 modell futnak, azonosítják az elsődleges ház megjeleníti a rendszer egység azonosítószám jelenik meg az első panelen LED-kijelzőjén. Erőforrás-azonosító jelenik meg az elsődleges ház alapértelmezés szerint **00**, mivel az erőforrás-azonosító jelenik meg a EBOD ház alapértelmezett: **01**. Az alábbi ábra és a táblázat ismertetik LED-kijelzőjén, az első panelen.

    ![Levelezőlap első OPS csúszkáját azonosító](./media/storsimple-power-cooling-module-replacement/IC740991.png)

     **Ábra 1** A készülék elülső panel  

  	|Címke|Leírás|
  	|:---|:-----------|
  	|1|Elnémítás gomb|
  	|2|Rendszer power|
  	|3|Hibafa modul|
  	|4|Logikai hiba|
  	|5|Egység azonosító megjelenítése|

3. Az elsődleges ház háttereként LED felügyeleti jelző is használható a hibás PCM azonosítása. Lásd az alábbi ábra, és megtudhatja, hogyan használható a LED keresse meg a hibás PCM táblázatot. Ha például a megfelelő **Ventilátor sikertelen** LED ég van, ha a ventilátor sikertelen volt. Hasonlóképpen ha a megfelelő **AC Fail** LED ég van, a power ellátási sikertelen volt. 

    ![Ellenőrző eszköz PCM mutató LED hátlap](./media/storsimple-power-cooling-module-replacement/IC740992.png)

     **Ábra 2** Oldalán PCM LED jellel

  	|Címke|Leírás|
  	|:---|:-----------|
  	|1|AC áramszünet|
  	|2|Ventilátor hiba|
  	|3|Elem hiba|
  	|4|PCM OK GOMBRA.|
  	|5|Adatközpont áramszünet|
  	|6|Megfelelő elem|

4. Olvassa el az alábbi ábra a vissza az StorSimple eszköz keresse meg a sikertelen PCM modulra. PCM 0 a bal oldalon pedig PCM 1 a jobb oldalon. Az alábbi táblázat ismerteti a modulokat.

     ![Az eszköz elsődleges ház modulok hátlap](./media/storsimple-power-cooling-module-replacement/IC740994.png)

     **Ábra 3** Eszköz és beépülő modulok oldalán 

  	|Címke|Leírás|
  	|:---|:-----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Vezérlő 0|
  	|4|1 vezérlő|

5. Kapcsolja ki a hibás PCM, és húzza ki a forrás. Most már eltávolíthatja a PCM.

6. A együtt, és a hordozható és mutatóujj között a PCM leíró oldalán bonyolultnak, és nyomja össze őket, hogy nyissa meg a leíró.

    ![Nyitó PCM fogópont](./media/storsimple-power-cooling-module-replacement/IC740995.png)

    **A 4-es szám** A PCM leíró megnyitása

7. A leíró fogja túl, és távolítsa el a a PCM.

    ![PCM eszköz eltávolítása](./media/storsimple-power-cooling-module-replacement/IC740996.png)

    **Ábra 5** A PCM eltávolítása

## <a name="install-a-replacement-pcm"></a>A csere PCM telepítése

Kövesse ezeket az utasításokat követve telepítse a PCM StorSimple eszköze. Győződjön meg arról, hogy a biztonsági mentés elem modulban, a Csere (764 W PCMs vonatkozik) PCM telepítése előtt beszúrt. További tudnivalókért lásd: [eltávolítása](storsimple-battery-replacement.md)és a biztonsági mentés elem modul beszúrása.

#### <a name="to-install-a-pcm"></a>Egy PCM telepítése

1. Ellenőrizheti, hogy a helyes helyett PCM a ház. Az elsődleges ház szüksége van egy 764 W PCM, és a EBOD ház szüksége van egy 580 W PCM. Az elsődleges helyiségben 580 W PCM, illetve a 764 W PCM EBOD helyiségben nem megpróbálja kell. Az alábbi képen látható, ha ezek az információk a címkét, amelyet a PCM dobozának azonosítása.

    ![Eszköz PCM címke](./media/storsimple-power-cooling-module-replacement/IC740973.png)

    **Ábra 6** PCM címke

2. Ellenőrizze, hogy az összekötők különös tekintettel, a ház kárt. 
                                        
    >[AZURE.NOTE] **A modul nem telepíthető, ha minden összekötő elgörbülve.**

3. A nyitott állapotban PCM fogóponttal húzza a modul be, a ház.

    ![PCM eszköz telepítése](./media/storsimple-power-cooling-module-replacement/IC740975.png)

    **Ábra 7** A PCM telepítése

4. Manuális zárja be a PCM fogópontot. Egy kattintással kell hallható, mint a fogópont együtt folytat. 
                                        
    >[AZURE.NOTE] Annak érdekében, hogy az összekötő PIN van-e kapcsolni, akkor is enyhén tug található fogópontot a a zár felengedése nélkül. A PCM diák meg, azt jelenti, hogy a zár lezárásának, mielőtt az összekötők folytat.

5. Csatlakozás a kábelek, a power forrás, és a PCM.

6. A törzs mentesség bálákban biztonságos. 

7. Kapcsolja be a PCM.

8. Győződjön meg arról, hogy sikeres volt-e a csere: a StorSimple kezelő szolgáltatás Azure klasszikus portálon nyissa meg az **eszközök** > **karbantartási** > **Hardveres állapotát**. A **Megosztott összetevők**a PCM állapotának zöld kell lennie. 
                                        
    >[AZURE.NOTE] Eltarthat néhány percet, amíg a csere PCM teljesen inicializálni.

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [StorSimple hardver összetevő feltölteni](storsimple-hardware-component-replacement.md).
