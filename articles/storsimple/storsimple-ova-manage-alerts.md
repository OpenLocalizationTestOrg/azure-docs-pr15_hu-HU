<properties 
   pageTitle="Megtekintheti és kezelheti a StorSimple virtuális tömb riasztások |} Microsoft Azure"
   description="Virtuális tömb StorSimple riasztási feltételek és súlyosságát és értesítések kezelése a StorSimple kezelő szolgáltatás használata ismerteti."
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
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-alerts-for-the-storsimple-virtual-array"></a>A StorSimple kezelő szolgáltatás használatával megtekintheti, és a StorSimple virtuális tömb értesítések kezelése

## <a name="overview"></a>– Áttekintés

Az **értesítések** fülre a StorSimple kezelő szolgáltatás teszi lehetővé, hogy tekintse át, és törölje a jelet a valós idejű alapon StorSimple virtuális tömb – kapcsolatos értesítések. Ezen a lapon a központi figyelheti a Szolgáltatásállapot-hiba a StorSimple virtuális tömbök (más néven StorSimple helyszíni virtuális eszközök) és a teljes StorSimple a Microsoft Azure megoldást.

Ebben az oktatóanyagban azt ismerteti, hogy miként konfigurálható az értesítések, a közös riasztási feltételeket, a riasztási fontossági szintek, illetve megtekintése és nyomon követése értesítések. Emellett figyelmeztető – rövid összefoglaló táblákat, amelyek lehetővé teszik, hogy gyorsan egy adott típusú keresse meg és megfelelően reagáljon tartalmazza.

