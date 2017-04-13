<properties 
   pageTitle="Cserélje le a elem StorSimple eszközön |} Microsoft Azure"
   description="Ismerteti, hogyan lehet eltávolítani, csere és a biztonsági mentés elem modul StorSimple eszközén karbantartása."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>A biztonsági mentés elem modul StorSimple eszközén cseréje

## <a name="overview"></a>– Áttekintés

Az elsődleges ház Power és hűtési modul (PCM) a Microsoft Azure StorSimple készülékén tartalmaz egy további elem csomag. A csomag power biztosít, így az StorSimple eszköz adatokat is menteni szeretné az elsődleges ház AC power elvesztése esetén. A elem csomag a *biztonsági mentés elem modul*nevezik. A biztonsági mentés elem modult csak az elsődleges ház (a EBOD ház nem tartalmaz a biztonsági mentés elem modul) StorSimple eszközbe létezik. 

Ebből az oktatóanyagból megtudhatja, hogy miként:

- A biztonsági mentés elem modul eltávolítása 
- Egy új biztonsági elem modul telepítése
- A biztonsági mentés elem modul kezelése

>[AZURE.IMPORTANT] Előtt eltávolítása és cseréje a biztonsági mentés elem modul, tekintse át a biztonsági a [hardver-összetevő helyettesítő StorSimple bemutatása](storsimple-hardware-component-replacement.md).

## <a name="remove-the-backup-battery-module"></a>A biztonsági mentés elem modul eltávolítása

A biztonsági mentés elem modul StorSimple mobileszközére egy mező cserélhető egységhez. Az a PCM telepítése előtt a elem modul kell tárolni az eredeti csomagban. Hajtsa végre az alábbi lépések elvégzésével távolítsa el a biztonsági mentés elem.

#### <a name="to-remove-the-backup-battery-module"></a>Ha el szeretné távolítani a biztonsági mentés elem modul

1. Az Azure klasszikus portálon lépjen az **eszközök** > **karbantartási** > **Hardveres állapotát**. A **Megosztott összetevők**keresse meg az elem állapotát.

2. Azonosítsa a PCM, ahol az elem sikertelen volt. Ábra 1 StorSimple eszköz hátterében jeleníti meg.

    ![Az eszköz elsődleges ház modulok hátlap](./media/storsimple-battery-replacement/IC740994.png)

    **Ábra 1** Vissza az elsődleges eszközt ábrázoló PCM és a vezérlő modulban

  	|Címke|Leírás|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Vezérlő 0|
  	|4|1 vezérlő|

    Amint a 2 a 3-as számú, a kell ég LED PCM 0, amely megfelel a az **Elem hiba** ellenőrző jelző.

    ![Az eszköz PCM figyelése jelző LED hátlap](./media/storsimple-battery-replacement/IC740992.png)

    **Ábra 2** Vissza a PCM megjelenítő LED felügyeleti jelző

  	|Címke|Leírás|
  	|:---|:-----------|
  	|1|AC áramszünet|
  	|2|Ventilátor hiba|
  	|3|Elem hiba|
  	|4|PCM OK GOMBRA.|
  	|5|Adatközpont áramszünet|
  	|6|Megfelelő elem|

3. A PCM és a sikertelen elem eltávolításához kövesse a [eltávolítása egy PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).

4. A eltávolítja a PCM emelje fel a elem modul leíró felfelé elforgatni, az alábbi ábrán jelzett és a forráshoz felfelé az elem törlése.

    ![Elem eltávolítása PCM](./media/storsimple-battery-replacement/IC741019.png)

    **Ábra 3** Az elem eltávolítása a PCM

5. Helyezze a modul csomagolása mező cserélhető egységben.

6. Vissza a hibás egység Microsoft megfelelő karbantartás és kezelése.

## <a name="install-a-new-backup-battery-module"></a>Egy új biztonsági elem modul telepítése

Hajtsa végre az alábbi lépéseket a Csere elem modul telepítése a PCM StorSimple készüléke elsődleges helyiségben található.

#### <a name="to-install-the-battery-module"></a>Az elem modul telepítése

1. A biztonsági mentés elem modul helyezze a PCM a megfelelő tájolását.

2. Nyomja le az elem modul lefelé teljes mértékben az összekötő ülőhelyéül.

3. Az elsődleges helyiségben PCM felülírásához [cserélje le a Power és hűtési modul StorSimple eszközén](storsimple-power-cooling-module-replacement.md)útmutatását.

4. A Csere befejezése után, lépjen az **eszközök** > **karbantartási** > **Hardveres állapotát** az Azure klasszikus portálon. Győződjön meg arról, hogy a telepítés sikeres volt az elem állapotát. Egy zöld állapotát jelzi, hogy az elem megfelelő.

## <a name="maintain-the-backup-battery-module"></a>A biztonsági mentés elem modul kezelése

Az StorSimple eszköz a biztonsági mentés elem modul nyújt a vezérlő power egy power veszteség rendezvény alatt. Lehetővé teszi az StorSimple eszköz mentése előtt ellenőrzött módon leáll fontos adatokat. Elemekkel két feltöltött a PCMs a a rendszer két egymást követő veszteség események képes kezelni.

Az Azure klasszikus portálon a **Karbantartás** lap **Állapot hardver** azt jelzi, hogy az elem hibásan működik, vagy a vonatkozó elhasználódott közelítő. **A PCM 0 elem** vagy a **PCM 1 elem** a **Megosztott összetevők**elem állapotát jelzi. A lapon megjelenik egy megközelítő elhasználódott a **csökkentett teljesítményű** állapotát, és **nem sikerült** a elhasználódott elérhető. 

>[AZURE.NOTE] Az elem jelentheti **sikertelen** , ha egyszerűen szükséges kell fizetnie.
 
Ha megjelenik a **csökkentett teljesítményű** állapotát, a következő lépések javasoljuk:

- Előfordulhat, hogy a rendszer tapasztalt legutóbbi power veszteség, vagy előfordulhat, hogy az elemek alatt rendszeres karbantartási. Megfigyelheti, a rendszer a folytatás előtt 12 órával.

    - Ha az állapot továbbra is **csökkentett teljesítményű** 12 óra folyamatos kapcsolatot AC power a vezérlők és PCMs futtatása után, majd az elem ki kell cserélni. Adja meg, [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) egy új biztonsági elem modulra.

    - Az állapot OK 12 óra elteltével változik, ha az elem működik, és ez csak a karbantartási költség szükséges.

- Ha nem volt egy társított elvesztése AC power és a PCM be van-e kapcsolva, és AC power csatlakozik, az elem ki kell cserélni. [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) rendelés egy új biztonsági elem modulra.

>[AZURE.IMPORTANT] A sikertelen elem nemzeti és területi szabályok szerint megszabadulni. 

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [StorSimple hardver összetevő feltölteni](storsimple-hardware-component-replacement.md).
