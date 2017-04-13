<properties 
   pageTitle="MPIO beállítása az StorSimple eszköz |} Microsoft Azure"
   description="Megtudhatja, hogy miként többutas I/O (MPIO) beállítása a Windows Server 2012 R2 rendszer futtatása állomáshoz való csatlakozás StorSimple eszköz."
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
   ms.workload="NA"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-for-your-storsimple-device"></a>Az StorSimple eszköz többutas I/O konfigurálása

A Microsoft beépített a többutas I/O (MPIO) a Windows Server súgója build erősen érhető el, hiba tervezett SAN konfigurációk szolgáltatás támogatása. MPIO felesleges fizikai elérési út összetevőket használ – adaptereken, a kábelek és a kapcsolók – a kiszolgáló és a tárolóeszközre közötti logikai elérési utak létrehozására. Ha a összetevő hibát okozó logikai elérési útját sikertelen, többutas logika használata egy alternatív útvonalon i/o-, hogy az alkalmazások továbbra is hozzáférhetnek az adataikhoz Ezenkívül attól függően, hogy a konfigurálás MPIO is teljesítménye növelhető újra terheléselosztás a betöltés minden elérési út keresztül. További tudnivalókért lásd: [MPIO áttekintés](https://technet.microsoft.com/library/cc725907.aspx "MPIO – áttekintés és szolgáltatások").  

Max-elérhetőségét a StorSimple megoldás MPIO kell konfigurálni az StorSimple eszközön. A Windows Server 2012 R2 futtató kiszolgálók MPIO van telepítve, amikor a kiszolgálókat majd elviseli egy hivatkozást, hálózati vagy felület hiba. 

MPIO Windows Serveren választható fiókfunkció, és alapértelmezés szerint nincs telepítve. Szolgáltatásként Kiszolgálókezelő segítségével kell telepíteni. Ez a témakör ismerteti a vonatkozó lépéseket kell követnie telepítése és futtatása a Windows Server 2012 R2 állomáson MPIO funkcióval, és egy StorSimple fizikai eszköz csatlakozik.

>[AZURE.NOTE] **Ez a lehetőség csak StorSimple 8000 sorozatok alkalmazható. MPIO jelenleg nem támogatott StorSimple virtuális eszközön.**

Meg kell MPIO konfigurálása StorSimple eszközén a következő lépésekkel:

- Lépés: 1: Telepítse a Windows Server állomáson MPIO

- Lépés: 2: StorSimple kötet MPIO beállítása

- Lépés 3: Csatlakoztatási StorSimple kötet az állomáson

- Lépés: 4: Magas elérhetősége MPIO konfigurálása és a terheléselosztás

Az alábbi szakaszok a fenti lépéseket mindegyik Budai tárgyalja.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Lépés: 1: Telepítse a Windows Server állomáson MPIO

Ez a szolgáltatás telepítése a Windows Server-szolgáltató, hajtsa végre az alábbi eljárást.

#### <a name="to-install-mpio-on-the-host"></a>A host MPIO telepítése

1. Nyissa meg a Windows Server-szolgáltató a Kiszolgálókezelő. Alapértelmezés szerint Kiszolgálókezelő elindul, amikor a Rendszergazdák csoport bejelentkezik egy számítógépre, a Windows Server 2012 R2 vagy Windows Server 2012 rendszert futtató egyik tagjával. Ha a Kiszolgálókezelő még nincs megnyitva, kattintson a **Start > Kiszolgálókezelő**.
![A Kiszolgálókezelő](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Kattintson a **Kiszolgálókezelő > irányítópultok > szerepkörök és szolgáltatások hozzáadása**. A **szerepkör hozzáadása és szolgáltatások** varázsló elindul.
![Szerepkörök és szolgáltatások varázsló 1 hozzáadása](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. **Szerepkörök hozzáadása és szolgáltatások** varázsló tegye a következőket:

    - **Első lépések** lapon kattintson a **Tovább**gombra.
    - A **telepítés típusának kiválasztása** lapon fogadja el az alapértelmezett **szerepköralapú vagy a szolgáltatás-alapú** telepítés. Kattintson a **Tovább**gombra. ![Szerepkörök és szolgáltatások varázsló 2 hozzáadása](./media/storsimple-configure-mpio-windows-server/IC740999.png)
    - **Jelölje be a cél kiszolgálón** lapján válassza **a kiszolgáló készletből kiszolgáló kiválasztása**. A kiszolgáló automatikusan talált kell lennie. Kattintson a **Tovább**gombra.
    - A **kiszolgálói szerepkörök kiválasztása** lapon kattintson a **Tovább**gombra.
    - **Szolgáltatások kiválasztása** lapon válassza ki a **Többutas I/O**, és kattintson a **Tovább**gombra. ![Szerepkörök és szolgáltatások varázsló 5 hozzáadása](./media/storsimple-configure-mpio-windows-server/IC741000.png)
    - A **Confirm telepítési beállítások** lapon erősítse meg a kijelölést és válassza a **automatikusan, ha szükséges, indítsa újra a célként megadott kiszolgáló**alább látható módon. Kattintson a **telepítés**gombra. ![Szerepkörök és szolgáltatások varázsló 8 hozzáadása](./media/storsimple-configure-mpio-windows-server/IC741001.png)
    - Kap értesítést jelenít meg, amikor befejeződött a telepítés. **Zárja be** a varázsló bezáráshoz kattintson. ![Szerepkörök és szolgáltatások varázsló 9 hozzáadása](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Lépés: 2: StorSimple kötet MPIO beállítása

MPIO kell beállítania, hogy a kötet StorSimple azonosítása. StorSimple kötet felismerje MPIO beállításához hajtsa végre az alábbi lépéseket.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>StorSimple kötet MPIO konfigurálása

1. Nyissa meg a **MPIO konfigurációs**. Kattintson a **Kiszolgálókezelő > irányítópultok > eszközök > MPIO**.

2. **MPIO tulajdonságai** párbeszédpanelen jelölje be a **Több elem görbék felfedezése** fülre.

3. Jelölje ki, **iSCSI eszközök támogatását**, és kattintson a **Hozzáadás**gombra.  
![MPIO tulajdonságok több elérési utak felfedezése](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. Indítsa újra a kiszolgálón.
5. **MPIO tulajdonságok** párbeszédpanelen kattintson a **MPIO eszközök** fülre. Kattintson a **hozzáadása**gombra.
    </br>![MPIO tulajdonságok MPIO eszközök](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. **MPIO-támogatás hozzáadása** párbeszédpanelen a **Hardver Eszközazonosítót**az eszköz időértékét adja meg. Az eszköz időértékét elérheti a StorSimple kezelő szolgáltatás megnyitása és lépés **Eszközök > irányítópult**. Az eszköz hányadik a **Quick Glance** ablaktábla, az eszköz irányítópult jobb jelenik meg.
    </br>![Adja hozzá a MPIO-támogatás](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Indítsa újra a kiszolgálón.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Lépés 3: Csatlakoztatási StorSimple kötet az állomáson

MPIO konfigurálása után a Windows Serveren, StorSimple az eszközre létrehozott kötet(ek) csatlakoztathatók, és majd kihasználhassa MPIO redundancia érdekében. A következő lépésekkel Kötet csatlakoztatásához.

#### <a name="to-mount-volumes-on-the-host"></a>Az állomáson kötet csatlakoztatni

1. Nyissa meg a **iSCSI kezdeményező tulajdonságok** ablak a Windows Server állomáson. Kattintson a **Kiszolgálókezelő > irányítópultok > eszközök > iSCSI kezdeményező**.
2. **ISCSI kezdeményező tulajdonságok** párbeszédpanelen kattintson a feltárás fülre, és válassza a **Cél portál felfedezése**.
3. **Cél portál felfedezése** párbeszédpanelen tegye a következőket:
    
    - Adja meg az adatok port StorSimple készüléke IP-címét (például adja meg az adatok 0).
    - Kattintson **az OK gombra** kattintva visszatérhet a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen.

    >[AZURE.IMPORTANT] **Magánhálózat használatakor iSCSI kapcsolatok adja meg a személyes hálózathoz csatlakozik az adatok portot IP-címét.**

4. Ismételje meg a 2-3-as második hálózati kapcsolat (például az adatok 1) az eszközön. Ne feledje, hogy ezek a felületek iSCSI engedélyezni. Kapcsolatos további információért olvassa el a [Módosítás hálózati kapcsolatok](storsimple-modify-device-config.md#modify-network-interfaces)című témakört.
5. Jelölje ki a **tárolók** lapon a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen. Meg kell jelennie a StorSimple eszköz cél IQN **Talált tárolók**csoportban.
 ![iSCSI kezdeményező tulajdonságok tárolók lapon](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Kattintson a **Csatlakozás** a StorSimple eszközzel egy iSCSI-munkamenet létrehozásához. **Csatlakozás a cél** párbeszédpanel jelenik meg.

7. **Csatlakozás célba** párbeszédpanelen jelölje be a **több elem elérési út engedélyezése** jelölőnégyzetet. Kattintson a **Speciális**gombra.

8. **Speciális beállítások** párbeszédpanelen tegye a következőket:                                       
    -    A **Helyi kártya** legördülő listára válassza a **Microsoft iSCSI kezdeményező**.
    -    A **Kezdeményező IP** legördülő listára válassza a host IP-címét.
    -    A **Cél portál** IP legördülő listában jelölje be az eszköz felületének IP.
    -    Kattintson **az OK gombra** kattintva visszatérhet a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen.

9. Kattintson a **Tulajdonságok**gombra. A **Tulajdonságok** párbeszédpanelen kattintson a **Munkamenet hozzáadása**elemre.
10. **Csatlakozás célba** párbeszédpanelen jelölje be a **több elem elérési út engedélyezése** jelölőnégyzetet. Kattintson a **Speciális**gombra.
11. A **Speciális beállítások** párbeszédpanelen:                                        
    -  A **helyi kártya** legördülő listára válassza a Microsoft iSCSI kezdeményező.
    -  A **Kezdeményező IP** legördülő listára válassza a host megfelelő IP-címét. Ebben az esetben az eszközre a két hálózati kapcsolatok egyetlen hálózat felhasználói felülete az állomáson csatlakozik. A kapcsolat ezért ugyanaz, mint az első munkamenet megadva.
    -  A **Target IP-portálon** legördülő listában jelölje ki a második adatok felület az eszközön engedélyezett IP-címét.
    -  Kattintson **az OK gombra** kattintva visszatérhet a iSCSI kezdeményező tulajdonságai párbeszédpanelen. A második munkamenet hozzáadta a célba.

12. Nyissa meg a **Számítógép-kezelés** való navigálással **Kiszolgálókezelő > irányítópultok > Számítógép-kezelés**. A bal oldali ablaktáblában kattintson a **tároló > lemez Management**. Az StorSimple eszközön létrehozott jelenti, amelyeket az állomás vannak új lemez(ek) megjelennek a **Merevlemez-kezelés** .

13. A lemez inicializálni, és hozzon létre egy új mennyiségi. A formátum folyamat során válassza ki a 64 KB blokk.
![Lemezen kezelése](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. A **Merevlemez-kezelés**felületen kattintson a jobb gombbal a **lemez** , és válassza a **Tulajdonságok parancsot**.
15. A StorSimple modell ### **Több elérési út lemez eszköz tulajdonságok** párbeszédpanelen kattintson a **MPIO** fülre.
![StorSimple 8100 több elérési út lemez DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. **DSM neve** csoportban kattintson a **Részletek** gombra, és ellenőrizze, hogy az alapértelmezett paramétereket paramétereket vannak beállítva. Az alapértelmezett paramétereket a következők:

    - Elérési út ellenőrizze az időszak = 30
    - Újrapróbálkozások száma = 3
    - OEM eltávolítása időtartam = 20
    - Próbálkozás = 1
    - Elérési út ellenőrzése engedélyezett = bejelölve.


>[AZURE.NOTE] **Ne módosítsa az alapértelmezett paramétereket.**

## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Lépés: 4: Magas elérhetősége MPIO konfigurálása és a terheléselosztás

Több elem elérési út alapuló magas rendelkezésre állásának és a terheléselosztás, több munkamenetek kell adható hozzá manuálisan deklarálhatnak a rendelkezésre álló különböző elérési utak. Például ha rendelkezik az állomás két felületek SAN csatlakozik az eszköz két felületek SAN csatlakoztatva van, majd (csak két munkamenetek lesz szükség, ha minden adat felület és a host felület egy másik IP-alhálózat pedig nem átirányítható) megfelelő elérési utat függvényérték konfigurált négy munkamenetek van szüksége.

>[AZURE.IMPORTANT] **Azt javasoljuk, hogy ne keverje 1 GbE és 10 GbE hálózati kapcsolatok. Két hálózati kapcsolatok használata esetén a mindkét felületek az azonos típusú kell lennie.**

Az alábbi eljárás munkamenetek, amikor egy StorSimple a két hálózati kapcsolatok csatlakozik a szolgáltató a két hálózati kapcsolatok hozzáadása ismerteti.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>MPIO magas rendelkezésre állásának és terheléselosztás

1. Végezze el a feltárás a cél: **iSCSI kezdeményező tulajdonságai** párbeszédpanelen a **Feltárás** lapon kattintson a **Felfedezése portálon**.
2. A **Csatlakozás a cél** párbeszédpanelen adja meg egy hálózati eszköz kapcsolatok IP-címét.
3. Kattintson **az OK gombra** kattintva visszatérhet a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen.

4. **ISCSI kezdeményező tulajdonságai** párbeszédpanelen jelölje be a **cél** fülre, jelölje ki az észlelt cél, és kattintson a **Csatlakozás**. A **Csatlakozás célba** párbeszédpanel jelenik meg.

5. A **Csatlakozás a cél** párbeszédpanelen:
    
    - Kilépés a a kijelölt alapértelmezett cél értékre **a kapcsolat** hozzáadása a kedvenc példányok listáját. Az eszközt, indítsa újra a kapcsolatot, minden alkalommal, amikor a számítógép újraindítása automatikusan megpróbálja így lesz.
    - Jelölje be a **több elem elérési út engedélyezése** jelölőnégyzetet.
    - Kattintson a **Speciális**gombra.

6. A **Speciális beállítások** párbeszédpanelen:
    - A **Helyi kártya** legördülő listára válassza a **Microsoft iSCSI kezdeményező**.
    - A **Kezdeményező IP** legördülő listára válassza a host IP-címét.
    - A **Target IP-portálon** legördülő listában jelölje ki a adatok felület az eszközön engedélyezett IP-címét.
    - Kattintson **az OK gombra** kattintva visszatérhet a iSCSI kezdeményező tulajdonságai párbeszédpanelen.

7. Kattintson a **Tulajdonságok**parancsra, majd kattintson a **Tulajdonságok** párbeszédpanelen a **Munkamenet hozzáadása**.

8. **Csatlakozás a cél** párbeszédpanelen jelölje be a **több elem elérési út engedélyezése** jelölőnégyzetet, és kattintson a **Speciális**gombra.

9. A **Speciális beállítások** párbeszédpanelen:
    1. A **helyi kártya** legördülő listára válassza a **Microsoft iSCSI kezdeményező**.
    2. A **Kezdeményező IP** legördülő listában jelölje ki a második felület az állomáson megfelelő IP-címét.
    3. A **Target IP-portálon** legördülő listában jelölje ki a második adatok felület az eszközön engedélyezett IP-címét.
    4. Kattintson **az OK gombra** kattintva visszatérhet a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen. Most már felvett egy második munkamenet a cél.

10. További munkamenetek (elérési út) hozzáadása a cél ismételje meg a 8-10. A két felületek az állomáson, és két az eszközön négy munkamenetek összesen is hozzáadhat.

11. Miután megadta a kívánt munkamenetek (elérési út), a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen, jelölje be a cél, és válassza a **Tulajdonságok parancsot**. Az a **tulajdonságokat tartalmazó** párbeszédpanel munkamenetek lapon jegyezze fel a négy munkamenet, amelyek megfelelnek a lehetséges útvonal függvényérték azonosítóját. Munkamenet törléséhez jelölje be a jelölőnégyzetet a munkamenet azonosítóját, és válassza a **kapcsolat bontása**.

12. Munkamenetek belül bemutatni eszközök megtekintéséhez válassza az **eszközök** fülre. A kijelölt eszköz MPIO házirend beállításához kattintson a **MPIO**gombra. Az **Eszköz részletei** párbeszédpanel jelenik meg. A **MPIO** lapon jelölje ki a megfelelő **Terheléselosztási házirend** -beállításokat. Megtekintheti az **aktív** vagy **készenléti** elérési út típusa is.

## <a name="next-steps"></a>Következő lépések

További tudnivalók [a StorSimple eszköz konfigurációjának módosítása az StorSimple kezelő szolgáltatás használatával](storsimple-modify-device-config.md).
 
