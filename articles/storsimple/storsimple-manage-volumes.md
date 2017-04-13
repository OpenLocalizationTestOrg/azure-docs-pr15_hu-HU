<properties
   pageTitle="A StorSimple kötet kezelése |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozzáadása, módosítása, figyelésére és StorSimple kötet törlése, illetve offline állapotba rájuk szükség."
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
   ms.workload="TBD"
   ms.date="05/11/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>A StorSimple kezelő szolgáltatás használatával kötet kezelése

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan a StorSimple kezelő szolgáltatás használatával létrehozhatja és kezelheti a StorSimple eszközön és StorSimple virtuális eszköz kötet.

A StorSimple kezelő szolgáltatás az Azure klasszikus portálon, amellyel a StorSimple megoldás kezelése az egyetlen webes felületet kiterjesztését. Nemcsak a kötet kezelése, a StorSimple kezelő szolgáltatás használatával létrehozása és StorSimple szolgáltatások kezelése, megtekintése és eszközök kezelése, megtekinteni az értesítéseket, és megtekintheti és biztonsági házirendek és a biztonságimásolat-katalógus kezelése.

> [AZURE.NOTE] Azure StorSimple csak a vékonyan kiépített kötet hozhat létre. Nem hozhatók létre kiépített teljesen vagy részben kiépített kötet az Azure StorSimple rendszeren.
>
> Vékony kiépítési, amelyen a rendelkezésre álló tárhely megjelenik haladja meg a fizikai erőforrások virtualizációs technológia. Előre lefoglalására elegendő tárhely, hanem Azure StorSimple használja a vékony kiépítési lefoglalhat csak elég hely az aktuális követelményeknek. Felhőbeli tárhelyről rugalmas jellegét ezt a megközelítést megkönnyíti, mivel az Azure StorSimple növelheti vagy csökkentheti a felhőbeli tárhelyről változó igényeinek kielégítéséhez.

## <a name="the-volumes-page"></a>A kötet lap

A **kötet** lap lehetővé teszi a tárhely kötet, amely a kezdeményezők (-kiszolgálók) a Microsoft Azure StorSimple eszközön kiépített kezelése. A lista mennyiségének StorSimple eszközén jeleníti meg.

 ![Kötet lap](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

A mennyiségi attribútumok sorozata áll:

- **Név** – egyedinek kell lennie, és a hangerő azonosíthatja kifejező nevet. Ez a név ellenőrzési jelentéseket, ha egy adott kötet szűrhet is szerepel.

- Lehet, hogy az **állapot** – online vagy offline. Ha egy kötet, ha a kapcsolat nélküli, még nem áll rendelkezésre az access használatát a mennyiségi kezdeményezők (-kiszolgálók) látható.

- **Kapacitás** – határozza meg a mennyiségi mekkora, észlelt a kezdeményező (kiszolgáló) szerint. Kapacitás adja meg a kezdeményező (kiszolgáló) tárolt adatok mennyiségét. Kötet vékonyan kiépített környezetet, és az adatok deduplicated van. Ez azt jelenti, hogy az eszköz nem előre lefoglalhat fizikai tárolókapacitással rendelkezik, belső és a felhő megfelelően konfigurált mennyiségi kapacitása. A mennyiségi kapacitás kiosztva, és igény felhasznált.

- **Típus** – lehet, hogy a kötet típusa többszintű vagy archivált (egy alszint típusú többszintű)

- **Access** – Itt adhatja meg, hogy a kötet elérésének engedélyezettek kezdeményezők (-kiszolgálók). A hangerőt, amelyek nem tagjai access vezérlő rekord (ACR) a mennyiségi társított kezdeményezők nem jelennek meg.

- **Figyelés** – adja meg a kötet ellenőrizni e vagy sem. A mennyiségi lesz figyelése annak létrehozásakor alapértelmezés szerint engedélyezve van. Lesz, de figyelése, a mennyiségi átirattal tiltható le. Ha engedélyezni szeretné a mennyiségi figyelése, kövesse a Monitor mennyiségig.

A leggyakoribb feladatok a mennyiségi társított a következők:

- A hangerő hozzáadása
- A mennyiség módosítása
- Kötet törlése
- A mennyiségi offline állapotba
- A mennyiségi figyelése

## <a name="add-a-volume"></a>A hangerő hozzáadása

[A mennyiségi létrehozott](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) üzembe helyezése a StorSimple megoldás közben. A mennyiségi felvétele az alábbi hasonló eljárást.

### <a name="to-add-a-volume"></a>A mennyiségi hozzáadása

1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tárolók** fülre.

2. Jelölje ki a mennyiségi tároló, és kattintson a nyílra a tároló társított kötet elérése a megfelelő sor.

3. Kattintson a **Hozzáadás** az oldal alján. A Hozzáadás a mennyiségi varázsló elindul.

     ![Adja hozzá a mennyiségi varázsló alapvető beállításai](./media/storsimple-manage-volumes/AddVolume1.png)

4. A Hozzáadás a mennyiségi varázsló az **Alapvető beállítások**csoportban tegye a következőket:

  1. Adja meg a mennyiségi **nevét** .
  2. Adja meg a mennyiségi **Kiépítéstől kapacitás** GB vagy TB-ot. Fizikai eszközök kapacitása 1 GB és a 64 TB között kell lennie. A maximális, amely egy virtuális StorSimple eszközön kötet kiépítése kapacitása 30 TB mennyiségben.
  3. Jelölje be a mennyiségi **Használati típusát** . A többszintű mennyiségi használatakor az archiválás adatokat **a mennyiségi kevesebb gyakran használt archiválásához adatok használata** jelölőnégyzet bejelölésével deduplication adattömb méretének módosítása a kötet 512 KB. Ha nem jelöli be ezt a beállítást, a megfelelő többszintű kötet 64 KB adattömb mérete használja. Deduplication adattömb nagyobb méretű lehetővé teszi, hogy az eszköz felgyorsításához nagy archiválás adatok továbbítása a felhőbe. (Többszintű kötet voltak korábbi néven elsődleges kötet.)
  5. A nyílikonra ![nyílikonra](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)a **További beállítások** lap megnyitásához.

        ![Mennyiségi varázsló további beállítások hozzáadása](./media/storsimple-manage-volumes/AddVolume2.png)

5. A **További beállítások**csoportban új rekord access vezérlő (ACR):

  1. A legördülő listából válassza ki a hozzáférési vezérlő rekord (ACR). Azt is megteheti egy új ACR is hozzáadhat. ACRs határozza meg, hogy mely állomások a kötet érheti el a fogadó IQN egyező együtt, amely szerepel a rekordot.
  2. Azt javasoljuk, hogy engedélyezze az alapértelmezett biztonsági másolatot az **alapértelmezett biztonsági másolatot készít a mennyiségi engedélyezése** jelölőnégyzet bejelölésével.
   3. Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-manage-volumes/HCS_CheckIcon.png) a hangerő létrehozását a megadott beállítások.

