<properties 
   pageTitle="StorSimple pillanatkép Manager telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként töltse le és telepítse a beépülő modult a StorSimple adatok védelme és biztonsági funkciók kezeléséről a StorSimple pillanatkép Manager."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>A StorSimple pillanatkép kezelő MMC beépülő modul telepítése

## <a name="overview"></a>– Áttekintés

A StorSimple pillanatkép felügyelője egy Microsoft Management Console (MMC) beépülő modul, amely egyszerűbbé teszi az adatok védelme és a biztonságimásolat-kezelési StorSimple a Microsoft Azure környezetben. StorSimple pillanatkép kezelővel, kezelheti a Microsoft Azure StorSimple a helyszíni és felhőbeli tárhely, mintha egy teljesen integrált tároló rendszer, így biztonsági mentési és visszaállítási folyamatok egyszerűsítése és költségek csökkentése. 

Ebben az oktatóanyagban konfigurációs követelmények, valamint telepítése, eltávolítása és frissítés StorSimple pillanatkép Manager eljárásokat ismerteti.

>[AZURE.NOTE] 
>
>- Microsoft Azure StorSimple virtuális tömbök (más néven StorSimple helyszíni virtuális eszközök) kezelése StorSimple pillanatkép Manager nem használható.
>
>- Ha telepíteni StorSimple frissítés 2 StorSimple eszközén, feltétlenül a legújabb StorSimple pillanatkép kezelője letölti és telepíti **StorSimple frissítés 2 telepítése előtt**. A legújabb StorSimple pillanatkép Manager visszamenőleges kompatibilitás működik, és működik-e a Microsoft Azure StorSimple kiadott verziói. Az előző verzióhoz képest StorSimple pillanatkép Manager használatakor szüksége lesz frissítheti (, nem kell az új verzió telepítése előtt, távolítsa el az előző verzióhoz képest).

## <a name="storsimple-snapshot-manager-installation"></a>StorSimple pillanatkép Manager telepítése

A Windows Server 2008 R2 SP1, Windows Server 2012-ben vagy Windows Server 2012 R2 operációs rendszert futtató számítógépeken StorSimple pillanatkép Manager telepíthető. A kiszolgálón a Windows 2008 R2 telepítenie kell a Windows Server 2008 SP1 és a Windows Management Framework 3.0. 

Mielőtt telepítése vagy frissítése a StorSimple pillanatkép Manager beépülő modult a Microsoft Management Console (MMC), győződjön meg arról, hogy a Microsoft Azure StorSimple eszköz és host kiszolgáló helyesen vannak-e beállítva. 

## <a name="configure-prerequisites"></a>Állítsa be a vonatkozó követelmények

Az alábbi lépésekkel adja meg a magas szintű áttekintése konfigurációs feladatokat, hogy ki kell töltenie a StorSimple pillanatkép kezelő telepítése előtt. Teljes Microsoft Azure StorSimple konfigurációját és a telepítési információ, többek között a rendszerkövetelmények és részletes útmutatást talál [Deploy helyszíni StorSimple eszközére](storsimple-deployment-walkthrough.md).

