<properties
   pageTitle="A StorSimple kötet (U2) kezelése |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozzáadása, módosítása, figyelésére és StorSimple kötet törlése, illetve offline állapotba rájuk szükség."
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
   ms.date="10/28/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>A StorSimple kezelő szolgáltatás használatával kezelheti a kötet (frissítés 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan létrehozása és kezelése a StorSimple eszközön és StorSimple virtuális eszköz kötet frissítés 2 telepítve van a StorSimple kezelő szolgáltatás használatával.

A StorSimple kezelő szolgáltatás az Azure klasszikus portálon, amellyel a StorSimple megoldás kezelése az egyetlen webes felületet bővítménye. Nemcsak a kötet kezelése, a StorSimple kezelő szolgáltatás használatával létrehozása és StorSimple szolgáltatások kezelése, megtekintése és eszközök kezelése, megtekinteni az értesítéseket, és megtekintheti és biztonsági házirendek és a biztonságimásolat-katalógus kezelése.

## <a name="volume-types"></a>Mennyiségi típusai

StorSimple kötet lehet:

- **Helyi meghajtóra kiemelt kötet**: ezek mennyiségű adat megmarad, a helyi StorSimple eszközön mindig.
- **Tiered kötet**: ezek mennyiségű adat is spill a felhőbe.

Az archiválás hangereje egy többszintű mennyiségi típusú. Archiválás kötet használt deduplication adattömb méretének lehetővé teszi, hogy az eszköz nem ruházhatja át az adatok nagyobb szegmensek a felhőben. 

