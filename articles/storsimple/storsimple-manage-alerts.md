<properties 
   pageTitle="Megtekintheti és kezelheti a StorSimple riasztások |} Microsoft Azure"
   description="StorSimple riasztási feltételek és súlyosságát, értesítések konfigurálása és értesítések kezelése a StorSimple kezelő szolgáltatás használata ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>A StorSimple kezelő szolgáltatás használatával megtekintheti, és StorSimple értesítések kezelése

## <a name="overview"></a>– Áttekintés

Az **értesítések** fülre a StorSimple kezelő szolgáltatás teszi lehetővé, hogy tekintse át, és törölje a jelet a valós idejű alapon StorSimple eszköz – kapcsolatos értesítések. Ezen a lapon a központi figyelheti a Szolgáltatásállapot-hiba StorSimple eszközén, és a teljes StorSimple a Microsoft Azure megoldást.

Ebben az oktatóanyagban közös riasztási feltételek, riasztási fontossági szintek és értesítések konfigurálása ismerteti. Emellett figyelmeztető – rövid összefoglaló táblákat, amelyek lehetővé teszik, hogy gyorsan egy adott típusú keresse meg és megfelelően reagáljon tartalmazza.

![Az értesítések lapra](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Közös riasztási feltételek

Az StorSimple eszköz készít riasztások feltételek számos választ. A leggyakoribb riasztási feltételek a következők:

- **Hardveres problémák** – az értesítések mondani, a hardver az állapota. Segítségükkel tudja, hogy a frissítések belső vezérlőprogram van szükség, ha a hálózati kapcsolat problémákat, vagy ha a probléma az adatok meghajtók közül.

- **Csatlakozási problémákat tapasztal** – az értesítések fordulhat elő, ha van, akkor az adatok átvitele megnehezítheti. Kapcsolati is problémákat tapasztal az adatok átvitele során és az Azure tárterület-fiókjából vagy az eszközök és a StorSimple kezelő szolgáltatás közötti kapcsolatot hiánya miatt. Kommunikációs problémák a nagyon nehéz mert sok pontjai hiba megoldásához. Akkor mindig először győződjön meg arról, hogy a hálózati kapcsolat és Internet-hozzáférés elérhetők speciális hibaelhárítás megjelenítheti a továbblépés előtt. Hibaelhárítási segítséget lépjen [a kapcsolat tesztelése parancsmagot a hibaelhárítás](storsimple-troubleshoot-deployment.md).

- **Teljesítménnyel kapcsolatos problémák** – az értesítések okoznak, amikor a rendszer nem hajt végre optimálisan, például amikor egy nagy terhelés alatt áll.

Ezeken kívül biztonsági frissítések és feladat hibák kapcsolatos riasztások jelenhet meg.

## <a name="alert-severity-levels"></a>Riasztási fontossági szintek

Értesítések különböző fontossági szintek, attól függően, hogy a hatást, amelyet a riasztási helyzet lesz, és az értesítésre válaszul kell rendelkeznie. A fontossági szintek a következők:

- **Kritikus** – Ez az értesítés van, a válasz, a rendszer sikeres teljesítményét gyengíti feltétel. Győződjön meg arról, hogy a szolgáltatás nem megszakad StorSimple művelet van szükség.

- **Figyelmeztetés** – Ez a feltétel válhat kritikus, ha nem szűnt meg. Érdemes vizsgálja meg a helyzet, és törölje a jelet a probléma szükséges műveleteket.

- **Információk** : Ez az értesítés, akkor lehet hasznos, nyomon követése és kezelése a rendszer az információkat tartalmaz.

## <a name="configure-alert-settings"></a>Értesítési beállítások konfigurálása

Megadhatja, hogy szeretné-e értesítést e-mailek, az egyes StorSimple eszközről riasztási feltételek szerint. Továbbá azonosíthatja értesítés címzettnek az e-mail címüket beírja a **többi e-mail címzett** mezőben egymástól pontosvesszővel elválasztva.

>[AZURE.NOTE] Legfeljebb 20 e-mail címek egy eszköz adhat meg.

Miután engedélyezte egy eszközt az értesítő e-mailt, a értesítési listájára tagjai minden alkalommal, amikor egy kritikus riasztás e-mailt kap. Az üzeneteket küld a *storsimple-alerts-noreply@mail.windowsazure.com* , és leírja a riasztási feltételhez. A címzettek **leiratkozási** e-mail értesítési listájára maguk eltávolítása elemre.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Ahhoz, hogy az eszköz riasztásokat értesítő e-mailt

1. Nyissa meg a **eszközök** > **konfigurálása** az eszközhöz.

2. A **Riasztási beállítások**csoportban állítsa be a következőket:

    1. A **Küldés e-mailben értesítést** mezőben válassza az **Igen gombra**.

    2. Az **E-mail szolgáltatás-rendszergazdák** mezőben válassza az **Igen lehetőséget** , ha azt szeretné, hogy a szolgáltatás rendszergazdája, és minden további rendszergazdák fogadása az értesítéseket.

    3. A **többi e-mail címzett** mezőjébe írja be más az értesítések kapó címzettek e-mail címét. Írja be nevét a formátumban *someone@somewhere.com*. El az e-mail címeket egymástól pontosvesszővel. Beállíthatja, hogy legfeljebb 20 e-mail címek egy eszközt. 

        ![Értesítések értesítési beállításainak](./media/storsimple-manage-alerts/AlertNotify.png)

3. A próba értesítő e-mailt küldeni, kattintson a **próba-e-mail küldése**mellett lévő nyíl ikonra. A StorSimple kezelő szolgáltatás állapotüzenetek jeleníti meg, a próba-értesítés továbbítja. 

4. A következő üzenet jelenik meg, kattintson az **OK gombra**. 

    ![Értesítések küldött értesítő e-mailt tesztelése](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Ha a teszt értesítő üzenet nem lehet küldeni, akkor a StorSimple kezelő szolgáltatás egy megfelelő üzenetet jelenít meg. Kattintson az **OK gombra**, várjon néhány percet, és próbálkozzon ismét elküldeni a próba-értesítő üzenet. 

## <a name="view-and-track-alerts"></a>Megtekintése és nyomon követése értesítések

A StorSimple kezelő szolgáltatás irányítópult az értesítések az eszközein, a rendezési szempont súlyosságát szint száma gyors áttekintést nyújt.

![Értesítések irányítópult](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

A súlyosságát szint gombra kattintva megnyílik az **értesítések** fülre. Az eredmények súlyosságát szintnek megfelelő értesítések tartalmazza.

![Értesítések jelentés riasztási típusúra változik](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Értesítés a listában kattintson nyújt további információt az értesítésre vonatkozóan, többek között a legutóbbi jelentett az értesítésre, a figyelmeztetés az eszközön, és a javasolt lépéseket az értesítésre megoldani a előfordulásainak száma. Ha egy hardver-riasztás, is megjelöli a hardver összetevőt.

![Hardveres értesítés – példa](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Másolhatja a figyelmeztetés adatait egy szövegfájlba Ha módosítani szeretné az adatokat küld a Microsoft Support. Az ajánlási követni és a rendszer a riasztási feltétel a helyszíni, törölje az értesítés az eszközről, jelölje ki az értesítés az **értesítések** fülre, és **törölje a jelet**parancsra. Nagyszámú értesítés törléséhez jelölje ki a minden riasztás, kattintson az **értesítés** oszlop kivételével minden oszlopra, és majd miután kijelölte a törlendő minden riasztást, kattintson a **törölje a jelet** . Figyelje meg, hogy bizonyos értesítések automatikusan nincs bejelölve, ha a probléma megoldódott, vagy ha a rendszer frissíti a figyelmeztetés az új adatokkal.

Ha **törlése**gombra kattint, akkor lehetőséget nyújt az értesítést, és a lépéseket a probléma megoldásához beállításikon kapcsolatos megjegyzések jelzése. Bizonyos eseményeket a rendszer által törlődik, egy másik esemény akkor következik be, új információkkal. Ebben az esetben a következő üzenet jelenik meg.

![Törölje a jelet a figyelmeztető üzenet](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Rendezés és véleményezés értesítések

Szükség lehet arra hatékonyabb jelentések futtatása a riasztások, hogy tekintse át, és törölje őket a csoportokban. Emellett az **értesítések** fülre legfeljebb 250 riasztások megjelenítéséhez. Túllépte az értesítések telefonszámát, ha nem az összes figyelmeztetések jelennek meg az alapértelmezett nézetben. Ha testre szeretné szabni, hogy melyik figyelmeztetések jelennek meg az alábbi mezők kombinálhatja:

- **Állapot** – **aktív** vagy **nincs bejelölve** riasztások megjelenítésére. Aktív továbbra is éppen figyelmeztetéseket a számítógépén, miközben az üres riasztások már vagy törli kézzel rendszergazda vagy programozás útján nincs bejelölve, mivel a rendszer frissíti a riasztási feltétel új információkkal.

- **Súlyosságát** – megjelenítheti az összes fontossági szintek (kritikus, figyelmeztetés, információ), vagy csak egy bizonyos súlyosságát, például csak a kritikus értesítések értesítések.

- **Forrás** – értesítései különböző forrásokból származó vagy azokra, vagy a szolgáltatás vagy egy vagy összes eszköz érkező értesítések korlátozza.

- **Időtartomány** – a **Kezdő** és **Záró** dátum és idő bélyegzőt megadásával, tekinthetik meg a riasztások érdeklik időszakban.

## <a name="alerts-quick-reference"></a>Értesítések – rövid összefoglaló

Az alábbi táblázatokban felsoroljuk a Microsoft Azure StorSimple riasztások előforduló, és további információk és javaslatok rendelkezésre álló részét. StorSimple eszköz riasztások sorolhatók a következő kategóriák közül:

- [Felhőbeli kapcsolat értesítések](#cloud-connectivity-alerts)

- [Fürt értesítések](#cluster-alerts)

- [Katasztrófa helyreállítási értesítések](#disaster-recovery-alerts)

- [Hardveres értesítések](#hardware-alerts)

- [Feladat figyelmeztetések](#job-failure-alerts)

- [Helyi meghajtóra rögzített mennyiségi értesítések](#locally-pinned-volume-alerts)

- [Hálózati értesítések](#networking-alerts)

- [Teljesítmény értesítések](#performance-alerts)

- [Biztonsági figyelmeztetések](#security-alerts)

- [Csomag figyelmeztetéseinek](#support-package-alerts)

- [Frissítési értesítések](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Felhőbeli kapcsolat értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Nem hozható létre kapcsolat < a*felhőbeli hitelesítőadat neve*>.|Nem tud csatlakozni a tárterület-fiókot.|Úgy tűnik, előfordulhat, hogy a csatlakozási problémát az eszközhöz. Futtassa a `Test-HcsmConnection` parancsmag StorSimple a Windows PowerShell felületről az eszközön való alapján azonosíthatja és megoldhatja a problémát. A beállítások megfelelőek, ha a probléma, amelynek a figyelmeztető emelte tárterület-fiók hitelesítő lehet is. Ebben az esetben, használja a `Test-HcsStorageAccountCredential` parancsmag határozza meg, hogy vannak-e problémák megoldásához.<ul><li>Jelölje be a hálózati beállításokat.</li><li>Jelölje be a tárterület-fiókja hitelesítő adatait.</li></ul>|
|Azt nem kapott egy szívveréséhez az eszközről utolsó <*szám*> percre.|Nem tud csatlakozni az eszközt.|Úgy tűnik, a csatlakozási probléma a eszközzel. Használja a `Test-HcsmConnection` parancsmag StorSimple a Windows PowerShell felületről azonosítása és a probléma megoldása, vagy forduljon a rendszergazdához az eszközön.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Ha nem sikerül a felhőbeli kapcsolat StorSimple viselkedése

Mi történik, ha a felhőbeli kapcsolat nem sikerül az operációs rendszert futtató gyártási StorSimple eszközömön?

Ha felhőbeli kapcsolat nem sikerül az StorSimple gyártási eszközön, majd attól függően, hogy a állapotát az eszköz, az alábbi akkor fordulhat elő: 

- **A helyi adatok az eszközön**: egy ideig lesz nincs zavarok és olvasás továbbra is szerepelni fognak jeleníthető meg. Azonban nyitott IOs száma növekszik, és meghaladja a, az olvasás sikerült elindítani sikertelen lesz. 

    Attól függően, hogy az eszközön adatok mennyiségét a írások továbbra is az a felhő kapcsolódási fordulhat elő, az első néhány órát, a megszakítás után. Az írások majd csökkentheti, és a lyncnek elindíthatja a sikertelen, ha a felhőbeli kapcsolat megszakadt az órákat. (Nincs ideiglenes tárolási adatok számára, hogy a felhőbe kell nyomni az eszközön. Ezen a területen kiürül, ha az adatokat küldi. Ha nem sikerül kapcsolatot, a tárolás területen adatokat nem lehet tolni a felhőbe, és IO meghiúsul.)   

 
- **Az adatok a felhőben**: a legtöbb felhő kapcsolódási hibák, a függvény hibát ad vissza. Miután visszaállította a kapcsolatot, a az IOs anélkül, hogy a kötet online állapotba a felhasználó folytatja a munkát. Ritkán felhasználói beavatkozás is szükség lehet az Azure klasszikus portálról vissza online hangerejének megjelenítéséhez. 
 
- **Felhőalapú pillanatképek folyamatban**: A művelet megismétlése 4-5 órán belül néhány időpontok, és nem helyreáll a kapcsolat, ha a felhőben pillanatképek sikertelen lesz.


### <a name="cluster-alerts"></a>Fürt értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Az eszközt nem sikerült fölé <*eszköz neve*>.|Eszköz karbantartási módban van.|Eszköz fölé megadása vagy kilépés a karbantartási üzemmód miatt sikertelen volt. Ez a normál, és nincs teendő. Miután ez az értesítés van nyugtázza, törölje a jelet az értesítések lapról.|
|Az eszközt nem sikerült fölé <*eszköz neve*>.|A belső vezérlőprogram eszköz vagy a szoftver csak frissült.|Hiba történt következtében frissítéshez fürt feladatátvevő. Ez a normál, és nincs teendő. Miután ez az értesítés van nyugtázza, törölje a jelet az értesítések lapról.|
|Az eszközt nem sikerült fölé <*eszköz neve*>.|Vezérlő leállítása vagy újraindítása.|Az eszközt nem sikerült fölé, mert az aktív vezérlő leállítása vagy a rendszergazda indítani. Van szükség, nincs további teendő. Miután ez az értesítés van nyugtázza, törölje a jelet az értesítések lapról.|
|Az eszközt nem sikerült fölé <*eszköz neve*>.|Tervezett feladatátvétel.|Győződjön meg arról, hogy ez volt tervezett feladatátvevő. Után a megfelelő műveletet kell venni, törölje a jelet az értesítés az értesítések lapról.|
|Az eszközt nem sikerült fölé <*eszköz neve*>.|Tervezett feladatátvétel.|Tervezett feladatátadás automatikusan helyreállítása StorSimple épül. Ha nagyszámú az értesítések látható, lépjen kapcsolatba a Microsoft Support.|
|Az eszközt nem sikerült fölé <*eszköz neve*>.|Más/ismeretlen okát.|Ha nagyszámú az értesítések látható, lépjen kapcsolatba a Microsoft Support. Után a probléma megoldódott, törölje a jelet az értesítés az értesítések lapról.|
|A kritikus eszközök szolgáltatás állapota jelentések, ahogy sikertelen volt.|DataPath szolgáltatáshiba. |Kérjen segítséget a Microsoft Support.|
|A hálózati kapcsolat virtuális IP-cím <*adatok #*> állapot jelenti, ahogy sikertelen volt. |Más/ismeretlen okát. |Előfordul, hogy ideiglenes feltételek okozhat az értesítések. Ha ez így, majd az értesítés automatikusan törlődnek után egy kis időt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support.|
|A hálózati kapcsolat virtuális IP-cím <*adatok #*> állapot jelenti, ahogy sikertelen volt.|Kapcsolat neve: <*adatok #*> IP-cím <IP address> nem lehet online állapotba, mert egy ismétlődő IP-cím észlelt a hálózaton. |Ellenőrizze, hogy, hogy az ismétlődő IP-cím a hálózat törlődik, vagy a felületen egy másik IP-címet konfigurálni.|


### <a name="disaster-recovery-alerts"></a>Katasztrófa helyreállítási értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Helyreállítási műveletek minden ehhez a szolgáltatáshoz beállítás nem állítható vissza. Eszköz konfigurációs adatok egyes eszközök esetén nem következetes állapotban van.|Adatok ellentmondás észlelt vészhelyreállítás után.|Titkosított adatokat a szolgáltatás nem szinkronizálja a rendszer az, hogy az eszközön. Engedélyezi az eszköz <*eszköz neve*> StorSimple Manager alkalmazásból kattintva indítsa el a szinkronizálást. Használja a Windows PowerShell felülete StorSimple futtatásához a `Restore-HcsmEncryptedServiceData` eszköz <*eszköz neve*> parancsmag, kattintson a régi jelszó megadása egy bemeneteként ezzel a parancsmaggal a biztonsági profilt visszaállítása. Futtassa a `Invoke-HcsmServiceDataEncryptionKeyChange` parancsmag adatok titkosítókulcs frissítéséhez. Után a megfelelő műveletet kell venni, törölje a jelet az értesítés az értesítések lapról.|


### <a name="hardware-alerts"></a>Hardveres értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Hardveres összetevő <*összetevő-azonosító*> Jelentések állapota <*állapot*>.||Előfordul, hogy ideiglenes feltételek okozhat az értesítések. Ha igen, ez az értesítés automatikusan törlődnek után egy kis időt. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support.|
|Passive vezérlő hibásan működik.|A passzív (másodlagos) vezérlő nem működik.|Az eszköz működik, de a vezérlőket hibásan működik. Indítsa el, hogy a vezérlő. Ha a probléma továbbra is fennáll, lépjen kapcsolatba a Microsoft Support.|

### <a name="job-failure-alerts"></a>Feladat figyelmeztetések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Biztonsági másolat <*forrás mennyiségi Csoportazonosító*> sikertelen volt.|Biztonsági mentési feladat sikertelen volt.|Csatlakozási problémákat tapasztal is megakadályozhatja a biztonsági mentés sikeres befejezését. Ha nincs csatlakozási problémákat tapasztal, előfordulhat, hogy elérte biztonsági másolatok maximális száma. Törölje a minden olyan biztonsági másolatokat, amely már nincs szükség, és ismételje meg a műveletet. Után a megfelelő műveletet kell venni, törölje a jelet az értesítés az értesítések lapról.|
|<*Cél mennyiségi dátumértékeket*> <*biztonsági adatforráselem azonosítók*> adatfeliratsor sikertelen volt.|Nem sikerült adatfeliratsor feladatot.|Frissítse a biztonsági másolat listában, és győződjön meg arról, hogy a biztonsági másolat is érvényes. Ha a biztonsági mentés érvényes, akkor lehet, hogy felhő kapcsolódási problémák léptek fel az adatfeliratsor művelet sikeres befejezését. Ha nincs csatlakozási problémákat tapasztal, előfordulhat, hogy elérte méretkorlátot. Törölje a minden olyan biztonsági másolatokat, amely már nincs szükség, és ismételje meg a műveletet. Miután végzett megfelelő lépéseket a probléma megoldásához, törölje a jelet az értesítés az értesítések lapról.|
|Nem sikerült <*biztonsági adatforráselem azonosítók*> visszaállítani.|Nem sikerült feladat visszaállítása.|Frissítse a biztonsági másolat listában, és győződjön meg arról, hogy a biztonsági másolat is érvényes. Ha a biztonsági mentés érvényes, akkor lehet, hogy felhő kapcsolódási problémák léptek fel a visszaállítási művelet sikeres befejezését. Ha nincs csatlakozási problémákat tapasztal, előfordulhat, hogy elérte méretkorlátot. Törölje a minden olyan biztonsági másolatokat, amely már nincs szükség, és ismételje meg a műveletet. Miután végzett megfelelő lépéseket a probléma megoldásához, törölje a jelet az értesítés az értesítések lapról.|

### <a name="locally-pinned-volume-alerts"></a>Helyi meghajtóra rögzített mennyiségi értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|A helyi mennyiségi <*mennyiségi neve*> létrehozása sikertelen volt.| Nem sikerült a mennyiségi létrehozási feladatot. <*Hibaüzenet a hibás hibakód megfelelő üzenet*>.|Csatlakozási problémákat tapasztal is megakadályozhatja a hely létrehozása művelet sikeres befejezését. Helyben rögzített kötet thickly kiépített környezetet és térköz létrehozásának folyamata magában foglalja a felhőbe többszintű kötet kiömlést. Ha nincs csatlakozási problémákat tapasztal, előfordulhat, hogy van fogy a helyi helyet az eszközön. Határozza meg, ha terület megtalálható az eszköz újraküldése Ez a művelet előtt.|
|Helyi mennyiségi <*mennyiségi neve*> kiterjesztett sikertelen volt.|A mennyiség módosítása feladat <*hibaüzenet a hibás hibakód megfelelő üzenet*> miatt nem sikerült.|Csatlakozási problémákat tapasztal is megakadályozhatja a mennyiségi bővítési művelet sikeres befejezését. Helyi rögzített kötet thickly kiépített környezetet és a folyamaton, a meglévő terület bővítése magában foglalja a felhőbe többszintű kötet kiömlést. Ha nincs csatlakozási problémákat tapasztal, előfordulhat, hogy van fogy a helyi helyet az eszközön. Határozza meg a hely létezésének az eszközön előtt, hogy újra próbálkozik ezt a műveletet.|
|Mennyiségi <*mennyiségi neve*> átalakítás sikertelen volt.|A mennyiségi átalakítási feladat a mennyiségi típusát a helyi meghajtóra konvertálása rögzített többszintű sikertelen.|A hangerő konvertálása helyi rögzített típusból többszintű nem lehet befejeződött. Győződjön meg arról, hogy nincsenek-e a művelet sikeresen befejeződött kapcsolódási problémák. Hibaelhárítási kapcsolódási problémák [elhárítása a próba-HcsmConnection parancsmagot a](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet)megnyitásához.<br>Az eredeti helyileg rögzített kötet most van megjelölve többszintű kötet, mivel a helyi meghajtóra rögzített mennyiségi adatainak egy része van kiömlött a felhőbe a konverzió során. A többszintű eredő mennyiségi továbbra is van betöltő helyi lemezterület jövőbeli helyi kötet nem nyerhető eszközre.<br>Esetleges kapcsolódási problémák megoldásához, törölje a jelet az értesítés, és a hangerő alakíthatja vissza az eredeti helyileg rögzített mennyiségi típus biztosítja az adatok elérhetővé válik a helyi meghajtóra újra az összes.|
|Mennyiségi <*mennyiségi neve*> átalakítás sikertelen volt.|A mennyiségi átalakítási feladat szeretné alakítani a mennyiségi típusát a helyi meghajtóra kiemelt többszintű sikertelen volt.|Írja be a kötet konverzió többszintű való helyileg kiemelt nem hajtható végre. Győződjön meg arról, hogy nincsenek-e a művelet sikeresen befejeződött kapcsolódási problémák. Hibaelhárítási kapcsolódási problémák [elhárítása a próba-HcsmConnection parancsmagot a](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet)megnyitásához.<br>Az eredeti többszintű kötet, most egy helyi rögzített mennyiségi megjelölve, a konverzió során továbbra is fennáll, tartózkodó a felhőben, miközben a thickly kiépített helyet a kötet az eszközön nem is visszaigényelt a jövőbeli helyi kötet adatot.<br>Esetleges kapcsolódási problémák megoldásához, törölje a jelet az értesítés, és a hangerő alakíthatja vissza az eredeti többszintű mennyiségi típus annak érdekében, hogy az eszközön thickly kiépítve helyi lemezterület is nyerhető.|
|Helyi lemezterület-felhasználás helyi elérhető információk, amelyek az közelítő <*mennyiségi csoport neve*>|A biztonsági másolat házirend helyi állapotaira amint lehet, hogy fogyni a helyet, és érvényteleníti host írási hibák elkerülése érdekében.|Gyakori helyi pillanatképek mellett egy nagy adatok tejeskanna a mennyiségben a biztonsági másolat házirend csoporthoz társított okozzák a helyi lemezterület gyorsan felhasználandó az eszközön. Bármely, a már nincs szükség helyi pillanatképek törlése. A helyi pillanatkép ütemtervek a biztonsági másolat házirend kevésbé gyakori helyi pillanatképek, és győződjön meg róla, felhőalapú pillanatképek rendszeresen hoznak is frissíteni. Ha ezek a műveletek nem figyelembe venni, ezek pillanatképek helyi lemezterület amint lehet, hogy merülnek, és a rendszer automatikusan törli őket, annak érdekében, hogy host írások is importálni.|
|Helyi pillanatképek <*mennyiségi csoportnévre*> érvénytelenné van vált.|A helyi pillanatképek <*mennyiségi csoportnévre*> érvényét, és aztán törölt, mert azok voltak meghaladja a helyi helyet az eszközön.|Annak érdekében, hogy ez nem ismétlődő kapcsolatban, olvassa el ezt a házirendet helyi pillanatkép ütemtervét, és törölje a bármely, a már nincs szükség helyi pillanatképek. Gyakori helyi pillanatképek mellett egy nagy adatok tejeskanna a mennyiségben a biztonsági másolat házirend csoporthoz társított okozhat a helyi lemezterület gyorsan felhasználandó az eszközön.|
|Nem sikerült <*biztonsági adatforráselem lehetővé tevő dokumentumazonosítók*> visszaállítani.|Nem sikerült a visszaállítási feladat.|Van helyileg kiemelt vagy a biztonsági másolat házirend helyileg rögzített és többszintű mennyiségének kombinálja frissítése a biztonsági másolat listában, és ellenőrizze, hogy a biztonsági mentés esetén továbbra is érvényes. Ha a biztonsági mentés érvényes, akkor lehet, hogy felhő kapcsolódási problémák léptek fel a visszaállítási művelet sikeres befejezését. A helyi meghajtóra rögzített kötet részeként ebben a csoportban pillanatkép helyreállított éppen nem rendelkezik az összes adataik letöltése az eszközre, és ebben a csoportban pillanatkép Ha kombinálja a többszintű és helyileg rögzített kötet, nem lesznek a többi szinkronizál. A visszaállítás sikeres befejezéséhez kötet készíthet ebben a csoportban offline az állomáson, és próbálkozzon újra a visszaállítási művelet. Figyelje meg, hogy a kötet adatait a visszaállítás során végrehajtott módosításai elvesznek.|

### <a name="networking-alerts"></a>Hálózati értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Nem indítható StorSimple-ra.|DataPath hiba |Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support.|
|Ismétlődő IP-címe "Data0" észlelni.| |A rendszer talált egy ütközést "10.0.0.1" IP-címet. A hálózati erőforrás "Data0" az eszközön *<device1>* offline állapotban. Győződjön meg arról, hogy a hálózat minden más személy nem használja az IP-cím. Hálózati kapcsolatos problémák megoldása, keresse fel [a Get-NetAdapter parancsmagot a hibaelhárítás](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Probléma megoldásához forduljon a hálózati rendszergazda. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support. |
|Kapcsolat nélküli üzemmódban "Data0" IPv4 (vagy IPv6-) címét.| |A hálózati erőforrás "Data0" IP-címet "10.0.0.1." előtag hossza '22' az eszközre, és *<device1>* offline állapotban. Győződjön meg arról, hogy a kapcsoló, amelyhez csatlakozott a kapcsolat legyen műveleti. Hálózati kapcsolatos problémák megoldása, keresse fel [a Get-NetAdapter parancsmagot a hibaelhárítás](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
 

### <a name="performance-alerts"></a>Teljesítmény értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|Az eszköz terhelés túllépte <*küszöb*>.|Várható válaszidő lassabb.|Az eszköz jelentések kihasználtsági bemeneti és kimeneti nagy terhelés alatt. Ez az eszköz nem működnek, valamint azt okozhat. Tekintse át a feladatok, az eszköz van csatolva, és határozza meg, ha vannak, amelyek sikerült helyezhetők át egy másik eszközre, vagy, amely már nem szükséges.<br>Az aktuális állapot megértéséhez, nyissa meg [az eszköz figyelheti a StorSimple Manager szolgáltatással](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Biztonsági figyelmeztetések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|A Microsoft Support munkamenete megkezdődött.|Harmadik fél támogatási munkamenethez érhetők el.|Erősítse meg, a hozzáférés engedélyezése. Után a megfelelő műveletet kell venni, törölje a jelet az értesítés az értesítések lapról.|
|A jelszó <*elem*> <*idő hossza*> lejár.|Jelszó lejáratának közelében van.|A jelszó módosítása, még mielőtt lejárna.|
|Hiányzó <*elem azonosítója*> biztonsági beállításokat.||Ez a mennyiségi tároló társított kötet való replikáció StorSimple beállításoktól nem használhatók. Győződjön meg arról, hogy az adatok biztonságos tárolja, azt javasoljuk, hogy a kötet tároló és bármely a mennyiségi tároló társított kötet törlése. Után a megfelelő műveletet kell venni, törölje a jelet az értesítés az értesítések lapról.|
|<*szám*> <*elem azonosítója*> sikertelen bejelentkezések.|Több sikertelen bejelentkezési kísérletet tesz.|Az eszköz lehet, hogy a homonimaszerű webcímmel vagy egy engedélyezett felhasználói kapcsolatba lépni egy hibás jelszómegadás próbál.<ul><li>Lépjen kapcsolatba a jogosult felhasználók, és ellenőrizze, hogy a következő kísérletek megbízható forrásból volt. Ha továbbra is megjelenik a sikertelen bejelentkezések nagyszámú, fontolja meg a Távfelügyelet letiltása és a hálózati rendszergazda. Után a megfelelő műveletet kell venni, törölje a jelet az értesítés az értesítések lapról.</li><li>Ellenőrizze, hogy a helyes jelszót a pillanatkép Manager példányok vannak beállítva. Után a megfelelő műveletet kell venni, törölje a jelet az értesítés az értesítések lapról.</li></ul>További információért lépjen [egy lejárt eszköz jelszó módosítása](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password).|
|Egy vagy több hiba történt a adatok titkosítókulcs módosítása közben.||Hiba történt a szolgáltatás adatok titkosítási kulcs módosítása közben. Miután hiba feltételek van címezve, futtassa a `Invoke-HcsmServiceDataEncryptionKeyChange` parancsmag StorSimple a Windows PowerShell felületről frissíteni a szolgáltatást az eszközön. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft-támogatást. Miután ez az értesítés az értesítések lapról oldja meg a problémát, törölje a jelet.|

### <a name="support-package-alerts"></a>Csomag figyelmeztetéseinek

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|A támogatás csomag létrehozása sikertelen volt.|StorSimple nem sikerült létrehozni a csomagot.|Ismételje meg ezt a műveletet. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support. Után a probléma megoldódott, törölje a jelet az értesítés az értesítések lapról.|

### <a name="update-alerts"></a>Frissítési értesítések

|Értesítés szöveges|Esemény|További információ / javasolt műveletek|
|:---|:---|:---|
|A gyorsjavítás.|A belső vezérlőprogram szoftver/frissítése kész.|A javítás sikeresen telepítve az eszközön.|
|Kézi frissítés érhető el.|Az elérhető frissítések értesítés.|Használatával a Windows PowerShell-felület StorSimple az eszközén a frissítések telepítéséhez. <br>További információért lépjen [a StorSimple 8000 sorozat eszköz frissítése](storsimple-update-device.md).|
|Új frissítések érhető el.|Az elérhető frissítések értesítés.|Telepítheti az alábbi frissítéseket a **karbantartása** lap vagy a Windows PowerShell-felület használatával StorSimple az eszközön. <br>További információért lépjen [a StorSimple 8000 sorozat eszköz frissítése](storsimple-update-device.md).|
|Frissítések telepítése nem sikerült.|Frissítések nem sikerült telepíteni.|A rendszer nem a frissítések telepítéséhez. Telepítheti az alábbi frissítéseket a **karbantartása** lap vagy a Windows PowerShell-felület használatával StorSimple az eszközön. Ha a probléma nem szűnik meg, lépjen kapcsolatba a Microsoft Support. <br>További információért lépjen [a StorSimple 8000 sorozat eszköz frissítése](storsimple-update-device.md).|
|Nem lehet automatikusan új frissítések keresése.|Az automatikus sikertelen volt.|Új frissítések keresése a **Karbantartás** lapról manuális ellenőrzése.|
|Elérhető a új WUA ügynök.|Értesítés elérhető frissítés.|Töltse le a Windows Update Agent legújabb verzióját, és telepítse a Windows PowerShell felületről.|
|Belső vezérlőprogram összetevő <*összetevő-azonosító*> verziója nem egyezik meg a hardverrel való.|Belső vezérlőprogram frissítés nem sikerült telepíteni.|Lépjen kapcsolatba a Microsoft-támogatást.|

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [StorSimple hibák és műveleti eszköz hibaelhárítása](storsimple-troubleshoot-operational-device.md).