Az új mennyiségi készen áll a használata.

## <a name="modify-a-volume"></a>A mennyiség módosítása

Kell nagyméretűre vagy a hosts, melyhez hozzáféréssel a mennyiség módosítása a mennyiség módosítása

> [AZURE.IMPORTANT]
>
> - Ha módosítja a kötet méretét az eszközön, a kötet méretét kell az állomáson módosítható.
> - A host mellett az itt ismertetett szereplő lépések az Windows Server 2012 (2012R2). Különböző lesz eljárásokkal Linux vagy egyéb host operációs rendszerek. Ha egy másik operációs rendszert futtató állomáson hangerejének módosítása tanulmányozza a host operációs rendszer utasításait.

### <a name="to-modify-a-volume"></a>A mennyiség módosítása

1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tároló** fülre. Ezen az oldalon az eszköz társított összes mennyiségi tárolók listája táblázatos formában.

2. Jelölje ki a mennyiségi tároló, és kattintson rá a tárolóban lévő összes kötet listájának megjelenítéséhez.

3. A **kötet** lapon jelölje ki a kötet, és kattintson a **Módosítás**gombra.

4. A módosítás mennyiségi varázslóban, az **Alapvető beállítások**csoportban a következőket végezheti el:

  - A **név** és **típus** szerkesztése, ha másnak szeretné módosítani az archiválás kötet többszintű kötet az arány deduplication adattömb a kötet 512 KB **a mennyiségi kevesebb gyakran használt archiválás adatok használata** jelölőnégyzet bejelölésével.
  - Növelje a **kapacitás kiépítve**. Csak növelhető, a **Kiépítéstől kapacitás** . A mennyiségi a létrehozása után nem lehet kisebb.

    > [AZURE.NOTE] A mennyiségi tároló után hozzá van rendelve egy mennyiségi nem módosítható.