>[AZURE.IMPORTANT]Mielőtt elkezdené, tekintse át a [Feladatlista a telepítésének konfigurálásához](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) és és [telepítési előfeltételek](storsimple-deployment-walkthrough.md#deployment-prerequisites) a [Deploy helyszíni StorSimple eszközére](storsimple-deployment-walkthrough.md).
> <br>
 
### <a name="before-you-install-storsimple-snapshot-manager"></a>StorSimple pillanatkép Manager telepítése előtt

1. Kicsomagolása, csatlakoztatni, és csatlakoztassa a Microsoft Azure StorSimple eszközt, [telepítse az StorSimple 8100 eszköz](storsimple-8100-hardware-installation.md) vagy [a StorSimple 8600 eszköz](storsimple-8600-hardware-installation.md)leírt módon.

2. Győződjön meg arról, hogy a gazdaszámítógép fut az alábbi operációs rendszerek egyikét:

    - A Windows Server 2008 R2 (a kiszolgálón a Windows 2008 R2, telepítenie kell a Windows Server 2008 SP1 és a Windows Management Framework 3.0)
    - A Windows Server 2012
    - A Windows Server 2012 R2
 
    A host StorSimple virtuális eszköz a Microsoft Azure virtuális gép kell lennie. 

3. Győződjön meg arról, hogy az a Microsoft Azure StorSimple konfigurációs feltételek teljesülése. A részletekért lépjen [telepítési előfeltételek](storsimple-deployment-walkthrough.md#deployment-prerequisites).

4. Az eszköz csatlakoztatása az állomásnév oszlopban, és hajtsa végre a kezdeti konfigurálása. A részletekért lépjen [telepítési lépéseket](storsimple-deployment-walkthrough.md#deployment-steps).

5. Kötet létre az eszközön, személyek hozzárendelése a fogadó, és ellenőrizze, hogy a fogadó csatlakoztatni, és használhatja őket. StorSimple pillanatkép Manager támogatja a következő típusú kötet: 

    - Egyszerű kötet
    - Egyszerű kötet
    - Dinamikus kötet
    - Dinamikus tükrözött (RAID 1)
    - Fürt megosztott kötet
 
    Tudni a StorSimple vagy StorSimple virtuális eszközzel létrehozott kötet, nyissa meg [lépés 6: hozzon létre egy mennyiségi](storsimple-deployment-walkthrough.md#step-6-create-a-volume), a [Deploy helyszíni StorSimple eszközére](storsimple-deployment-walkthrough.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Egy új StorSimple pillanatkép Manager telepítése

StorSimple pillanatkép Manager telepítése előtt győződjön meg arról, hogy a kötet létre a StorSimple eszközön vagy StorSimple virtuális eszköz csatlakoztatott, inicializált és formázott [konfigurálása Előfeltételek](#configure-prerequisites)leírt módon.

>[AZURE.IMPORTANT]
>
>- A host StorSimple virtuális eszköz a Microsoft Azure virtuális gép kell lennie. 
>
>- A host Windows 2008 R2, Windows Server 2012-ben vagy Windows Server 2012 R2 futnia kell. Ha a kiszolgáló a Windows Server 2008 R2 rendszer fut, akkor is telepíteni kell a Windows Server 2008 SP1 és a Windows Management Framework 3.0.
>
>- Csatlakozás előtt az eszköz StorSimple pillanatkép Manager meg kell adnia egy iSCSI-kapcsolat az StorSimple eszközön az állomástól.

Kövesse ezeket a lépéseket követve egy friss telepítéshez StorSimple pillanatkép Manager. Ha szeretné telepíteni egy frissítést, válassza a [frissítése vagy telepítse újra a StorSimple pillanatkép Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

- Lépés: 1: StorSimple pillanatkép Manager telepítése 
- Lépés: 2: Csatlakozás StorSimple pillanatkép kezelő eszköz 
- 3 lépés: Ellenőrizze a kapcsolat az eszközön 

###<a name="step-1-install-storsimple-snapshot-manager"></a>Lépés: 1: StorSimple pillanatkép Manager telepítése

A StorSimple pillanatkép kezelő telepítéséhez kövesse az alábbi lépéseket.

#### <a name="to-install-storsimple-snapshot-manager"></a>A StorSimple pillanatkép kezelő telepítéséhez

1. Töltse le a StorSimple pillanatkép Managert (Nyissa meg a Microsoft Download Center [StorSimple pillanatkép Manager](https://www.microsoft.com/download/details.aspx?id=44220) ) és a helyben menteni az állomáson.

2. A Fájlkezelőben kattintson a jobb gombbal a tömörített naplófájlt, és kattintson az **összes kibontása**gombra.

3. A **tömörített kibontása (Zipped)** ablakban **Jelölje be a cél, és bontsa ki a fájlok** mezőbe írja be vagy tallózással keresse meg a Ha szeretné, hogy a bemutatókban használja a fájl elérési útját. 

       >[AZURE.IMPORTANT] A c meghajtó StorSimple pillanatkép Manager kell telepíthető.
 
4. Jelölje be a **befejezéskor kibontott fájlok megjelenítése** jelölőnégyzetet, és kattintson a **kibontásához**.

    ![Bontsa ki a fájlok párbeszédpanel](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. A kibontás befejezésekor, megnyílik a célmappát. Kattintson duplán az alkalmazás beállítása ikonjára, amely megjelenik a cél mappában.

5. A **Telepítő sikeres** üzenet jelenik meg, kattintson a **Bezárás**gombra. Ekkor megjelennek a StorSimple pillanatkép Manager ikonra az asztalon.

    ![asztali ikon](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Lépés: 2: Csatlakozás StorSimple pillanatkép kezelő eszköz

Kövesse az alábbi lépéseket StorSimple pillanatkép Manager csatlakozhat egy StorSimple eszközt.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>Való csatlakozáshoz StorSimple pillanatkép-kezelő eszköz

1. Kattintson a StorSimple pillanatkép Manager ikonra az asztalon. A StorSimple pillanatkép kezelő ablakban megjelenik. Az ablak egy **hatókör** , egy **eredményeket** tartalmazó ablaktáblában, és egy **Műveletek** ablaktábla tartalmazza. 

    ![StorSimple pillanatkép Manager felhasználói felület](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    - A **hatókör** ablaktáblán (a bal oldali ablaktáblában) csomópontot a fastruktúra rendezett listáját tartalmazza. Jelöljön ki egy nézet vagy csomópontra kapcsolódó adatokat néhány csomópontok elemre. Kattintson a nyílikonra kattintva kibonthatja vagy összecsukhatja a csomópontot. Kattintson a jobb gombbal egy elemet a **hatókör** ablaktáblán az elemhez elérhető műveletek listájának megjelenítéséhez. 

    - Az **eredmények** ablaktáblája (a középső ablaktáblában) a csomópontot, nézet vagy a **hatókör** ablaktáblában kijelölt adatok állapota részletes információkat tartalmaz.

    - A **Műveletek** ablaktáblában a műveleteket végezhet a csomópontot, nézet vagy a **hatókör** ablaktáblában kijelölt adatok sorolja fel.

    A StorSimple pillanatkép Manager felhasználói felület teljes körű leírását olvassa el a [StorSimple pillanatkép Manager felhasználói felület](storsimple-use-snapshot-manager.md)című témakört.

2. A **hatókör** ablaktáblán kattintson a jobb gombbal a **eszközök** csomópontot, és kattintson a **beállítás egy eszközt**. Az **eszköz beállítása** párbeszédpanel jelenik meg.

    ![Eszköz beállítása](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3. Az **eszköz** legördülő listában jelölje ki a Microsoft Azure StorSimple eszközök vagy virtuális IP-címét. A **jelszó** mezőbe írja be a StorSimple pillanatkép Manager jelszót, Ön által létrehozott az eszköz az Azure klasszikus portálon. Kattintson az **OK gombra**.

4. StorSimple pillanatkép Manager azonosította, hogy az eszköz keres. Az eszköz érhető el, ha StorSimple pillanatkép Manager összeadja a kapcsolatot. Azt is megteheti, hogy [az eszközön való kapcsolódás ellenőrzése](#to-verify-the-connection) , hogy a kapcsolat hozzáadása sikeresen befejeződött megerősítéséhez.

    Ha valamilyen okból az eszköz nem érhető el, a StorSimple pillanatkép Manager hibaüzenetet ad vissza. Kattintson az **OK gombra** kattintva zárja be a hibaüzenetet, és kattintson a **Mégse gombra** az **eszköz beállítása** párbeszédpanel bezárásához.

5. Eszköz csatlakozik, StorSimple pillanatkép Manager importálja minden mennyiségi csoport be van állítva, hogy az eszköz, feltéve, hogy a kötet csoport biztonsági másolatok van társítva. Mennyiségi csoportok, amelyeknek nincs társított biztonsági másolatok nem importálja. Emellett a mennyiségi csoport készített biztonsági házirendek nem importálva. Ha látni szeretné az importált csoportokat, kattintson a jobb gombbal a legfelső **Mennyiségi csoportok** csomópontot a **hatókör** ablakban, majd kattintson a **Váltás importált csoportok**gombra.

### <a name="step-3-verify-the-connection-to-the-device"></a>3 lépés: Ellenőrizze a kapcsolat az eszközön

Ellenőrizze, hogy StorSimple pillanatkép Manager csatlakoztatva van-e a StorSimple eszköz kövesse az alábbi lépéseket.

#### <a name="to-verify-the-connection"></a>Ellenőrizze a kapcsolat

1. A **hatókör** ablaktáblán kattintson az **eszközök** csomópontot.

    ![StorSimple pillanatkép-kezelő eszköz állapota](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. Az **eredmények** ablaktáblája ellenőrzése: 

   - Ha a zöld jelző jelenik meg az eszköz ikonjára, és a **rendelkezésre álló** jelenik meg, az **állapot** oszlopban, az eszköz csatlakozik. 

   - Ha egy piros jelölő jelenik meg az eszköz ikonjára, és nem érhető el az **állapot** oszlopban jelenik meg, az eszköz nem csatlakozik. 

   - Ha **frissítésével** megjelenik az **állapot** oszlopban, majd StorSimple pillanatkép Manager van beolvasása mennyiségi csoportok és a kapcsolódó biztonsági másolatok egy csatlakoztatott eszközhöz

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Frissítése vagy StorSimple pillanatkép Manager újratelepítése

El kell távolítania StorSimple pillanatkép Manager teljesen telepítenie a szoftvert vagy frissítése előtt. 

StorSimple pillanatkép Manager újratelepítése, előtt készítsen biztonsági másolatot a meglévő StorSimple pillanatkép Manager-adatbázis az állomáson. Ez a biztonsági házirendek és a konfigurációs információk menti, így könnyűszerrel visszaállíthatja az adatok biztonsági másolatból.

Ha frissít, akár StorSimple pillanatkép Manager újratelepítése, tegye a következőket:

- Lépés: 1: Távolítsa el a StorSimple pillanatkép Manager 
- Lépés: 2: Az adatbázis biztonsági mentése StorSimple pillanatkép Manager 
- Lépés 3: Telepítse újra a StorSimple pillanatkép Manager, és az adatbázis visszaállítása 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Lépés: 1: Távolítsa el a StorSimple pillanatkép Manager

StorSimple pillanatkép Manager eltávolításához kövesse az alábbi lépéseket.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple pillanatkép Manager eltávolítása

1. Az állomáson nyissa meg a **Vezérlőpultot**, kattintson a **programok**, és válassza a **Programok és szolgáltatások**elemre.

2. A bal oldali ablaktáblában kattintson a **program módosítása vagy eltávolítása**gombra.

3. Kattintson a jobb gombbal a **StorSimple pillanatkép Manager**, és válassza az **Eltávolítás**gombra.

4. Ez a parancs elindítja a StorSimple pillanatkép Manager telepítőprogramja. Kattintson a **Beállítás módosítása**elemre, és válassza az **Eltávolítás**gombra.

    >[AZURE.NOTE] Ha a háttérben futó MMC folyamatokat, StorSimple pillanatkép vezető vagy lemez kezelése, például az eltávolítás sikertelen lesz, és zárja be az MMC összes előfordulását, mielőtt megkísérelne távolítsa el a program üzenetet kap. Jelölje ki a **automatikusan zárja be az alkalmazást, és próbálja meg újra kell indítania őket a telepítés befejezése után**, és kattintson **az OK**gombra.
 
5. Amikor az eltávolítási folyamat befejeződik, **A telepítő sikeres** üzenet jelenik meg. Kattintson a **Bezárás**gombra.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Lépés: 2: Az adatbázis biztonsági mentése StorSimple pillanatkép Manager

Kövesse az alábbi lépéseket létrehozása és mentése a StorSimple pillanatkép Manager-adatbázis másolata.

#### <a name="to-back-up-the-database"></a>Az adatbázis biztonsági másolatának

1. A Microsoft StorSimple alkalmazáskezelési szolgáltatás leállítása:

   1. Indítsa el a Kiszolgálókezelő.

   2. A kiszolgáló Manager irányítópultjával, kattintson az **eszközök** menü válassza ki a **szolgáltatásokat**.

   3. A **szolgáltatások** lapján jelölje be **A Microsoft StorSimple alkalmazáskezelési szolgáltatás**.

   4. A jobb oldali **Microsoft StorSimple alkalmazáskezelési szolgáltatás**csoportban kattintson **a szolgáltatás leállítása**.

        ![A StorSimple kezelő szolgáltatás leállítása](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. Keresse meg azt a C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData egy rejtett mappát.

3. Keresse meg a katalógus XML-fájlt, másolja át a fájlt, és a másolatot tárolni megbízható helyen vagy a felhőben.

    ![StorSimple biztonságimásolat-katalógus fájl](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Indítsa újra a Microsoft StorSimple alkalmazáskezelési szolgáltatás: 

    1. A kiszolgáló Manager irányítópultjával, kattintson az **eszközök** menü válassza ki a **szolgáltatásokat**.

    2. A **szolgáltatások** lapján jelölje be a **Microsoft StorSimple Management szolgáltatás**(e).

    3. A jobb oldali **Microsoft StorSimple alkalmazáskezelési szolgáltatás**csoportban kattintson a **Indítsa újra a szolgáltatást**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Lépés 3: Telepítse újra a StorSimple pillanatkép Manager, és az adatbázis visszaállítása

StorSimple pillanatkép Manager újratelepítést, kövesse a [egy új StorSimple pillanatkép Manager telepítése](#install-a-new-storsimple-snapshot-manager). Ezután a következő eljárással a StorSimple pillanatkép Manager-adatbázis visszaállítása.

#### <a name="to-restore-the-database"></a>Az adatbázis visszaállítása

1. A Microsoft StorSimple alkalmazáskezelési szolgáltatás leállítása:

    1. Indítsa el a Kiszolgálókezelő.

    2. A kiszolgáló Manager irányítópultjával, kattintson az **eszközök** menü válassza ki a **szolgáltatásokat**.

    3. A **szolgáltatások** lapján jelölje be **A Microsoft StorSimple alkalmazáskezelési szolgáltatás**.

    4. A jobb oldali **Microsoft StorSimple alkalmazáskezelési szolgáltatás**csoportban kattintson **a szolgáltatás leállítása**.

2. Keresse meg azt a C:\ProgramData\Microsoft\StorSimple\BACatalog. 

     >[AZURE.NOTE] ProgramData egy rejtett mappát.

3. Törölje a katalógus XML-fájlt, és cserélhet le a korábban mentett verziót.

4. Indítsa újra a Microsoft StorSimple alkalmazáskezelési szolgáltatás: 

    1. A kiszolgáló Manager irányítópultjával, kattintson az **eszközök** menü válassza ki a **szolgáltatásokat**.

    2. A **szolgáltatások** lapján jelölje be **A Microsoft StorSimple alkalmazáskezelési szolgáltatás**.

    3. A jobb oldali **Microsoft StorSimple alkalmazáskezelési szolgáltatás**csoportban kattintson a **Indítsa újra a szolgáltatást**.

## <a name="next-steps"></a>Következő lépések

- StorSimple pillanatkép Manager kapcsolatos további információért lépjen [StorSimple pillanatkép Manager?](storsimple-what-is-snapshot-manager.md).

- Többet szeretne tudni a StorSimple pillanatkép kezelő felhasználói felülettel, keresse fel a [StorSimple pillanatkép kezelő felhasználói felülettel](storsimple-use-snapshot-manager.md).

- StorSimple pillanatkép Manager használatával kapcsolatos további információért lépjen [StorSimple felügyelheti a StorSimple megoldás pillanatkép Managert használhatja](storsimple-snapshot-manager-admin.md).
