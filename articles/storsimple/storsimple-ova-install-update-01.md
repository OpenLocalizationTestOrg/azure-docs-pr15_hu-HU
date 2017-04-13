<properties 
   pageTitle="Frissítések telepítése a StorSimple virtuális tömb |} Microsoft Azure"
   description="Ismerteti a portál és -gyorsjavítás módszerrel frissítések telepítése a StorSimple virtuális tömb webes felhasználói felület használatával"
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
   ms.date="09/07/2016"
   ms.author="alkohli" />

# <a name="install-updates-on-your-storsimple-virtual-array"></a>Frissítések telepítése a StorSimple virtuális tömb

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti a frissítések telepítése a StorSimple virtuális tömb és az Azure klasszikus portálon keresztül a helyi webes felhasználói felület lépéseket. Szoftverfrissítések és a StorSimple virtuális tömb naprakészen gyorsjavítások alkalmazni szeretne. 

Ne feledje, hogy egy frissítés vagy az gyorsjavítás az eszköz újraindítása. Tekintve, hogy a StorSimple virtuális tömb egyetlen csomópont eszközt, bármilyen I/O folyamatban megszakadása, és az eszköz legrövidebb leállás találkozik. 

Mielőtt egy frissítést, azt javasoljuk, hogy Ön által mennyiségeket vagy offline megosztások az állomáson első, és kattintson az eszközt. Ez a kis méretűre állítása adatsérülésekkel lehetőségét.

> [AZURE.IMPORTANT] Ha a frissítés 0,1 vagy Georgia szoftver verziók futtatja, telepítse 0,3 keresztül a helyi webes felhasználói felület gyorsjavítás módszer kell használnia. Ha frissítés 0,2 futtatja, azt javasoljuk, hogy az Azure klasszikus portálon keresztül a frissítések telepítése.

## <a name="use-the-local-web-ui"></a>A helyi webes felhasználói felület használata 
 
Két lépésből áll a helyi webes felhasználói felület használatakor:

- Töltse le a frissítés és a javítás
- Telepítse a frissítést, vagy a javítás

### <a name="download-the-update-or-the-hotfix"></a>Töltse le a frissítés és a javítás

A következő lépésekkel töltheti le a szoftver frissítés a Microsoft Update katalógusból.

#### <a name="to-download-the-update-or-the-hotfix"></a>Töltse le a frissítés és a javítás

1. Az Internet Explorer indítása, és kattintson a [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Ha első alkalommal használja a Microsoft Update katalógus ezen a számítógépen, kattintson a **telepítse** , amikor a program megkérdezi, hogy a Microsoft Update katalógus bővítmény telepítése.
  
3. A Microsoft Update katalógus a Keresés mezőbe írja be a letölteni kívánt javítás Tudásbázis (KB) számát. Írja be a **3182061** frissítés 0,3, és kattintson a **Keresés**gombra.

    Gyorsjavítás jelenik meg a lista, például **StorSimple virtuális tömb frissítés 0,3**.

    ![A keresési katalógus](./media/storsimple-ova-install-update-01/download1.png)

4. Kattintson a **hozzáadása**gombra. A frissítés bekerül a kosár.

5. Kattintson a **Kosár megtekintése hivatkozásra**.

6. Kattintson a **Letöltés**gombra. Adja meg vagy **tallózással keresse meg** a helyi helyét szeretné megjeleníteni a letöltéseket. A frissítések a megadott helyen letöltési és azt az almappát, mint a frissítés azonos nevű helyezni. A mappa is másolható hálózati megosztáson elérhető az eszközről.

7. Nyissa meg a másolt mappát, és meg kell jelennie a Microsoft Update önálló csomagfájlt `WindowsTH-KB3011067-x64`. Ez a fájl gyorsjavítás vagy a frissítés telepítése szolgál.


### <a name="install-the-update-or-the-hotfix"></a>Telepítse a frissítést, vagy a javítás

A frissítés vagy gyorsjavítás telepítése előtt győződjön meg arról, hogy a frissítés, vagy a javítás letöltött vagy a szolgáltató a helyi vagy hálózati megosztáson keresztül érhető el. 

Ennek a módszernek frissítések telepítéséhez, ám-eszközön, és frissítheti a 0,1 szoftver verziók. Ez az eljárás kevesebb mint 2 percig tart. A következő lépésekkel gyorsjavítás vagy a frissítés telepítése.


#### <a name="to-install-the-update-or-the-hotfix"></a>A frissítés vagy a javítás telepítése

1. A helyi webes felhasználói felület, válassza a **Karbantartás** > **Frissítést**.

    ![eszköz frissítése](./media/storsimple-ova-install-update-01/update1m.png)

2. A **fájl elérési útja frissítése**adja meg a fájl nevét a frissítésre vagy a javítás. A frissítés vagy gyorsjavítás telepítőfájlt is tallózással, ha egy hálózati megosztáson elhelyezni. Kattintson a **alkalmazása**gombra.

    ![eszköz frissítése](./media/storsimple-ova-install-update-01/update2m.png)

3.  Figyelmeztetés jelenik meg. Ez az adott akkor egyetlen csomópont eszköz, a frissítés, az eszköz újraindítása és legrövidebb leállás van. Kattintson a jelölőnégyzet ikonra.

    ![eszköz frissítése](./media/storsimple-ova-install-update-01/update3m.png)

4. A frissítés kezdődik. Miután sikeresen frissítése az eszköz újraindítása. A helyi felhasználói felületének Ez az időtartam nem érhető el.

    ![eszköz frissítése](./media/storsimple-ova-install-update-01/update5m.png)

5. Az újraindítás befejezése után, megnyílik a **Jelentkezzen be a** lapot. Ellenőrizze, hogy az eszközhöz frissítette, a helyi webhely felhasználói felülete használja a **Karbantartás** > **Frissítést**. A megjelenített szoftver verziószáma **10.0.0.0.0.10288.0** frissítés 0,3 kell lennie.

    > [AZURE.NOTE] A helyi webhely felhasználói felület kissé másképp és az Azure klasszikus portált a szoftver verziók azt jelentést. Például a helyi webes Felhasználóifelület-jelentések **10.0.0.0.0.10288** , és az Azure klasszikus portál jelentések **10.0.10288.0** az azonos verzióban. 

    ![eszköz frissítése](./media/storsimple-ova-install-update-01/update6m.png)





## <a name="use-the-azure-classic-portal"></a>Az Azure klasszikus portál használata

Ha frissítés 0,2 fut, azt javasoljuk, hogy az Azure klasszikus portálon keresztül frissítések telepítése. A portál eljárás elvégzéséhez a felhasználó számára beolvasása, töltse le és a frissítések telepítése. Ez az eljárás perc körülbelül 7 befejezéséhez. A következő lépésekkel gyorsjavítás vagy a frissítés telepítése.

[AZURE.INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

A telepítés befejezése után (a feladat állapota 100 %-os), nyissa meg **Eszközök > karbantartási > szoftverfrissítések**. A megjelenített szoftver verziószáma 10.0.10288.0 kell lennie.

![eszköz frissítése](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Következő lépések

További tudnivalók [a StorSimple virtuális tömb felügyelete](storsimple-ova-web-ui-admin.md).
