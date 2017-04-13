<properties 
   pageTitle="StorSimple pillanatkép Manager biztonságimásolat-katalógus |} Microsoft Azure"
   description="A StorSimple pillanatkép Manager beépülő modult használatával megtekintheti és kezelheti a biztonságimásolat-katalógus ismerteti."
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
   ms.date="04/26/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Pillanatkép Managert használhatja StorSimple, a biztonságimásolat-katalógus kezelése

## <a name="overview"></a>– Áttekintés

Az elsődleges StorSimple pillanatkép Manager funkciója lehetővé teszi a pillanatképek formájában StorSimple kötet alkalmazás egységes biztonsági másolatot készíteni. Pillanatképek majd szerepelnek a *biztonságimásolat-katalógus*nevezett XML-fájlba. A biztonságimásolat-katalógus pillanatképek mennyiségi csoportot, majd a helyi pillanatfelvétel vagy felhőalapú pillanatkép rendezi. 

Az oktatóanyag azt ismerteti, hogyan használhatja a **Biztonsági másolat katalógus** csomópont az alábbi feladatok elvégzésére:

- A hangerő visszaállítása 
- A mennyiségi vagy mennyiségi csoport másolása 
- Biztonsági másolat törlése 
- Fájlok helyreállítása
- A Storsimple pillanatkép Manager-adatbázis visszaállítása

Megtekinthetők a biztonságimásolat-katalógus kibontása a **Biztonságimásolat-katalógus** csomópontot, a **hatókör** ablaktáblán, és kattintson a a mennyiségi csoport kibontása

- Ha mennyiségi nevére kattint, az **eredmény** ablaktáblában a helyi állapotaira és a felhő pillanatképek érhető el a mennyiségi csoport száma látható. 

- Ha **Helyi pillanatfelvétel** vagy **Felhőalapú pillanatkép**gombra kattint, az **eredményeket** tartalmazó ablaktáblában minden biztonsági pillanatkép (attól függően, hogy a **Nézet** beállításai) az alábbi információkat jeleníti meg: 

    - **Név** – idő a pillanatkép volt. 

    - **Típus** – e ez a helyi pillanatkép vagy felhőalapú pillanatkép megtekintéséhez. 

    - **Tulajdonos** – a tartalom tulajdonosa. 

    - **Érhető el** – jelenleg elérhető-e a pillanatkép. **Igaz** azt jelzi, hogy a pillanatkép elérhető, ahonnan; **Hamis** azt jelzi, hogy a pillanatkép már nem érhető el. 

    - **Imported** -e a biztonsági mentés importálva. **Igaz** jelzi, hogy a biztonsági mentés StorSimple kezelő szolgáltatás importálta az eszköz konfigurálásáról StorSimple pillanatkép Manager; időben **Hamis** azt jelzi, hogy azt nem importálta, de StorSimple pillanatkép kezelő által lett létrehozva. (Könnyedén megállapítható az importált mennyiségi csoport mivel utótag kerül, amely azonosítja az eszközt, amelyből a mennyiségi csoport importálta.)

    ![Biztonsági másolat katalógus](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)

- Bontsa ki a **Helyi pillanatkép** vagy **Felhőalapú pillanatkép**, és kattintson az egyes pillanatkép nevét, ha az **eredményeket** tartalmazó ablaktáblában a kijelölt pillanatkép az alábbi információkat jeleníti meg:

    - **Név** – a meghajtó betűvel jelölt mennyiségi. 

    - **Helyi név** – a helyi nevét a meghajtó (ha elérhető). 

    - **Eszköz** – az eszköz nevét a hangerejének található. 

    - **Érhető el** – jelenleg elérhető-e a pillanatkép. **Igaz** azt jelzi, hogy a pillanatkép elérhető, ahonnan; **Hamis** azt jelzi, hogy a pillanatkép már nem érhető el. 


## <a name="restore-a-volume"></a>A hangerő visszaállítása

Az alábbi eljárással kötet visszaállítása biztonsági másolatból.

#### <a name="prerequisites"></a>Előfeltételek

Ha még nem tette meg, a mennyiségi és mennyiségi csoportot létrehozni, és törölje a mennyiségi. Alapértelmezés szerint StorSimple pillanatkép Manager biztonsági másolatot készít a mennyiségi előtt lehetővé teszi, hogy törölhető. Ez a okokból megakadályozhatja, hogy az adatok elvesztését, ha a mennyiségi véletlenül törölné, vagy ha valamilyen okból állíthatók van szüksége az adatok. 

StorSimple pillanatkép Manager a következő üzenetet jeleníti meg, miközben a megelőző biztonsági másolatot készít.

