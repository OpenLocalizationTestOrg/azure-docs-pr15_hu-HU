<properties 
   pageTitle="Eszközkezelés StorSimple pillanatkép Manager |} Microsoft Azure"
   description="A StorSimple pillanatkép kezelő MMC beépülő modul használatával és StorSimple Eszközkezelés ismerteti."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>StorSimple pillanatkép Manager használatával és StorSimple eszközök kezelése

## <a name="overview"></a>– Áttekintés

Segítségével csomópontok a **hatókör** StorSimple pillanatkép kezelő ablaktáblán importált StorSimple eszköz adatainak ellenőrzése és frissítése a csatlakoztatott tárolók. Emellett az **eszközök** csomópont kattintáskor megjelenik csatlakoztatott eszközök és a megfelelő állapotinformációkat, az **eredmény** ablaktáblában.

![Csatlakoztatott eszközök](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Ábra 1: StorSimple pillanatkép Manager csatlakoztatott eszközt** 

Attól függően, hogy a **Nézet** beállítások az **eredmény** ablaktáblában az egyes az alábbi információkat jeleníti meg. (Nézet beállításával kapcsolatos további tudnivalókért nyissa meg [a Nézet menü](storsimple-use-snapshot-manager.md#view-menu).


| Eredmények oszlop  |Leírás          |
|:----------------|:--------------------| 
| név            | Az eszköz az Azure klasszikus portálon konfigurált neve|
| Modell           | Az eszköz modell száma|
| Verzió         | A szoftver az eszközre telepített verziója |
| Állapot          | E érhető el az eszközön |
| Az utolsó szinkronizálás     | Dátum és időpont, amikor utolsó szinkronizálása az eszköz |
| Sorszám      | Az eszköz időértékét |
 
Ha a jobb gombbal kattint a **hatókör** ablaktáblán az **eszközök** csomópont, akkor az alábbi műveletek közül választhat:

- Fel vagy cserél le egy eszközt 
- Csatlakoztassa a telefont, és ellenőrizze az import 
- Csatlakoztatott eszközök frissítése 

Ha kattintson az **eszközök** csomópontra, és ezután kattintson a jobb gombbal egy eszköz nevét, az **eredmény** ablaktáblában, akkor az alábbi műveletek közül választhat:

- Hitelesítő eszköz 
- Eszköz részletei 
- Egy eszköz frissítése 
- Eszköz konfiguráció törlése 
- Eszköz jelszavának módosítása

>[AZURE.NOTE] Minden műveletet is elérhetők a **Műveletek** ablaktáblában.
 
Ebből az oktatóanyagból megtudhatja, hogyan-kezelővel StorSimple pillanatképként való kapcsolódáshoz és eszközök kezeléséhez, és végezze el az alábbi műveleteket:

- Fel vagy cserél le egy eszközt 
- Csatlakoztassa a telefont, és ellenőrizze az import 
- Csatlakoztatott eszközök frissítése 
- Hitelesítő eszköz 
- Eszköz részletei 
- Az egyes eszköz frissítése 
- Eszköz konfiguráció törlése 
- Lejárt eszköz jelszó módosítása
- A hibás eszköz cseréje

>[AZURE.NOTE] Általános információt a StorSimple pillanatkép Manager felületén megnyitásához [StorSimple pillanatkép kezelő felhasználói felülettel](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Fel vagy cserél le egy eszközt

Az alábbi eljárással hozzáadása vagy cseréje StorSimple eszközt.

#### <a name="to-add-or-replace-a-device"></a>Hozzáadása vagy cseréje eszköz

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán kattintson a jobb gombbal a **eszközök** csomópontot, és kattintson a **beállítás egy eszközt**. Az **eszköz beállítása** párbeszédpanel jelenik meg.

    ![StorSimple eszköz beállítása](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3. Az **eszköz** legördülő párbeszédpanelen válassza az eszközök vagy virtuális IP-címét. 

4. A **jelszó** mezőbe írja be a StorSimple pillanatkép Manager jelszót, Ön által létrehozott az eszköz az Azure klasszikus portálon. Kattintson az **OK gombra**. StorSimple pillanatkép Manager azonosította, hogy az eszköz keres. 

    - Az eszköz érhető el, ha StorSimple pillanatkép Manager összeadja a kapcsolatot. 

    - Ha valamilyen okból az eszköz nem érhető el, a StorSimple pillanatkép Manager hibaüzenetet ad vissza. Kattintson az **OK gombra** kattintva zárja be a hibaüzenetet, és kattintson a **Mégse gombra** az **eszköz beállítása** párbeszédpanel bezárásához.

## <a name="connect-a-device-and-verify-imports"></a>Csatlakoztassa a telefont, és ellenőrizze az import

A következő eljárással csatlakozhat egy StorSimple eszközt, és ellenőrizze, hogy a program importálja a meglévő mennyiségi csoportokat, amely a társított biztonsági másolatok.

#### <a name="to-connect-a-device-and-verify-imports"></a>Csatlakoztassa a telefont, és ellenőrizze, hogy importálja

1. Csatlakoztassa a telefont a StorSimple pillanatkép Manager, kövesse a Hozzáadás gombra, és cserélje le egy eszközt. Amikor csatlakozik egy eszközt, StorSimple pillanatkép Manager válaszol az alábbi képlettel történik:

    - Ha valamilyen okból az eszköz nem érhető el, a StorSimple pillanatkép Manager hibaüzenetet ad vissza. 

   - Az eszköz érhető el, ha StorSimple pillanatkép Manager összeadja a kapcsolatot. Eszköz kijelölése, megjelenik az **eredményeket** tartalmazó ablaktáblában, és az állapot mező azt jelzi, hogy az eszköz esetén **érhető el**. StorSimple pillanatkép Manager importálja a mennyiségi csoportokat be van állítva az eszközt, feltéve, hogy a kötet csoportok társított biztonsági másolatok. Biztonsági házirendek nem importálja. Mennyiségi csoportok, amelyeknek nincs társított biztonsági másolatok nem importálja.

2. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

3. Kattintson a jobb gombbal a felső csomóponthoz a **hatókör** ablaktáblán, és kattintson az **Import megjelenítésének váltása**.

    ![Import megjelenítésének választó váltása](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4. A **Váltás import megjelenítése** párbeszédpanel jelenik meg, az importált mennyiségi csoportokat és a biztonsági másolatok állapotáról. Kattintson az **OK gombra**. 

Után a mennyiségi csoportokat és a biztonsági mentés sikeres importált, kezelővel StorSimple pillanatkép kezelheti őket, ahogyan szeretné kezelése a mennyiségi csoportok és a biztonsági másolatok létrehozott, és StorSimple pillanatkép Manager beállítva. 

## <a name="refresh-connected-devices"></a>Csatlakoztatott eszközök frissítése

A következő eljárással StorSimple csatlakoztatott eszközök StorSimple pillanatkép Manager szinkronizálása.

####<a name="to-refresh-connected-devices"></a>Csatlakoztatott eszközök frissítése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán kattintson a jobb gombbal az **eszközök**, és kattintson az **Eszközök frissítése**gombra. Ez szinkronizálja a csatlakoztatott eszközök StorSimple pillanatkép Manager, hogy a kötet csoportokat és a biztonsági másolatok, beleértve minden legújabb kiegészítéseinek tekinthet meg. 

    ![StorSimple eszközök frissítése](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
Az **Eszközök frissítése** művelet beolvassa csatlakoztatott eszközök mennyiségi új csoportokat és a összes társított biztonsági másolatot. Eltérően a **kötet újraellenőrzése** művelet elérhető a **kötet** csomópontot **Eszközök frissítése** nem sikerül visszaállítani a beállításjegyzék biztonsági mentése.

## <a name="authenticate-a-device"></a>Hitelesítő eszköz

Az alábbi eljárással hitelesíteni a StorSimple StorSimple pillanatkép kezelő eszközt.

#### <a name="to-authenticate-a-device"></a>Hitelesítést végezni eszköz

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán kattintson az **eszközök**.

3. Az **eredmény** ablaktáblában kattintson a jobb gombbal az eszközre, és kattintson a **hitelesítés**.

4. A **hitelesítés** párbeszédpanel jelenik meg. Írja be az eszközt jelszót, és kattintson **az OK**gombra.

    ![Hitelesítő párbeszédpanel](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## <a name="view-device-details"></a>Eszköz részletei

A következő eljárással StorSimple eszköz részleteinek megtekintése, és ha szükséges, szinkronizálnia a StorSimple pillanatkép kezelő eszközt.

#### <a name="to-view-and-resynchronize-device-details"></a>Megtekintése és az eszközadatok szinkronizálnia

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán kattintson az **eszközök**.

3. Az **eredmény** ablaktáblában kattintson a jobb gombbal az eszközre, és kattintson a **Részletek**gombra. 

4. az **Eszközadatok** párbeszédpanel jelenik meg. Ez a mező nevét, modell, verziója, sorozatszám, állapot, cél iSCSI mutatja minősített neve (IQN), és utolsó szinkronizálás dátuma és időpontja. 

   - Kattintson a **újraszinkronizálási** az eszköz szinkronizálása.

   - Kattintson az **OK** vagy a **Mégse gombra** kattintva zárja be a párbeszédpanelt.

    ![Eszköz részletei](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## <a name="refresh-an-individual-device"></a>Az egyes eszköz frissítése

Az alábbi eljárással az egyes StorSimple eszköz StorSimple pillanatkép Manager szinkronizálnia.

#### <a name="to-refresh-a-device"></a>Egy eszköz frissítése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához. 

2. A **hatókör** ablaktáblán kattintson az **eszközök**. 

3. Az **eredmény** ablaktáblában kattintson a jobb gombbal az eszközre, és kattintson az **Eszköz frissítése**. Ez StorSimple pillanatkép Manager szinkronizálása az eszköz. 

## <a name="delete-a-device-configuration"></a>Eszköz konfiguráció törlése

A következő eljárással törlése egy egyéni StorSimple eszköz konfigurációs StorSimple pillanatkép Manager alkalmazásból.

#### <a name="to-delete-a-device-configuration"></a>A eszköz konfiguráció törlése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához. 

2. A **hatókör** ablaktáblán kattintson az **eszközök**. 

3. Az **eredmény** ablaktáblában kattintson a jobb gombbal az eszközre, és kattintson a **Törlés**gombra. 

4. A következő üzenet jelenik meg. Kattintson az **Igen** törli a konfigurációt, vagy kattintson a **nincs** , a törlés megszakításához.

    ![Eszköz konfigurációs törlése](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Lejárt eszköz jelszó módosítása

Meg kell adnia a jelszó StorSimple eszköz StorSimple pillanatkép Manager hitelesítést végezni. Az eszköz beállítása a Windows PowerShell-felület használatakor beállíthatja a jelszót. A jelszó azonban is lejár. Ez történik, ha a jelszó módosítása az Azure klasszikus portal segítségével. Majd mert az eszköz StorSimple pillanatkép Manager van beállítva, a jelszó lejárta előtt, meg kell újra hitelesíteni az eszközt StorSimple pillanatkép Manager. 

#### <a name="to-change-the-expired-password"></a>A lejárt jelszó módosítása

1. Az Azure klasszikus portálon indítsa el a StorSimple kezelő szolgáltatás.

2. Kattintson az **eszközök** > **konfigurálása** az eszközhöz.

3. Görgessen le a StorSimple pillanatkép Manager szakasz. Írja be egy jelszót, 14-15 karakter. Győződjön meg arról, hogy a jelszó kombinálja a nagybetűsre, kisbetűsre, numerikus és különleges karaktereket tartalmaz-e.

4. Írja be újra a jelszót újra.

5. A lap alján a **Mentés** gombra.

#### <a name="to-re-authenticate-the-device"></a>Az eszköz újból hitelesíteni

1. Indítsa el a StorSimple pillanatkép Manager.

2. A **hatókör** ablaktáblán kattintson az **eszközök**. Konfigurált eszközök megjelenik az **eredmény** ablaktáblában. 

3. Jelölje ki az eszközt, kattintson a jobb gombbal, és kattintson **hitelesítés**.

4. A **hitelesítés** ablakban adja meg az új jelszót. 

5. Jelölje ki a eszközön kattintson a jobb gombbal, és válassza a **frissítés eszköz**. Ez StorSimple pillanatkép Manager szinkronizálása az eszköz. 

## <a name="replace-a-failed-device"></a>A hibás eszköz cseréje

Ha egy StorSimple eszközt nem sikerült, és helyébe készenléti (feladatátvétel) eszközt, kövesse az alábbi lépéseket az új eszközre csatlakozhat, és a kapcsolódó biztonsági másolatok megtekintése.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Új eszköz csatlakozhat feladatátvétel után

1. Újra a iSCSI kapcsolatot az új eszközre. Az utasításokat, kattintson a "7 lépés: csatlakoztatási initialize és formázása a mennyiségi" a [Deploy helyszíni StorSimple eszközére](storsimple-deployment-walkthrough-u2.md). 

>[AZURE.NOTE] Ha az új StorSimple eszköz ugyanazt a címet, a régit, bizonyára tudni importálni a régi konfiguráció. 

2. A Microsoft StorSimple alkalmazáskezelési szolgáltatás leállítása:

    1. Indítsa el a Kiszolgálókezelő.

    2. A kiszolgáló Manager irányítópultjával, kattintson az **eszközök** menü válassza ki a **szolgáltatásokat**. 

    3. A **szolgáltatások** ablakban jelölje ki a **Microsoft StorSimple alkalmazáskezelési szolgáltatás**. 

    4. A jobb oldali **Microsoft StorSimple alkalmazáskezelési szolgáltatás**csoportban kattintson **a szolgáltatás leállítása**. 

3. Távolítsa el a régi eszköz a konfigurációs információk: 

    1. A Fájlkezelőben nyissa meg a C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    2. A BACatalog mappában lévő fájlok törlése. 

4. Indítsa újra a Microsoft StorSimple alkalmazáskezelési szolgáltatás: 

    1. A kiszolgáló Manager irányítópultjával, kattintson az **eszközök** menü válassza ki a **szolgáltatásokat**. 

    2. A **szolgáltatások** ablakban jelölje ki a **Microsoft StorSimple alkalmazáskezelési szolgáltatás**. 

    3. A jobb oldali **Microsoft StorSimple alkalmazáskezelési szolgáltatás**csoportban kattintson a **Indítsa újra a szolgáltatást**. 

5. Indítsa el a StorSimple pillanatkép Manager. 

6. Adja meg az új StorSimple eszközt, hajtsa végre a lépéseket a 2: Csatlakozás [Üzembe StorSimple pillanatkép Manager](storsimple-snapshot-manager-deployment.md)StorSimple eszközt. 

7. Kattintson a jobb gombbal a legfelső szintű csomópontot a **hatókör** ablaktáblán (a példában StorSimple pillanatkép kezelő), és kattintson az **Import megjelenítésének váltása**. 

8. Megjelenik egy üzenet, ha az importált mennyiségi csoportokat és a biztonsági másolatok StorSimple pillanatkép Manager láthatók. Kattintson az **OK gombra**. 

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, [StorSimple pillanatkép Manager a StorSimple megoldás való](storsimple-snapshot-manager-admin.md)használatáról.
- Megtudhatja, hogyan [-kezelővel StorSimple pillanatkép megtekintéséhez és kezeléséhez kötet](storsimple-snapshot-manager-manage-volumes.md).

