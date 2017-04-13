<properties 
   pageTitle="StorSimple pillanatkép Manager és a kötet |} Microsoft Azure"
   description="Megtudhatja, hogy miként használhatja a StorSimple pillanatkép Manager beépülő modult, megtekintheti és kezelheti a kötet, és biztonsági mentések beállítása."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>-Kezelővel StorSimple pillanatkép megtekintéséhez és kezeléséhez kötet

## <a name="overview"></a>– Áttekintés

Felhasználhatja a StorSimple pillanatkép Manager **kötet** csomópont (a **hatókör** ablaktáblán) jelölje ki a kötet, és a velük kapcsolatos információk megtekintése. A kötet azokat az állomás csatlakoztatott kötet, egymásnak megfelelő meghajtók mutatják be. A **kötet** csomópontot a helyi kötet és StorSimple, többek között a kötet iSCSI és egy eszközt való talált által támogatott fájltípusok mennyiségi jeleníti meg. 

További információt a támogatott kötet megnyitásához [többféle mennyiségi támogatása](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Eredmény ablaktábla mennyiségi listáján](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

A **kötet** csomópont is lehetővé teszi a újraellenőrzése, vagy törölje a kötet, után StorSimple pillanatkép Manager találja őket. 

Ebben az oktatóanyagban bemutatja, hogyan lehet csatlakoztatása, inicializálni, és kötet formázása és majd használja a StorSimple pillanatkép kezelője:

- Kötet kapcsolatos információk megtekintése 
- Kötet törlése
- Kötet újraellenőrzése 
- Egy egyszerű mennyiségi konfigurálása, és készítsen biztonsági másolatot
- Állítsa be a dinamikus tükrözött kötet, és készítsen biztonsági másolatot

>[AZURE.NOTE] A **mennyiségi** csomópont műveletekről is elérhetők a **Műveletek** ablaktáblában.
 
## <a name="mount-volumes"></a>Kötet csatlakoztatása

A következő eljárással csatlakoztatásához, inicializálni és formázhatja StorSimple őket. Ez az eljárás a lemez kezelése, a rendszer segédprogram merevlemezt, és a megfelelő kötet vagy partíciók kezelésére szolgáló használja. Lemezen kezelésével kapcsolatos további tudnivalókért nyissa meg a [Lemez kezelése](https://technet.microsoft.com/library/cc770943.aspx) a Microsoft TechNet webhelyen.

#### <a name="to-mount-volumes"></a>Kötet csatlakoztatása

1. A host számítógépen indítsa el a Microsoft iSCSI kezdeményező.

2. Adja meg az egyik felület IP-címek, mint a cél portál vagy feltárás IP-címet, majd az eszköz csatlakozzon. Miután csatlakozott az eszközt, a program lesznek elérhetők a Windows rendszerhez. A Microsoft iSCSI kezdeményező használatával kapcsolatos további tudnivalókért nyissa meg a szakasz "Csatlakozik egy iSCSI cél eszközön" [telepítése és konfigurálása a Microsoft]iSCSI kezdeményező[1].

3. Az alábbi lehetőségek közül használatával indítsa el a lemez kezelése:

    - A **Futtatás** mezőbe írja be a Diskmgmt.msc.

    - Indítsa el a Kiszolgálókezelő, bontsa ki a **tárhely** csomópontot, és válassza a **Lemez kezelése**.

    - Indítsa el a **Felügyeleti eszközök**, bontsa ki a **Számítógép-kezelés** csomópontot, és válassza a **Lemez kezelése**. 

    >[AZURE.NOTE] Futtassa a rendszergazdai jogosultságokkal kell használnia.
 
4. Online állapotba jelenti:

   1. Merevlemez-kezelése lapon kattintson a jobb gombbal bármelyik **Offline**megjelölt mennyiségi.

   2. Kattintson a **lemez újraaktiválása**. A lemez be kell jelölni **Online** után a lemez aktiválódik.

5. Inicializálni jelenti:

   1. Kattintson a jobb gombbal az észlelt kötet.

   2. A menüben jelölje ki a **Lemezen inicializálni**.

   3. A **Lemez inicializálni** párbeszédpanelen jelölje ki a lemezen inicializálni kívánt, és kattintson **az OK**gombra.

6. Egyszerű kötet formázása:

   1. Kattintson a jobb gombbal a kötet, amelyet formázni szeretne.

   2. A menüben válassza az **Új egyszerű kötet**.

   3. Az új egyszerű kötet varázsló segítségével a mennyiségi formázása:

      - Adja meg, hogy a kötet méretét.
      - Adja meg a meghajtó betűvel.
      - Jelölje ki az NTFS fájlrendszer.
      - Adjon meg egy 64 KB terhelés egység méretet.
      - Gyorsformázás.

7. Több elem partíciót kötet formázása. Utasításokért ugorjon a, "Partíciót és kötet" [lemez](https://msdn.microsoft.com/library/dd163556.aspx)kezelése végrehajtása.

## <a name="view-information-about-your-volumes"></a>A kötet kapcsolatos információk megtekintése

A következő eljárással helyi és Azure StorSimple kötet kapcsolatos információk megtekintése.

#### <a name="to-view-volume-information"></a>Mennyiségi információk megtekintése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához. 

2. A **hatókör** ablaktáblán kattintson a **kötet** csomópontot. Helyi és csatlakoztatott kötet, többek között az összes Azure StorSimple kötet, listája megjelenik az **eredmény** ablaktáblában. Az **eredmények** ablaktáblájában oszlopai nem állítható be. (Kattintson a jobb gombbal a **kötet** csomópontot, jelölje be a **Nézet**, és válassza az **Oszlopok hozzáadása/eltávolítása**.)

    ![Az oszlopok konfigurálása](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)

    Eredmények oszlop | Leírás 
    :--------------|:-------------
    név           | A **név** oszlopban tartalmazza az egyes észlelt mennyiségi rendelt betű.
    Eszköz         | Az **eszköz** oszlopban tartalmazza az eszköz a számítógép csatlakoztatva van IP-címét.
    Eszköz mennyiségi neve | Az **Eszköz mennyiségi neve** oszlop, amely a kijelölt kötet tartozik eszköz hangerejének nevét tartalmazza. Az adott kötet Azure klasszikus portálon definiált mennyiségi nevét.
    Elérési utak   | Az **Elérési utak** oszlop az elérési utat a mennyiségi jeleníti meg. Ez az a meghajtó vagy csatlakozási pontot, ahol a mennyiségi érhető el az állomáson.
 
## <a name="delete-a-volume"></a>Kötet törlése

A következő eljárással kötet StorSimple pillanatkép Manager törlése.

>[AZURE.NOTE] A mennyiségi nem törölhetők, ha bármelyik mennyiségi csoport része. (A Törlés beállítás nem áll rendelkezésre, amelyek a mennyiségi csoport tagjainak kötet.) Törölnie kell a teljes mennyiségi csoport törlése a mennyiségi.


#### <a name="to-delete-a-volume"></a>Kötet törlése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán kattintson a **kötet** csomópontot. 

3. Az **eredmény** ablaktáblában kattintson a jobb gombbal a hangerőt, amelyeket törölni szeretne.

4. A menüben kattintson a **Törlés**gombra. 

    ![Kötet törlése](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 

5. Ekkor megjelenik a **Kötet törlése** párbeszédpanel. Írja be a **Megerősítés** a szövegmezőbe, és kattintson **az OK**gombra.

6. Alapértelmezés szerint StorSimple pillanatkép Manager biztonsági másolatot készít a mennyiségi törlés előtt. A okokból is a számítógép megvédését az adatok elvesztését, ha a törlés szándékolatlan volt. StorSimple pillanatkép Manager **Automatikus pillanatkép** előrehaladását jelenik meg, miközben, készít biztonsági másolatot a mennyiségi. 

    ![Az automatikus pillanatkép üzenet](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Kötet újraellenőrzése

Az alábbi eljárással StorSimple pillanatkép Manager csatlakoztatott kötet újraellenőrzése.

#### <a name="to-rescan-the-volumes"></a>Hogy a kötet újraellenőrzése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán kattintson a jobb gombbal a **kötet**, és kattintson a **kötet újraellenőrzése**gombra.

    ![Kötet újraellenőrzése](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
 
    Ez az eljárás szinkronizálja a mennyiségi lista StorSimple pillanatkép Manager. Módosítások új kötet vagy törölt kötet, például az eredmények megjelennek.

## <a name="configure-and-back-up-a-basic-volume"></a>Állítsa be, és készítsen biztonsági másolatot egy egyszerű mennyiségi

Az alábbi eljárással biztonsági másolatot egy egyszerű mennyiségi konfigurálása majd vagy biztonsági azonnal elindul, illetve ütemezett biztonsági másolatok házirend létrehozása.

### <a name="prerequisites"></a>Előfeltételek

Első lépések:

- Győződjön meg arról, hogy a StorSimple eszköz és host számítógép megfelelően van-e beállítva. További információért lépjen [Deploy helyszíni StorSimple eszközére](storsimple-deployment-walkthrough-u2.md).

- Telepítése és beállítása StorSimple pillanatkép Manager. További információért lépjen [StorSimple pillanatkép Manager telepítése](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Biztonsági másolat alapvető kötet konfigurálása

1. Hozzon létre egy egyszerű mennyiségi StorSimple az eszközre.

2. Csatlakoztassa, inicializálni és formázása a hangerőt, [Kötet csatlakoztatása](#mount-volumes)ismertetett módon. 

3. Kattintson a StorSimple pillanatkép Manager ikonra az asztalon. A StorSimple pillanatkép kezelő ablakban megjelenik. 

4. A **hatókör** ablaktáblán kattintson a jobb gombbal a **kötet** csomópontot, és válassza a **kötet újraellenőrzése**gombra. A beolvasott képet befejezéskor kötet listája jelenjen meg az **eredmény** ablaktáblában. 

5. Az **eredmény** ablaktáblában kattintson a jobb gombbal a hangerőt, és válassza a **Mennyiségi csoport létrehozása**. 

    ![Mennyiségi csoport létrehozása](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 

6. **Mennyiségi csoport létrehozása** párbeszédpanelen írja be a mennyiségi csoport nevét, kötet rendelni, és kattintson **az OK**gombra.

7. A **hatókör** ablaktáblán bontsa ki a **Mennyiségi csoportok** csomópontot. Az új mennyiségi csoport megjelennek a **Mennyiségi csoportok** csomópont alatt. 

8. Kattintson a jobb gombbal a mennyiségi csoport nevére.

    - Interaktív (igény szerinti) biztonsági mentési feladat kezdéséhez kattintson a **Biztonsági másolat készítése**. 

    - Az automatikus biztonsági mentés ütemezhet, kattintson a **Biztonsági másolat házirend létrehozása**parancsra. Az **Általános** lapon jelölje be a mennyiségi csoportot a listából. Az **Ütemterv** lapon adja meg a kimutatás adatait. Amikor elkészült, kattintson az **OK gombra**. 

9. Győződjön meg arról, hogy a biztonsági mentési feladat elindult, bontsa ki a **feladatok** csomópontot, a **hatókör** ablaktáblán, és válassza az **operációs rendszert futtató** csomópontot. A futó feladatok listáját megjelenik az **eredmény** ablaktáblában. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Állítsa be, és készítsen biztonsági másolatot dinamikus tükrözött kötet

Biztonsági másolatot dinamikus tükrözött kötet konfigurálásához az alábbi lépések elvégzésével:

- Lépés: 1: Használata lemez kezelése dinamikus tükrözött kötet létrehozásához. 

- Lépés: 2: StorSimple pillanatkép Managert használhatja biztonsági másolat konfigurálása.

### <a name="prerequisites"></a>Előfeltételek

Első lépések:

- Győződjön meg arról, hogy a StorSimple eszköz és host számítógép megfelelően van-e beállítva. További információért lépjen [Deploy helyszíni StorSimple eszközére](storsimple-deployment-walkthrough-u2.md).

- Telepítése és beállítása StorSimple pillanatkép Manager. További információért lépjen [StorSimple pillanatkép Manager telepítése](storsimple-snapshot-manager-deployment.md).

- Állítsa be a két kötet StorSimple az eszközre. (A példákban a rendelkezésre álló kötet nincs **lemez 1** és a **lemez 2**.) 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Lépés: 1: Használata lemez Management dinamikus tükrözött kötet létrehozása

Lemezen kezelésére szolgál a merevlemezeken és mennyiségeket vagy partíciók, amelyek a bennük kezelésére szolgáló egy rendszer segédprogramot. Lemezen kezelésével kapcsolatos további tudnivalókért nyissa meg a [Lemez kezelése](https://technet.microsoft.com/library/cc770943.aspx) a Microsoft TechNet webhelyen.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Dinamikus tükrözött kötet létrehozása

1. Az alábbi lehetőségek közül használatával indítsa el a lemez kezelése: 

   - Nyissa meg a **Futtatás** párbeszédpanelt, írja be a **Diskmgmt.msc**, és nyomja le az ENTER billentyűt.

   - Indítsa el a Kiszolgálókezelő, bontsa ki a **tárhely** csomópontot, és válassza a **Lemez kezelése**. 

   - Indítsa el a **Felügyeleti eszközök**, bontsa ki a **Számítógép-kezelés** csomópontot, és válassza a **Lemez kezelése**. 

2. Győződjön meg arról, hogy két kötet elérhető StorSimple az eszközre. (A példában a rendelkezésre álló kötet nincs **lemez 1** és a **lemez 2**.) 

3. A lemez Management ablakban a jobb oldali oszlopban az alsó ablaktáblában kattintson a jobb gombbal a **lemez 1** , és válassza a **Új tükrözött mennyiségi**. 

    ![Új tükrözött kötet](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 

4. Az **Új tükrözött mennyiségi** varázsló lapján kattintson a **Tovább**gombra.

5. A **Lemez kiválasztása** lapon jelölje be a **lemez 2** a **kijelölt** ablakban, kattintson a **Hozzáadás**gombra, és kattintson a **Tovább gombra**. 

6. A **meghajtó betű hozzárendelése vagy az elérési utat** lapon fogadja el az alapértelmezett beállításokat, és kattintson a **Tovább gombra**. 

7. **Mennyiségi formátum** lapon a **Felosztás egység** listában jelölje ki a **64 ezer**. Jelölje be a **gyorsformázás** jelölőnégyzetet, és kattintson a **Tovább**gombra. 

8. **Az új tükrözött kötet befejezése** lapon tekintse át a beállításokat, és kattintson a **Befejezés gombra**. 

9. A megjelenik egy üzenet jelzi, hogy a egyszerű lemez dinamikus lemezen alakulnak. Kattintson az **Igen**gombra.

    ![Dinamikus lemez átalakítási üzenet](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 

10. A merevlemez-kezelés győződjön meg arról, hogy a lemez 1 és a lemez 2 oszlopcímkeként jelennek meg a dinamikus tükrözött. (**Dinamikus** jelenjen meg az Állapot oszlopban, és módosítsa a kapacitás sáv színe piros, tükrözött kötet jelző.) 

    ![Lemezen tükrözött Management dinamikus lemezen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 
 
### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Lépés: 2: StorSimple pillanatkép Managert használhatja biztonsági másolat konfigurálása

Az alábbi eljárással konfigurálása dinamikus tükrözött kötet, majd vagy biztonsági azonnal elindul, illetve ütemezett biztonsági másolatok házirend létrehozása.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Dinamikus tükrözött kötet biztonsági másolata konfigurálása

1. Kattintson a StorSimple pillanatkép Manager ikonra az asztalon. A StorSimple pillanatkép kezelő ablakban megjelenik. 

2. A **hatókör** ablaktáblán kattintson a jobb gombbal a **kötet** csomópontot, és válassza a **kötet újraellenőrzése**. A beolvasott képet befejezéskor kötet listája jelenjen meg az **eredmény** ablaktáblában. A dinamikus tükrözött kötet egyetlen kötet néven jelenik meg. 

3. Az **eredmény** ablaktáblában kattintson a jobb gombbal a dinamikus tükrözött kötet, és válassza a **Mennyiségi csoport létrehozása**. 

4. **Mennyiségi csoport létrehozása** párbeszédpanelen írja be a mennyiségi csoport nevét, a dinamikus tükrözött kötet rendelni ezt a csoportot, és kattintson **az OK**gombra. 

5. A **hatókör** ablaktáblán bontsa ki a **Mennyiségi csoportok** csomópontot. Az új mennyiségi csoport megjelennek a **Mennyiségi csoportok** csomópont alatt. 

6. Kattintson a jobb gombbal a mennyiségi csoport nevére. 

    - Interaktív (igény szerinti) biztonsági mentési feladat kezdéséhez kattintson a **Biztonsági másolat készítése**. 

    - Az automatikus biztonsági mentés ütemezhet, kattintson a **Biztonsági másolat házirend létrehozása**parancsra. Az **Általános** lapon jelölje be a mennyiségi csoport a listából. Az **Ütemterv** lapon adja meg a kimutatás adatait. Amikor elkészült, kattintson az **OK gombra**. 

7. A biztonsági mentési feladat figyelheti futása közben. A **hatókör** ablaktáblán bontsa ki a **feladatok** csomópontot, és kattintson **fut**, a feladat részletei megjelennek az **eredmény** ablaktáblában. A biztonsági mentési feladat befejezésekor, a részletek átkerülnek az **utolsó 24** óra feladatlista. 

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, [StorSimple pillanatkép Manager a StorSimple megoldás való](storsimple-snapshot-manager-admin.md)használatáról.
- Ismerje meg, használata [StorSimple pillanatkép kezelője mennyiségi csoportok létrehozása és kezelése](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