![Az automatikus pillanatkép üzenet](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

>[AZURE.IMPORTANT]A mennyiségi mennyiségi csoport részét képező nem törölhetők. A Törlés beállítás nem érhető el. <br>

#### <a name="to-restore-a-volume"></a>A hangerő visszaállítása

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához. 

2. A **hatókör** ablaktáblán bontsa ki a **Biztonsági másolat katalógus** csomópontot, mennyiségi csoport kibontása, és válassza a **Helyi pillanatképek** vagy **Felhőalapú pillanatképek**. Biztonsági pillanatképek listája megjelenik az **eredmény** ablaktáblában. 

3. Keresse meg a biztonsági mentés, amelyet szeretne visszaállítani, kattintson a jobb gombbal, majd kattintson a **Visszaállítás**gombra. 

    ![Biztonsági másolat katalógus visszaállítása](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 

4. A Megerősítés lapon tekintse át a részleteket, írja be a **Megerősítés**, és kattintson **az OK**gombra. StorSimple pillanatkép Manager használja a biztonsági mentés visszaállítása a mennyiségi. 

    ![Megerősítést kérő üzenet visszaállítása](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 

5. Figyelheti a visszaállítási művelet futása közben. A **hatókör** ablaktáblán bontsa ki a **feladatok** csomópontot, és kattintson a **futtatása**gombra. A feladat részletei az **eredmény** ablaktáblában jelennek meg. A visszaállítási feladat befejezésekor, a feladat részletei átkerülnek az **utolsó 24 óra** listában.

## <a name="clone-a-volume-or-volume-group"></a>A mennyiségi vagy mennyiségi csoport másolása

Az alábbi eljárással másolatának létrehozása (adatfeliratsor) a mennyiségi vagy mennyiségi csoportot.

#### <a name="to-clone-a-volume-or-volume-group"></a>A mennyiségi vagy mennyiségi csoport klónozhatja

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán bontsa ki a **Biztonsági mentés katalógusának** csomópontot, mennyiségi csoport kibontása, és kattintson a **Felhőben pillanatképek**parancsra. Biztonsági másolatok listája megjelenik az **eredmény** ablaktáblában.

3. Keresse meg a hangerőt, vagy a mennyiségi kívánt csoportot, klónozhatja, és kattintson a jobb gombbal a hangerőt, vagy a mennyiségi csoport neve, **Adatfeliratsor**lehetőségre. Megjelenik az **Adatfeliratsor felhő pillanatkép** párbeszédpanel.

    ![Felhőalapú pillanatkép másolása](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Az **Adatfeliratsor felhő pillanatkép** párbeszédpanelen végezze el az alábbi képlettel történik: 

    1. **A szöveg mezőbe** írja be a klónozott mennyiségi nevét. Ez a név megjelennek a **kötet** csomópontot. 

    2. (Nem kötelező) jelölje be a **meghajtóba**, és a legördülő listából válassza ki a meghajtó betűvel. 

    3. (Nem kötelező) válassza ki a **Mappát (NTFS)**, és írjon be egy mappa elérési útja vagy kattintson a Tallózás gombra és jelölje ki a mappa helyét. 

    4. Kattintson a **létrehozása**gombra.

5. Amikor a klónképző folyamat befejeződik, inicializálni a klónozott hangerejének. Indítsa el a Kiszolgálókezelő, és kezdje a lemez kezelése. Részletes útmutatásért olvassa el a [Kötet csatlakoztatása](storsimple-snapshot-manager-manage-volumes.md#mount-volumes)című témakört. Elindul, miután a mennyiségi látható csoportban a **hatókör** ablaktáblán a **kötet** csomópontot. Ha nem látható a hangerő szerepel a felsorolásban, frissítse a listát, mennyiségének (kattintson a jobb gombbal a **kötet** csomópontot, és kattintson a **frissítés**).

## <a name="delete-a-backup"></a>Biztonsági másolat törlése

A következő eljárással pillanatkép törlése a biztonsági másolat katalógusról. 

>[AZURE.NOTE]Pillanatkép törlésekor a biztonsági másolatban lévő adatok a pillanatkép társított. A folyamaton, a felhőből adatok törlése, előfordulhat, hogy szánjon némi időt.<br>
 
#### <a name="to-delete-a-backup"></a>Biztonsági másolat törlése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán bontsa ki a **Biztonsági mentés katalógusának** csomópontot, mennyiségi csoport kibontása, és válassza a **Helyi pillanatképek** vagy **Felhőalapú pillanatképek**. Pillanatképek listája megjelenik az **eredmény** ablaktáblában. 

3. Kattintson a jobb gombbal a törölni kívánt pillanatkép, és kattintson a **Törlés**gombra.

4. A megerősítést kérő üzenet megjelenésekor kattintson az **OK gombra**. 

## <a name="recover-a-file"></a>Fájlok helyreállítása

Ha egy mennyiségi véletlenül töröl egy fájlt, visszaállíthatja a fájl beolvasása egy pillanatfelvételek előtti dátumokat a törlés, és a pillanatkép használatával hozhat létre a kötet átirattal, majd a fájl másolása a klónozott kötet az eredeti kötet.

#### <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, ellenőrizze, hogy rendelkezik-e a mennyiségi csoport aktuális biztonsági másolatot. Törölje a kötet mennyiségi lévő egyik tárolt fájl. Végül kövesse az alábbi lépéseket a törölt fájl visszaállítása biztonsági másolatból a. 

#### <a name="to-recover-a-deleted-file"></a>A törölt fájlok helyreállítása

1. Kattintson a StorSimple pillanatkép Manager ikonra az asztalon. Megjelenik a StorSimple pillanatkép Manager konzol ablak. 

2. A **hatókör** ablaktáblán bontsa ki a **Biztonsági másolat katalógus** csomópontot, és tallózással keresse meg a törölt fájlt tartalmazó pillanatkép megtekintéséhez. Általában akkor válassza a törlés előtt létrehozott pillanatkép. 

3. Keresse meg a hangerőt, klónozhatja, amelyet kattintson a jobb gombbal, és kattintson a **Adatfeliratsor**. Megjelenik az **Adatfeliratsor felhő pillanatkép** párbeszédpanel.

    ![Felhőalapú pillanatkép másolása](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Az **Adatfeliratsor felhő pillanatkép** párbeszédpanelen végezze el az alábbi képlettel történik: 

   1. **A szöveg mezőbe** írja be a klónozott mennyiségi nevét. Ez a név megjelennek a **kötet** csomópontot. 

   2. (Nem kötelező) Jelölje be a **meghajtóba**, és a legördülő listából válassza ki a meghajtó betűvel. 

   3. (Nem kötelező) Jelölje ki a **Mappát (NTFS)**, és írjon be egy mappa elérési útja, vagy kattintson a **Tallózás gombra** , és válasszon ki egy helyet a mappának. 

   4. Kattintson a **létrehozása**gombra. 

5. Amikor a klónképző folyamat befejeződik, inicializálni a klónozott hangerejének. Indítsa el a Kiszolgálókezelő, és kezdje a lemez kezelése. Részletes útmutatásért olvassa el a [Kötet csatlakoztatása](storsimple-snapshot-manager-manage-volumes.md#mount-volumes)című témakört. Elindul, miután a mennyiségi látható csoportban a **hatókör** ablaktáblán a **kötet** csomópontot. 

    Ha nem látható a hangerő szerepel a felsorolásban, frissítse a listát, mennyiségének (kattintson a jobb gombbal a **kötet** csomópontot, és kattintson a **frissítés**).

6. Nyissa meg a NTFS mappát, amely tartalmazza a klónozott kötet, bontsa ki a **kötet** csomópontot, és nyissa meg a klónozott mennyiségi. Keresse meg a helyreállítani kívánt fájlt, és másolja a vágólapra az elsődleges mennyiségi.

7. Miután visszaállította a fájlt, törölheti a klónozott mennyiségi tartalmazó NTFS mappát.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>A StorSimple pillanatkép Manager-adatbázis visszaállítása

Rendszeresen készítsen biztonsági másolatot a StorSimple pillanatkép Manager-adatbázis az állomáson. Ha katasztrófa fordul elő, vagy valamilyen okból a számítógép nem sikerül, majd visszaállíthatja a biztonsági másolatból. Az adatbázis biztonsági másolatának létrehozása a következő kézi áll.

#### <a name="to-back-up-and-restore-the-database"></a>Biztonsági mentése és visszaállítása az adatbázis

1. A Microsoft StorSimple alkalmazáskezelési szolgáltatás leállítása:

    1. Indítsa el a Kiszolgálókezelő.

    2. A kiszolgáló Manager irányítópultjával, kattintson az **eszközök** menü válassza ki a **szolgáltatásokat**.

    3. A **szolgáltatások** ablakban jelölje ki a **Microsoft StorSimple alkalmazáskezelési szolgáltatás**.

    4. A jobb oldali **Microsoft StorSimple alkalmazáskezelési szolgáltatás**csoportban kattintson **a szolgáltatás leállítása**.

2. A host számítógépen tallózással C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData egy rejtett mappát.
 
3. Keresse meg a katalógus XML-fájlt, másolja át a fájlt, és a másolatot tárolni megbízható helyen vagy a felhőben. Ha nem sikerül az állomás, a biztonságimásolat-fájlt segítségével helyreállítása a létrehozott StorSimple pillanatkép Manager biztonsági házirendek súgó.

    ![Azure StorSimple biztonságimásolat-katalógus fájl](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)

4. Indítsa újra a Microsoft StorSimple alkalmazáskezelési szolgáltatás: 

    1. A kiszolgáló Manager irányítópultjával, kattintson az **eszközök** menü válassza ki a **szolgáltatásokat**.
    
    2. A **szolgáltatások** ablakban jelölje ki a **Microsoft StorSimple alkalmazáskezelési szolgáltatás**.

    3. A jobb oldali **Microsoft StorSimple alkalmazáskezelési szolgáltatás**csoportban kattintson a **Indítsa újra a szolgáltatást**.

5. A host számítógépen tallózással C:\ProgramData\Microsoft\StorSimple\BACatalog. 

6. Törölje a katalógus XML-fájlt, és cserélhet le az Ön által létrehozott biztonsági másolatot. 

7. Kattintson az asztali StorSimple pillanatkép Manager ikonra StorSimple pillanatkép kezelő indításához. 

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [felügyelheti a StorSimple megoldás StorSimple pillanatkép segítségével](storsimple-snapshot-manager-admin.md).
- További tudnivalók a [StorSimple pillanatkép Manager tevékenységek és a munkafolyamatokat](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).