Ha szükséges, módosíthatja a mennyiségi típusú helyi többszintű vagy többszintű helyi. További információért lépjen [a mennyiségi típus](#change-the-volume-type)módosítása.

### <a name="locally-pinned-volumes"></a>Helyi meghajtóra rögzített kötet

Helyben rögzített kötet teljesen kiépített kötet, amelyek nem réteg adatokat a felhőben, ezáltal biztosítva a helyi garantálja az elsődleges adatokat, függetlenül a felhőbeli kapcsolat. A helyi meghajtóra rögzített kötet adatainak nem deduplicated és tömörített; azonban pillanatképek helyileg rögzített mennyiségének deduplicated vannak. 

Helyi meghajtóra rögzített kötet teljesen kiépített; Ezért kell rendelkeznie elég helyet az eszközön amikor létrehozza őket. 8 TB StorSimple 8100 az eszközre, és 20 TB 8600 az eszközre maximális mérete legfeljebb helyileg rögzített kötet is kiépítése. StorSimple fenntartja a fennmaradó helyi lemezterület pillanatképek, a metaadat-alapú és adatfeldolgozási az eszközön. A maximális rendelkezésére álló hely helyileg rögzített kötet méretét növelheti, de nem egyszer létrehozott kötet méretének csökkentheti.

Amikor létrehoz egy helyi rögzített mennyiségi, többszintű mennyiségének létrehozása a rendelkezésre álló hely csökken. A Fordított sorrend szintén igaz: Ha létező többszintű kötet, a hely a helyi meghajtóra létrehozása a kiemelt kötet kisebb, mint a maximális számával már említettük lesz. Helyi kötet kapcsolatos további tudnivalókért tekintse át a [Gyakori kérdések a helyi meghajtóra rögzített kötet](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Többszintű kötet

Többszintű kötet vékonyan kiépített kötet, amelyben a gyakran használt adatok maradjon helyi az eszközre, és kevesebb a gyakran használt adatot automatikusan többszintű a felhőbe. Vékony kiépítési, amelyen a rendelkezésre álló tárhely megjelenik haladja meg a fizikai erőforrások virtualizációs technológia. Előre lefoglalására elegendő tárhely, hanem StorSimple használja a vékony kiépítési lefoglalhat csak elég hely az aktuális követelményeknek. Felhőbeli tárhelyről rugalmas jellegét ezt a megközelítést megkönnyíti, mert StorSimple növelheti vagy csökkentheti a felhőbeli tárhelyről változó igényeinek kielégítéséhez.

A többszintű mennyiségi használatakor az archiválás adatokat **a mennyiségi kevesebb gyakran használt archiválás adatok használata** jelölőnégyzet bejelölésével deduplication adattömb méretének módosítása a kötet 512 KB. Ha nem jelöli be ezt a lehetőséget, a megfelelő többszintű hangerejének 64 KB adattömb mérete fogja használni. Deduplication adattömb nagyobb méretű lehetővé teszi, hogy az eszköz felgyorsításához nagy archiválás adatok továbbítása a felhőbe.

>[AZURE.NOTE] A frissítés előtti StorSimple 2 verziójával létrehozott archív kötet többszintű importálja lesz az archiválás jelölőnégyzet be van jelölve.

### <a name="provisioned-capacity"></a>Kiépített kapacitás

Olvassa el az alábbi táblázat az egyes eszköztől és a hangerő: maximális kiépített kapacitás. (Tájékoztatjuk, hogy helyileg rögzített kötet nem érhetők el a virtuális eszköz.)

|             | Többszintű kötet maximális mérete | A kiemelt kötet mérete legfeljebb helyileg |
|-------------|----------------------------|------------------------------------|
| **Fizikai eszközök** |       |       |
| 8100                 | 64 TB | 8 TB |
| 8600                 | 64 TB | 20 TB |
| **Virtuális eszközök**  |       |       |
| 8010                | 30 TB | A #HIÁNYZIK   |
| 8020               | 64 TB | A #HIÁNYZIK   |

## <a name="the-volumes-page"></a>A kötet lap

A **kötet** lap lehetővé teszi a tárhely kötet, amely a kezdeményezők (-kiszolgálók) a Microsoft Azure StorSimple eszközön kiépített kezelése. A lista mennyiségének StorSimple eszközén jeleníti meg.

 ![Kötet lap](./media/storsimple-manage-volumes-u2/VolumePage.png)

A mennyiségi attribútumok sorozata áll:

- **Mennyiségi neve** – egy leíró nevet, amely egyedinek kell lennie, és jobban azonosítsa a mennyiségi. Ez a név ellenőrzési jelentéseket, ha egy adott kötet szűrhet is szerepel.

- Lehet, hogy az **állapot** – online vagy offline. Ha egy mennyiségi offline, még nem áll rendelkezésre az access használatát a mennyiségi kezdeményezők (-kiszolgálók) látható.

- **Kapacitás** – Itt adhatja meg a kezdeményező (kiszolgáló) tárolt adatok mennyiségét. Helyi meghajtóra a kiemelt teljesen kiépített és StorSimple az eszközre találhatók. Többszintű kötet vékonyan kiépített környezetet, majd az adatok deduplicated van. A vékonyan kiépített kötet az eszköz nem előre lefoglalhat fizikai tárolókapacitású belső és a felhő megfelelően konfigurált mennyiségi kapacitása. A mennyiségi kapacitás kiosztva, és igény felhasznált.

- **Típus** – azt jelzi, hogy a kötet **Tiered** (alapértelmezett) vagy **helyben kiemelt**.

- **Biztonsági másolat** – azt jelzi, hogy létezik-e egy alapértelmezett biztonsági házirendet a kötet.

- **Access** – Itt adhatja meg, hogy a kötet elérésének engedélyezettek kezdeményezők (-kiszolgálók). A hangerőt, amelyek nem tagjai access vezérlő rekord (ACR) a mennyiségi társított kezdeményezők nem jelennek meg.

- **Figyelés** – adja meg a kötet ellenőrizni e vagy sem. A mennyiségi lesz figyelése annak létrehozásakor alapértelmezés szerint engedélyezve van. Lesz, de figyelése, a mennyiségi átirattal tiltható le. Ha engedélyezni szeretné a mennyiségi figyelése, kövesse a [Monitor mennyiségig](#monitor-a-volume). 

Ebben az oktatóprogramban az utasításokat segítségével hajtsa végre az alábbi műveleteket:

- A hangerő hozzáadása 
- A mennyiség módosítása 
- A mennyiségi típusának módosítása
- Kötet törlése 
- A mennyiségi offline állapotba 
- A mennyiségi figyelése 

## <a name="add-a-volume"></a>A hangerő hozzáadása

[A mennyiségi létrehozott](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) üzembe helyezése a StorSimple megoldás közben. A mennyiségi felvétele az alábbi hasonló eljárást.

#### <a name="to-add-a-volume"></a>A mennyiségi hozzáadása

1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tárolók** fülre.

2. Mennyiségi tároló válasszon a listából, és kattintson duplán a tároló társított kötet eléréséhez.

3. Kattintson a **Hozzáadás** az oldal alján. A Hozzáadás a mennyiségi varázsló elindul.

     ![Adja hozzá a mennyiségi varázsló alapvető beállításai](./media/storsimple-manage-volumes-u2/TieredVolEx.png)

4. A Hozzáadás a mennyiségi varázsló az **Alapvető beállítások**csoportban tegye a következőket:

  1. Adja meg a mennyiségi **nevét** .
  2. Jelölje ki a **Használati típusát** a legördülő listából. Az, hogy az adatok munkaterhelésekből érhető el az eszközön helyileg állandóan látszódjon, és válassza a **Helyi meghajtóra kiemelt**. Minden más típusú adatokat jelölje ki a **Tiered**. (**Tiered** az alapértelmezett érték.)
  3. Ha **Tiered** lépés: 2 lehetőséget választotta, választhat az archiválás kötet konfigurálása **a mennyiségi kevesebb gyakran használt archiválás adatok használata** jelölőnégyzetet.
  4. Írja be a mennyiségi GB vagy TB **Kiépítve kapacitás** . [Provisioned kapacitás](#provisioned-capacity) is találhat az egyes eszköztől és a hangerő: maximális méretét. Nézze meg a **Rendelkezésre álló kapacitás** meghatározása a tárhely ténylegesen érhető el az eszközön.

5. Kattintson a nyíl ikonra![Nyíl ikonra](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). A helyi meghajtóra rögzített mennyiségi állítja be, ha a következő üzenet jelenik meg.

    ![Mennyiségi típusú üzenetek módosítása](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
   
5. A nyílikonra ![nyílikonra](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)ismét a nyissa meg a **További beállítások** lapot.

    ![Mennyiségi varázsló további beállítások hozzáadása](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>

6. A **További beállítások**csoportban új rekord access vezérlő (ACR):
  
  1. A legördülő listából válassza ki a hozzáférési vezérlő rekord (ACR). Azt is megteheti egy új ACR is hozzáadhat. ACRs határozza meg, hogy mely állomások a kötet érheti el a fogadó IQN egyező együtt, amely szerepel a rekordot. Ha nem ad meg egy ACR, a következő üzenet jelenik meg.

        ![Adja meg a ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)

  2. Azt javasoljuk, hogy bejelölte az **alapértelmezett biztonsági másolatot készít a mennyiségi engedélyezése** jelölőnégyzetet.
  3. Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) a hangerő létrehozását a megadott beállítások.

Az új mennyiségi készen áll a használata.

>[AZURE.NOTE] Ha helyileg rögzített kötet létrehozása, és hozzon létre egy másik helyileg rögzített mennyiségi azonnal ezután feladat futtatása egymás után mennyiségi létrehozását. Az első mennyiségi létrehozási feladatot befejezés előtt a következő mennyiségi létrehozási feladat külön előkészületek nélkül használhatja.

## <a name="modify-a-volume"></a>A mennyiség módosítása

Kell nagyméretűre vagy a hosts, melyhez hozzáféréssel a mennyiség módosítása a mennyiség módosítása

> [AZURE.IMPORTANT] 
>
> - Ha módosítja a kötet méretét az eszközön, a kötet méretét kell az állomáson módosítható. 
> - A host mellett az itt ismertetett szereplő lépések az Windows Server 2012 (2012R2). Különböző lesz eljárásokkal Linux vagy egyéb host operációs rendszerek. Ha egy másik operációs rendszert futtató állomáson hangerejének módosítása tanulmányozza a host operációs rendszer utasításait. 

#### <a name="to-modify-a-volume"></a>A mennyiség módosítása

1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tárolók** fülre.

2. Mennyiségi tároló válasszon a listából, és kattintson duplán a tároló társított kötet megtekinthetik.

3. Jelölje ki a kötet, és a lap alján kattintson a **Módosítás**gombra. A mennyiség módosítása varázsló elindul.

4. A módosítás mennyiségi varázslóban, az **Alapvető beállítások**csoportban a következőket végezheti el:

  - Módosíthatja a **nevet**.
  - Konvertálás a **Felhasználás típusa** helyileg kiemelt való többszintű vagy a többszintű való helyileg kiemelt (lásd: további információt [a mennyiségi típus módosítása](#change-the-volume-type) ).
  - Növelje a **kapacitás kiépítve**. Csak növelhető, a **Kiépítéstől kapacitás** . A mennyiségi a létrehozása után nem lehet kisebb.

5. A **További beállítások**módosíthatja a ACR, feltéve, hogy a kötet offline állapotban. A hangerő online állapotban, ha szüksége lesz offline állapotba először. Keresse meg [a mennyiségi offline állapotba](#take-a-volume-offline) lépéseit a ACR módosítása előtt.

    > [AZURE.NOTE] A hangerő **alapértelmezett biztonsági engedélyezése** beállítás nem módosítható.

6. A módosítások mentéséhez kattintson az ellenőrzés ikon ![négyzet-ikon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Az Azure klasszikus portal egy frissítési mennyiségi üzenetet jeleníti meg. A sikeres üzenet jelenik meg, amikor a mennyiségi sikeresen megtörtént.

7. Ha a kötet vannak kibontása, hajtsa végre a host a Windows rendszerű számítógépén az alábbi lépéseket:

   1. Nyissa meg a **Számítógép-kezelés** ->**lemezre kezelése**.
   2. Kattintson a jobb gombbal a **Lemez kezelése** , és válassza a **Újraellenőrzése**.
   3. A lemezt a listában jelölje ki a módosított, kattintson a jobb gombbal, és jelölje be **Kötet kiterjesztése**. A mennyiségi kiterjesztése varázsló elindul. Kattintson a **Tovább**gombra.
   4. A varázsló, az alapértelmezett értékeket elfogadása. Miután a varázsló végzett, a mennyiségi meg kell jelennie a nagyobb méretű.

    >[AZURE.NOTE] Ha bontsa ki a helyi meghajtóra rögzített kötet, és bontsa ki egy másik helyileg kiemelt mennyiségi azonnal ezt követően a mennyiségi kiterjesztett feladatok egymás után futnak. Az első mennyiségi kiterjesztett feladatot befejezés előtt a következő mennyiségi kiterjesztésének feladattal külön előkészületek nélkül használhatja.

![A videó érhető el](./media/storsimple-manage-volumes-u2/Video_icon.png) **Videó érhető el**

Nézni egy videót, amely bemutatja, hogyan kattintva bontsa ki a kötet, kattintson [ide](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>A mennyiségi típusának módosítása

Módosíthatja a mennyiségi típusát a helyi meghajtóra kiemelt többszintű, vagy a helyi meghajtóra kiemelt való többszintű. A konvertálás azonban nem lehet a gyakori előfordulás. Az átalakítás okai való helyileg kiemelt többszintű a következők:

- Adatok elérhetősége és a teljesítmény helyi garanciát
- Felhőalapú késések és a felhő csatlakozási problémákat tapasztal kiszűrése.

Ezek rendszerint kis gyakran elérni kívánt létező kötet. A helyi meghajtóra rögzített mennyiségi teljesen kiépítve, után. A többszintű mennyiségi egy helyi rögzített mennyiségi konvertálni, StorSimple ellenőrzi, hogy elég helyet az eszközön az átalakítás megkezdése előtt. Ha nincs elég hely, hibaüzenetet kap, és a művelet törlődnek. 

> [AZURE.NOTE] A helyi meghajtóra kiemelt többszintű való átalakítás megkezdése előtt ellenőrizze a helyigény a többi munkaterhelésekből, érdemes megfontolni. 

Előfordulhat, hogy meg szeretné változtatni egy helyi rögzített mennyiségi többszintű kötet, ha a többi kötet kiépítése további helyre van szüksége. A helyi meghajtóra rögzített kötet többszintű, a végleges kapacitás méretét az eszköz nő a rendelkezésre álló kapacitás alakításakor. Csatlakozási problémákat tapasztal megakadályozhatja, hogy a kötet konvertálás a helyi típusból többszintű írja be ide, ha a helyi kötet a konvertálás befejeztéig többszintű kötet tulajdonságainak mutathat. Ennek oka az, néhány adatot előfordulhat, hogy rendelkezik kiömlött a felhőbe. Kiömlött adat továbbra is az eszközön, amely nem felszabadított, amíg a művelet indítani, és befejezése helyi lemezterület vezérlőelemmel.

>[AZURE.NOTE] A mennyiségi konvertálása időt vehet igénybe néhány, és nem lehet megszüntetni konverzió után induljon. A hangerő online marad a konverzió során és biztonsági másolatokat készíthet, de nem kibontása vagy visszaállítani a hangerőt, a konvertálás történő során.  

Alakítása a többszintű helyileg rögzített kötet negatív hatással lehet a Teljesítmény eszköz. Emellett az alábbi tényezőket növelheti a konvertálás befejezéséhez szükséges időt:

- Nincs elegendő a sávszélesség.

- Nincs aktuális biztonsági másolat.

Ha csökkenteni szeretné a e tényezők lehetnek:

- Tekintse át a sávszélesség-házirendek szabályozási, és győződjön meg arról, hogy egy dedikált 40 MB sávszélesség áll rendelkezésre.
- A konvertálás az essen ütemezése.
- A konvertálás megkezdése előtt, hogy a felhőben pillanatkép.

Ha több kötet (támogatása a különböző munkaterhelésekből) konvertálni, majd, előnyben részesíti a mennyiségi konvertálás, hogy nagyobb prioritás térfogat először alakulnak. Például érdemes alakíthatja kötet, amely a virtuális gépeken futó (VMs) vagy az SQL-munkaterhelésekből kötet a fájl megosztása munkaterhelésekből kötet konvertálása előtt.

#### <a name="to-change-the-volume-type"></a>A mennyiség módosítása

1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tárolók** fülre.

2. Mennyiségi tároló válasszon a listából, és kattintson duplán a tároló társított kötet megtekinthetik.

3. Jelölje ki a kötet, és a lap alján kattintson a **Módosítás**gombra. A mennyiség módosítása varázsló elindul.

4. Az **Alapvető beállítások** lapon módosíthatja a használatát írja be az új típus a **Felhasználás típusa** legördülő listából.

    - Ha a típus módosítja **a kiemelt helyi meghajtóra**, StorSimple ellenőrzi, van-e elegendő kapacitása.
    - Ha **Tiered** módosítja a típus, és ezt a hangerő-adatok archiválása lesz, jelölje be **a mennyiségi kevesebb gyakran használt archiválásához adatok használata** jelölőnégyzetet.

        ![Archiválás jelölőnégyzet](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)

5. A nyílikonra ![nyílikonra](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) a **További beállítások** lap megnyitásához. Ha egy helyi rögzített mennyiségi állítja be, a következő üzenet jelenik meg.

    ![Mennyiségi típusú üzenetek módosítása](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)

6. Kattintson a nyíl ikonra ![nyíl ikonra](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) ismét a Folytatás gombra.

7. Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) a folyamat megkezdéséhez. Az Azure portal egy frissítési mennyiségi üzenetet jeleníti meg. A sikeres üzenet jelenik meg, amikor a mennyiségi sikeresen megtörtént.

## <a name="take-a-volume-offline"></a>A mennyiségi offline állapotba

Előfordulhat, hogy a kötet offline állapotba helyezéséhez amikor módosítani vagy törölni szeretné. Amikor egy hangereje offline, azt nem áll rendelkezésre írási és olvasási hozzáférést. Szüksége lesz a mennyiségi offline állapotba, az állomáson, valamint az eszközön. 

#### <a name="to-take-a-volume-offline"></a>A mennyiségi offline állapotba

1. Győződjön meg róla, hogy a szóban forgó kötet nem használja, mielőtt offline állapotba.

2. Hogy a hangerő offline az állomáson első. Potenciális veszélyt adatsérülésekkel a kötet, így nem. Adott lépéseket tekintse át az operációs rendszer az utasításokat.

3. Után a fogadó kapcsolat nélküli üzemmódban, várni kell a mennyiségi offline az eszközön hajt végre az alábbi lépéseket:

  1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tárolók** fülre. A **Mennyiségi tárolók** lap az eszköz társított összes mennyiségi tárolók listája táblázatos formában.
  2. Jelölje ki a mennyiségi tároló, és kattintson rá a tárolóban lévő összes kötet listájának megjelenítéséhez.
  3. Jelölje ki a kötet, és kattintson az **offline állapotba**.
  4. Amikor a program megerősítést kér, kattintson az **Igen**gombra. A hangerő most már offline.

    Után a mennyiségi offline, elérhetővé válik a **Online állapotba** lehetőséget.

> [AZURE.NOTE] Az **Offline állapotba** parancs kérést küld a mennyiségi offline állapotba az eszközön. Hosts még használ a hangerőt, ha ennek eredménye megszakadt a kapcsolat, de a mennyiségi offline állapotúvá nem sikertelen lesz. 

## <a name="delete-a-volume"></a>Kötet törlése

> [AZURE.IMPORTANT] A mennyiségi törölheti csak akkor, ha a kapcsolat nélküli üzemmódban.

Hajtsa végre a következő kötet törlése.

#### <a name="to-delete-a-volume"></a>Kötet törlése

1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tárolók** fülre.

2. Jelölje be a mennyiségi tároló, amelyen a törölni kívánt kötet. Kattintson a hangerő tároló, amelyen az **kötet** .

3. Ez a tároló társított összes kötet táblázatos formában jelennek meg. Jelölje be a törölni kívánt kötet állapotát. Ha törölni szeretné a hangerőt nem offline, offline állapotba először [a mennyiségi offline állapotba](#take-a-volume-offline)ismertetett lépéseket követve.

4. A hangerő offline követően kattintson a **törli** az oldal alján.

5. Amikor a program megerősítést kér, kattintson az **Igen**gombra. A hangerő ekkor törlődik, és a **kötet** lapon megjelenik a frissített listáját a tárolóban kötet.

    >[AZURE.NOTE]Ha töröl egy helyi rögzített mennyiség, az új kötet rendelkezésre álló hely előfordulhat, hogy nem frissülnek azonnal. A StorSimple kezelő szolgáltatás rendszeres frissíti a helyi lemezterület. Javasoljuk, hogy várja meg néhány percet, mielőtt próbál létrehozni az új kötet.<br> Ezenkívül ha helyileg rögzített kötet törlése, és törölje egy másik helyileg kiemelt mennyiségi azonnal ezt követően a mennyiségi törlés feladat futtatása egymás után. Az első mennyiségi törlési művelet befejezés előtt a következő mennyiségi törlési művelet külön előkészületek nélkül használhatja.
 
## <a name="monitor-a-volume"></a>A mennyiségi figyelése

Mennyiségi figyelése lehetővé teszi a mennyiségi statisztika lehet/O – kapcsolódó gyűjtése. Figyelés az első 32 kötet létrehozott alapértelmezés szerint engedélyezve van. További mennyiségének figyelése alapértelmezés szerint nincs engedélyezve. Figyelés klónozott mennyiségének is alapértelmezés szerint nincs engedélyezve.

Hajtsa végre az alábbi lépések végrehajtásával engedélyezheti vagy letilthatja a mennyiségi figyelése.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Engedélyezése vagy letiltása a mennyiségi figyelése

1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tárolók** fülre.

2. Jelölje be a mennyiségi tároló, amelyben a mennyiségi található, és kattintson a a mennyiségi tároló a **kötet** lap eléréséhez.

3. A tároló társított összes kötet táblázatos felhasználói felületén jelennek meg. Kattintson a gombra, és válassza a mennyiségi vagy a mennyiségi adatfeliratsor.

4. A lap alján kattintson a **Módosítás**gombra.

5. A mennyiség módosítása varázsló az **Alapvető beállítások**csoportban jelölje be a **engedélyezése** vagy **letiltása** a **Figyelés** legördülő listából.

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan [adatfeliratsor StorSimple mennyiségig](storsimple-clone-volume.md).

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.

 