![Az értesítések lapra](./media/storsimple-ova-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Értesítési beállítások konfigurálása

Megadhatja, hogy szeretné-e értesítést e-mailek, az egyes StorSimple virtuális eszközén riasztási feltételek szerint. Továbbá azonosíthatja értesítés címzettnek az e-mail címüket beírja a **többi e-mail címzett** mezőben egymástól pontosvesszővel elválasztva.

>[AZURE.NOTE] Legfeljebb 20 e-mail címek egy virtuális eszköz adhat meg.

Miután engedélyezte a virtuális eszköz az értesítő e-mailt, a tagok értesítési listájára minden alkalommal, amikor egy kritikus riasztás e-mailt kap. Az üzeneteket küld a *storsimple-alerts-noreply@mail.windowsazure.com* , és leírja a riasztási feltétel. A címzettek **leiratkozási** e-mail értesítési listájára maguk eltávolítása elemre.

#### <a name="to-enable-email-notification-of-alerts-for-a-virtual-device"></a>Ahhoz, hogy egy virtuális eszközre vonatkozó értesítések az értesítő e-mailt

1. Nyissa meg a **eszközök** > **konfigurációs** a virtuális eszközt. Nyissa meg a **riasztási beállítások** szakaszban.

    ![riasztási beállítások](./media/storsimple-ova-manage-alerts/alerts2.png)

2. A **riasztási beállítások**csoportban állítsa be a következőket:

    1. A **Küldés e-mailben értesítést** mezőben válassza az **Igen gombra**.

    2. Az **E-mail szolgáltatás-rendszergazdák** mezőben válassza az **Igen lehetőséget** , ha azt szeretné, hogy a szolgáltatás rendszergazdája, és minden további rendszergazdák fogadása az értesítéseket.

    3. A **többi e-mail címzett** mezőjébe írja be más az értesítések kapó címzettek e-mail címét. Írja be nevét a formátumban *someone@somewhere.com*. El az e-mail címeket egymástól pontosvesszővel. Legfeljebb 20 e-mail címek egy virtuális eszköz is beállíthatja. 

        ![értesítések értesítési beállításainak](./media/storsimple-ova-manage-alerts/alerts3.png)

3. A lap alján kattintson a **mentése** a konfiguráció mentése gombra.

4. A próba értesítő e-mailt küldeni, kattintson a **próba-e-mail küldése**mellett lévő nyíl ikonra. A StorSimple kezelő szolgáltatás állapotüzenetek jeleníti meg, a próba-értesítés továbbítja. 

5. A következő üzenet jelenik meg, kattintson az **OK gombra**. 

    ![Értesítések küldött értesítő e-mailt tesztelése](./media/storsimple-ova-manage-alerts/alerts4.png)

    >[AZURE.NOTE] Ha a teszt értesítő üzenet nem lehet küldeni, akkor a StorSimple kezelő szolgáltatás egy megfelelő üzenetet jelenít meg. Kattintson az **OK gombra**, várjon néhány percet, és próbálkozzon ismét elküldeni a próba-értesítő üzenet. 

    A próba-értesítő üzenet a következőhöz hasonló lesz.

    ![Értesítések e-mailek példa tesztelése](./media/storsimple-ova-manage-alerts/alerts5.png)

## <a name="common-alert-conditions"></a>Közös riasztási feltételek

A StorSimple virtuális tömb készít riasztások feltételek számos választ. A leggyakoribb riasztási feltételek a következők:

- **Csatlakozási problémákat tapasztal** – az értesítések fordulhat elő, ha van, akkor az adatok átvitele megnehezítheti. Kapcsolati is problémákat tapasztal az adatok átvitele során és Azure tárterület-fiókból vagy a virtuális eszközök és a StorSimple kezelő szolgáltatás közötti kapcsolatot hiánya miatt. Kommunikációs problémák a nagyon nehéz mert sok pontjai hiba megoldásához. Akkor mindig először győződjön meg arról, hogy a hálózati kapcsolat és Internet-hozzáférés elérhetők speciális hibaelhárítás megjelenítheti a továbblépés előtt. Portok és tűzfalbeállításokat információt megnyitásához [StorSimple virtuális tömb rendszerkövetelményei](storsimple-ova-system-requirements.md). Hibaelhárítási segítséget lépjen [a kapcsolat tesztelése parancsmagot a hibaelhárítás](storsimple-troubleshoot-deployment.md).

- **Teljesítménnyel kapcsolatos problémák** – az értesítések okoznak, amikor a rendszer nem hajt végre optimálisan, például amikor egy nagy terhelés alatt áll.

Ezeken kívül biztonsági frissítések és feladat hibák kapcsolatos riasztások jelenhet meg.

## <a name="alert-severity-levels"></a>Riasztási fontossági szintek

Értesítések különböző fontossági szintek, attól függően, hogy a hatást, amelyet a riasztási helyzet lesz, és az értesítésre válaszul kell rendelkeznie. A fontossági szintek a következők:

- **Kritikus** – Ez az értesítés van, a válasz, a rendszer sikeres teljesítményét gyengíti feltétel. Győződjön meg arról, hogy a szolgáltatás nem megszakad StorSimple művelet van szükség.

- **Figyelmeztetés** – Ez a feltétel válhat kritikus, ha nem szűnt meg. Érdemes vizsgálja meg a helyzet, és törölje a jelet a probléma szükséges műveleteket.

- **Információk** : Ez az értesítés, akkor lehet hasznos, nyomon követése és kezelése a rendszer az információkat tartalmaz.

## <a name="view-and-track-alerts"></a>Megtekintése és nyomon követése értesítések

A StorSimple kezelő szolgáltatás irányítópult az értesítések a virtuális eszközök, a rendezési szempont súlyosságát szint száma gyors áttekintést nyújt.

![Értesítések irányítópult](./media/storsimple-ova-manage-alerts/alerts6.png)

A súlyosságát szint gombra kattintva megnyílik az **értesítések** fülre. Az eredmények súlyosságát szintnek megfelelő értesítések tartalmazza.

![Értesítések jelentés riasztási típusúra változik](./media/storsimple-ova-manage-alerts/alerts7.png)

Értesítés a listában kattintson nyújt további információt az értesítésre vonatkozóan, többek között a legutóbbi jelentett az értesítésre, a figyelmeztetés az eszközön, és a javasolt lépéseket az értesítésre megoldani a előfordulásainak száma.

Másolhatja a figyelmeztetés adatait egy szövegfájlba Ha módosítani szeretné az adatokat küld a Microsoft Support. Az ajánlási követni és a rendszer a riasztási feltétel a helyszíni, törölje az értesítés, jelölje ki az értesítés az **értesítések** fülre, és **törölje a jelet**parancsra. Nagyszámú értesítés törléséhez jelölje ki a minden riasztás, kattintson az **értesítés** oszlop kivételével minden olyan oszlop, és majd miután kijelölte a törlendő minden riasztást, kattintson a **törölje a jelet** . Figyelje meg, hogy bizonyos értesítések automatikusan nincs bejelölve, ha a probléma megoldódott, vagy ha a rendszer frissíti a figyelmeztetés az új adatokkal.

Ha **törlése**gombra kattint, akkor lehetőséget nyújt az értesítést, és a lépéseket a probléma megoldásához beállításikon kapcsolatos megjegyzések jelzése. 

![figyelmeztető megjegyzés](./media/storsimple-ova-manage-alerts/clear-alert.png)

Kattintson az ellenőrzés ikon ![négyzet-ikon](./media/storsimple-ova-manage-alerts/check-icon.png) szeretné menteni a megjegyzéseit.

Bizonyos eseményeket a rendszer által törlődik, egy másik esemény akkor következik be, új információkkal. Ebben az esetben a következő üzenet jelenik meg.

![Törölje a jelet a figyelmeztető üzenet](./media/storsimple-ova-manage-alerts/alerts8.png)

## <a name="sort-and-review-alerts"></a>Rendezés és véleményezés értesítések

Legfeljebb 250 riasztások jeleníthet meg az **értesítések** fülre. Túllépte az értesítések telefonszámát, ha nem az összes figyelmeztetések jelennek meg az alapértelmezett nézetben. Ha testre szeretné szabni, hogy melyik figyelmeztetések jelennek meg az alábbi mezők kombinálhatja:

- **Állapot** – **aktív** vagy **nincs bejelölve** riasztások megjelenítésére. Aktív továbbra is éppen figyelmeztetéseket a számítógépén, miközben az üres riasztások már vagy törli kézzel rendszergazda vagy programozás útján nincs bejelölve, mivel a rendszer frissíti a riasztási feltétel új információkkal.

- **Súlyosságát** – megjelenítheti az összes fontossági szintek (kritikus, figyelmeztetés, információ), vagy csak egy bizonyos súlyosságát, például csak a kritikus értesítések értesítések.

- **Forrás** – értesítései különböző forrásokból származó vagy azokra a szolgáltatásra vagy a egy vagy összes virtuális eszközök érkező értesítések korlátozza.

- **Időtartomány** – a **Kezdő** és **Záró** dátum és idő bélyegzőt megadásával, tekinthetik meg a riasztások érdeklik időszakban.

## <a name="alerts-quick-reference"></a>Értesítések – rövid összefoglaló

Az alábbi táblázatokban felsoroljuk a Microsoft Azure StorSimple riasztások előforduló, és további információk és javaslatok rendelkezésre álló részét. StorSimple virtuális eszköz riasztások sorolhatók a következő kategóriák közül:

- [Felhőbeli kapcsolat értesítések](#cloud-connectivity-alerts)

- [Értesítések beállítása](#configuration-alerts)

- [Feladat figyelmeztetések](#job-failure-alerts)

- [Teljesítmény értesítések](#performance-alerts)

- [Biztonsági figyelmeztetések](#security-alerts)

- [Frissítési értesítések](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Felhőbeli kapcsolat értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Eszköz *<device name>* nem csatlakozik a felhőben.|Az elnevezett eszköz nem tud csatlakozni a felhőben. |Nem sikerült csatlakozni a felhőben. Ennek oka lehet az alábbiak egyikét:<ul><li>Előfordulhat, hogy a hálózati beállításokat az eszközön probléma.</li><li>Előfordulhat, hogy probléma a tárterület-fiók hitelesítő adatait.</li></ul>További információt a hibaelhárítás csatlakozási problémákat tapasztal lépjen a [helyi webes felület](storsimple-ova-web-ui-admin.md) az eszköz.|


### <a name="configuration-alerts"></a>Értesítések beállítása

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|A helyszíni virtuális eszköz konfiguráció nem támogatott.|Gyenge teljesítményt.|Az aktuális konfiguráció teljesítmény csökkenés vonhat. Győződjön meg arról, hogy a kiszolgáló megfelel a minimális konfiguráció. További információért lépjen [StorSimple virtuális tömb követelményeknek](storsimple-ova-system-requirements.md). 
|A <*eszköz neve*> futtatja kiépített lemezterületet ki.|Szabad lemezterület figyelmeztetés.|Kevés a kiépített lemezterület. Hogy tárterületet szabadítson fel, fontolja meg a munkaterhelésekből áthelyezése egy másik mennyiségi vagy hálózati megosztáshoz, vagy az adatok törlése lehetőséget.

### <a name="job-failure-alerts"></a>Feladat figyelmeztetések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Biztonsági másolat <*eszköz neve*> nem hajtható végre.|Biztonsági mentési feladat hiba.|Nem hozható létre a biztonsági másolatot. Fontolja meg az alábbiak egyikét:<ul><li>Csatlakozási problémákat tapasztal is megakadályozhatja a biztonsági mentés sikeres befejezését. Győződjön meg arról, hogy nincsenek-e kapcsolódási problémák. További tájékoztatást a kapcsolódási problémák elhárítása nyissa meg a [helyi webes felület](storsimple-ova-web-ui-admin.md) virtuális mobileszközére.</li><li>A rendelkezésre álló tárterületkorlát ért. Hogy tárterületet szabadítson fel, fontolja meg az összes biztonsági másolatot, amely már nem szükséges törlését.</li></ul> A probléma elhárításához, törölje a jelet az értesítés, és ismételje meg a műveletet.|
|Visszaállítás <*eszköz neve*> nem hajtható végre.|Vissza a feladat hiba.|Nem sikerült visszaállítása biztonsági másolatból. Fontolja meg az alábbiak egyikét:<ul><li>A biztonsági másolat lista nem lehet érvényes. Frissítse a listát, ellenőrizze, hogy továbbra is érvényes.</li><li>Csatlakozási problémákat tapasztal is megakadályozhatja a visszaállítási művelet sikeres befejezését. Győződjön meg arról, hogy nincsenek-e kapcsolódási problémák.</li><li>A rendelkezésre álló tárterületkorlát ért. Hogy tárterületet szabadítson fel, fontolja meg az összes biztonsági másolatot, amely már nem szükséges törlését.</li></ul>A probléma elhárításához, törölje a jelet az értesítés, és próbálkozzon újra a visszaállítási művelet.|
|<*Eszköz neve*> adatfeliratsor nem hajtható végre.|Feladat meghibásodása klónozhatja.|Nem hozható létre átirattal. Fontolja meg az alábbiak egyikét:<ul><li>A biztonsági másolat lista nem lehet érvényes. Frissítse a listát, ellenőrizze, hogy továbbra is érvényes.</li><li>Csatlakozási problémákat tapasztal is megakadályozhatja a adatfeliratsor művelet sikeres befejezését. Győződjön meg arról, hogy nincsenek-e kapcsolódási problémák.</li><li>A rendelkezésre álló tárterületkorlát ért. Hogy tárterületet szabadítson fel, fontolja meg az összes biztonsági másolatot, amely már nem szükséges törlését.</li></ul>A probléma elhárításához, törölje a jelet az értesítés, és ismételje meg a műveletet.|

### <a name="performance-alerts"></a>Teljesítmény értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Az adatátvitel váratlan késések tapasztalja.|Lassú adatátvitel.|Szabályozási hiba lép fel, ha egy művelet túllépi a tárhelyszolgáltatáshoz méretezhetőség céljainak. A tároló szolgáltatás végzi, annak érdekében, hogy nincs egyetlen ügyfél vagy egy bérlői költségére mások a szolgáltatást használhatja. További információt a hibaelhárítás Azure tárterület-fiókját lépjen a [Monitor, diagnosztizálása, és a Microsoft Azure tároló – problémamegoldás](storage-monitoring-diagnosing-troubleshooting.md).
|Kevés a helyi foglalás lemezterület <*eszköz neve*>.|Lassú válaszidő.|<*eszköz neve*> kiépített összesített méretét 10 %-át van fenntartva helyi az eszközre, és most kevés a foglalt adhatja meg. A terhelést a <*eszköz neve*> tejeskanna magasabb mértéke hoz létre, vagy előfordulhat, hogy a legutóbb áttelepítette nagy mennyiségű adatot. Ez a eredményezhet csökkentett teljesítmény. Tartsa szem előtt, megoldásához a következő műveletek egyikét:<ul><li>Ez az eszköz a felhőben sávszélességgel növelése</li><li>Csökkentése vagy munkaterhelésekből áthelyezése egy másik mennyiségi vagy megosztás.</li></ul>

### <a name="security-alerts"></a>Biztonsági figyelmeztetések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Jelszó <*eszköz neve*> <*szám*> nap múlva lejár.|Jelszót figyelmeztetés.| A jelszó lejár < szám < nap. Fontolja meg, hogy a jelszó módosításával. További információért lépjen [a StorSimple virtuális tömb eszközt rendszergazdai jelszó](storsimple-ova-change-device-admin-password.md)módosítása.

### <a name="update-alerts"></a>Frissítési értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Új frissítések keresése az eszközön érhetők el.|A StorSimple virtuális tömböt frissítések állnak rendelkezésre.|Új frissítések keresése a **karbantartása** lap a telepíthetők.|
|Nem sikerült a <*eszköz neve*> új frissítések keresése.|A frissítés sikertelen. |Hiba történt az új frissítések telepítése. A frissítések manuális telepítheti. Ha a probléma nem szűnik meg, lépjen kapcsolatba a [Microsoft-támogatást](storsimple-contact-microsoft-support.md).|


## <a name="next-steps"></a>Következő lépések

- [Tudnivalók a StorSimple virtuális tömb](storsimple-ova-overview.md).