5. A **További beállítások**csoportban a következőket végezheti el:

  - A ACRs, módosítsa, feltéve, hogy a kötet offline állapotban. A hangerő online állapotban, ha szüksége lesz offline állapotba először. Keresse meg [a mennyiségi offline állapotba](#take-a-volume-offline) lépéseit a ACR módosítása előtt.
  - ACRs listájának módosítása után a mennyiségi offline állapotban.

    > [AZURE.NOTE] A kötet **alapértelmezett biztonsági másolatot készít a mennyiségi engedélyezése** beállítás nem módosítható.

6. A módosítások mentéséhez kattintson az ellenőrzés ikon ![négyzet-ikon](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Az Azure klasszikus portal egy frissítési mennyiségi üzenetet jeleníti meg. A sikeres üzenet jelenik meg, amikor a mennyiségi sikeresen megtörtént.

7. Ha a kötet vannak kibontása, hajtsa végre a host a Windows rendszerű számítógépén az alábbi lépéseket:

   1. Nyissa meg a **Számítógép-kezelés** ->**lemezre kezelése**.
   2. Kattintson a jobb gombbal a **Lemez kezelése** , és válassza a **Újraellenőrzése**.
   3. A lemezt a listában jelölje ki a módosított, kattintson a jobb gombbal, és jelölje be **Kötet kiterjesztése**. A mennyiségi kiterjesztése varázsló elindul. Kattintson a **Tovább**gombra.
   4. A varázsló, az alapértelmezett értékeket elfogadása. Miután a varázsló végzett, a mennyiségi meg kell jelennie a nagyobb méretű.

![A videó érhető el](./media/storsimple-manage-volumes/Video_icon.png) **Videó érhető el**

Nézni egy videót, amely bemutatja, hogyan kattintva bontsa ki a kötet, kattintson [ide](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>A mennyiségi offline állapotba

Előfordulhat, hogy a kötet offline állapotba helyezéséhez amikor módosítani vagy törölni szeretné. Amikor egy hangereje offline, azt nem áll rendelkezésre írási és olvasási hozzáférést. Szüksége lesz a mennyiségi offline állapotba, az állomáson, valamint az eszközön. Hajtsa végre az alábbi lépéseket a mennyiségi offline állapotba.

### <a name="to-take-a-volume-offline"></a>A mennyiségi offline állapotba

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

### <a name="to-delete-a-volume"></a>Kötet törlése

1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tárolók** fülre.

2. Jelölje be a mennyiségi tároló, amelyen a törölni kívánt kötet. Kattintson a hangerő tároló, amelyen az **kötet** .

3. Ez a tároló társított összes kötet táblázatos formában jelennek meg. Jelölje be a törölni kívánt kötet állapotát. Ha törölni szeretné a hangerőt nem offline, offline állapotba először [a mennyiségi offline állapotba](#take-a-volume-offline)ismertetett lépéseket követve.

4. A hangerő offline követően kattintson a **törli** az oldal alján.

5. Amikor a program megerősítést kér, kattintson az **Igen**gombra. A hangerő ekkor törlődik, és a **kötet** lapon megjelenik a frissített listáját a tárolóban kötet.

## <a name="monitor-a-volume"></a>A mennyiségi figyelése

Mennyiségi figyelése lehetővé teszi a mennyiségi statisztika lehet/O – kapcsolódó gyűjtése. Figyelés az első 32 kötet létrehozott alapértelmezés szerint engedélyezve van. További mennyiségének figyelése alapértelmezés szerint nincs engedélyezve. Figyelés klónozott mennyiségének is alapértelmezés szerint nincs engedélyezve.

Hajtsa végre az alábbi lépések végrehajtásával engedélyezheti vagy letilthatja a mennyiségi figyelése.

### <a name="to-enable-or-disable-volume-monitoring"></a>Engedélyezése vagy letiltása a mennyiségi figyelése

1. Az **eszközök** lapon válassza ki az eszközt, kattintson rá duplán, és kattintson a **Hangerő tárolók** fülre.

2. Jelölje be a mennyiségi tároló, amelyben a mennyiségi található, és kattintson a a mennyiségi tároló a **kötet** lap eléréséhez.

3. A tároló társított összes kötet táblázatos felhasználói felületén jelennek meg. Kattintson a gombra, és válassza a mennyiségi vagy a mennyiségi adatfeliratsor.

4. A lap alján kattintson a **Módosítás**gombra.

5. A mennyiség módosítása varázsló az **Alapvető beállítások**csoportban jelölje be a **engedélyezése** vagy **letiltása** a **Figyelés** legördülő listából.

    ![A mennyiségi alapvető beállítások módosítása](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)


## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan [adatfeliratsor StorSimple mennyiségig](storsimple-clone-volume.md).

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.
