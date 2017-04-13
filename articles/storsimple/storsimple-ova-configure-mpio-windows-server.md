<properties 
   pageTitle="MPIO konfigurálása a StorSimple virtuális tömb állomáson |} Microsoft Azure"
   description="Megtudhatja, hogy miként többutas I/O (MPIO) beállítása a Windows Server 2012 R2 rendszer futtatása állomáshoz való csatlakozás virtuális tömb StorSimple."
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
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>A Windows Server host a StorSimple virtuális tömb többutas I/O konfigurálása

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogyan (MPIO) többutas I/O szolgáltatás telepítése a Windows Server-szolgáltató, csak StorSimple kötet speciális beállítások alkalmazása, és ellenőrizze a MPIO StorSimple kötet. Az eljárás feltételezi, hogy, hogy a StorSimple 1200 virtuális tömb két hálózati kapcsolódási pontok a két hálózati kapcsolatok és a Windows Server szolgáltató csatlakozik. Ez a cikk szereplő információk csak a virtuális tömb vonatkozik. StorSimple 8000 sorozat eszközön információért lépjen [Konfigurálása MPIO StorSimple állomás](storsimple-configure-mpio-windows-server.md). 

A Windows Server segít build erősen érhető el, hiba tervezett tároló konfigurációk MPIO szolgáltatást. MPIO felesleges fizikai elérési út összetevőket használ – adaptereken, a kábelek és a kapcsolók – a kiszolgáló és a tárolóeszközre közötti logikai elérési utak létrehozására. Ha a összetevő hibát okozó logikai elérési útját sikertelen, többutas logika használata egy alternatív útvonalon i/o-, hogy az alkalmazások továbbra is hozzáférhetnek az adataikhoz Ezenkívül attól függően, hogy a konfigurálás MPIO is teljesítménye növelhető újra terheléselosztás a betöltés minden elérési út keresztül. További tudnivalókért lásd: [MPIO áttekintés](https://technet.microsoft.com/library/cc725907.aspx "MPIO – áttekintés és szolgáltatások").  

A magas rendelkezésre állás a StorSimple megoldás állítsa be a Windows Server állomásokon csatlakozik a StorSimple 1200 virtuális tömbképletek (más néven a helyszíni virtuális eszköz) MPIO. A host kiszolgálók majd elviseli egy hivatkozást, hálózati vagy felület hiba. 

Meg kell MPIO konfigurálja a következő lépésekkel: 

- Konfigurációs vonatkozó követelmények

- Lépés: 1: Telepítse a Windows Server állomáson MPIO

- Lépés: 2: StorSimple kötet MPIO beállítása

- Lépés 3: Csatlakoztatási StorSimple kötet az állomáson

Az alábbi szakaszok a fenti lépéseket mindegyik Budai tárgyalja.


## <a name="prerequisites"></a>Előfeltételek

Ez a szakasz a Windows Server host és a virtuális tömb konfigurációs előfeltételei részletesen.

### <a name="on-windows-server-host"></a>A Windows Server állomáson

-  Győződjön meg arról, hogy a Windows Server-szolgáltató 2 hálózati kapcsolatok engedélyezve van-e.


### <a name="on-storsimple-virtual-array"></a>A virtuális tömb StorSimple

- A virtuális tömb iSCSI kiszolgáló kell beállítania. További tudnivalókért olvassa el a [virtuális tömb iSCSI kiszolgáló beállítása](storsimple-ova-deploy3-iscsi-setup.md)című témakört. Egy vagy több hálózati kapcsolaton akkor célszerű engedélyezni a tömb.   

- A virtuális tömbben lévő a hálózati kapcsolatok érhető el a Windows Server állomáswebhelyén kell lennie.

- Egy vagy több kötet a StorSimple virtuális tömb kell létrehozni. További tudnivalókért lásd: [hozzáadása a mennyiségi](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) a StorSimple 1200 virtuális tömb a. Az eljárás a virtuális tömb 3 térfogat (2 helyileg rögzített és 1 többszintű mennyiségi ahogy alább) létrehozott azt.
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Hardverkonfiguráció StorSimple virtuális tömb

Az alábbi ábrán a magas elérhetőség hardverkonfigurációja és a Windows Server host és StorSimple virtuális tömb az eljárás használt terheléselosztási többutas.  

![hardverkonfiguráció MPIO](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

A fenti ábrán látható:

- A Hyper-v kiépítéstől StorSimple virtuális tömb egyetlen csomópont aktív eszközről iSCSI-kiszolgálónak konfigurált.

- A tömb két virtuális hálózati kapcsolatok engedélyezettek. A helyi webes a 1200 virtuális tömb felhasználói felület ellenőrizze, hogy két hálózati kapcsolatok engedélyezve vannak-e meg a **Hálózati beállítások** navigálással alább látható módon:

    ![Hálózati kapcsolatok 1200 engedélyezve](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Megjegyzés: a IPv4-címei (Ethernet, alapértelmezés szerint Ethernet-2) engedélyezett hálózati kapcsolatok, és az állomáson későbbi felhasználás céljából menteni.

- Két hálózati kapcsolatok engedélyezve vannak a Windows Server állomáson. Ha a csatlakoztatott felületek host és tömbre ugyanahhoz az alhálózathoz, majd lesz 4 elérési utak érhető el. Ez volt a helyzet ezt az eljárást. Jó helyen jár Ha minden hálózati kapcsolat tömb és host felületén a különböző IP-alhálózat (és nem átirányítható), majd csak 2 elérési utak érhetők el.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Lépés: 1: Telepítse a Windows Server állomáson MPIO

MPIO Windows Serveren választható fiókfunkció, és alapértelmezés szerint nincs telepítve. Szolgáltatásként Kiszolgálókezelő segítségével kell telepíteni. Ez a szolgáltatás telepítése a Windows Server-szolgáltató, hajtsa végre az alábbi eljárást.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Lépés: 2: StorSimple kötet MPIO beállítása

MPIO kell beállítania, hogy a kötet StorSimple azonosítása. StorSimple kötet felismerje MPIO beállításához hajtsa végre az alábbi lépéseket.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Lépés 3: Csatlakoztatási StorSimple kötet az állomáson

MPIO konfigurálása után a Windows Server, a StorSimple tömb létrehozott kötet(ek) csatlakoztathatók, és majd kihasználhassa MPIO redundancia érdekében. A következő lépésekkel Kötet csatlakoztatásához.

#### <a name="to-mount-volumes-on-the-host"></a>Az állomáson kötet csatlakoztatni

1. Nyissa meg a **iSCSI kezdeményező tulajdonságok** ablak a Windows Server állomáson. Kattintson a **Kiszolgálókezelő > irányítópultok > eszközök > iSCSI kezdeményező**.
2. **ISCSI kezdeményező tulajdonságok** párbeszédpanelen kattintson a feltárás fülre, és válassza a **Cél portál felfedezése**.
3. **Cél portál felfedezése** párbeszédpanelen tegye a következőket:
    
    - A StorSimple virtuális tömbjét adja meg az első engedélyezett hálózati kapcsolat IP-címét. Alapértelmezés szerint ez lehet **Ethernet**. 
    - Kattintson **az OK gombra** kattintva visszatérhet a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen.

    >[AZURE.IMPORTANT] **Magánhálózat használatakor iSCSI kapcsolatok adja meg a személyes hálózathoz csatlakozik az adatok portot IP-címét.**

4. Ismételje meg a tömb lépéseket 2-3-as második hálózati kapcsolat (például Ethernet-2). 

5. Jelölje ki a **tárolók** lapon a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen. A virtuális tömb meg kell jelennie minden mennyiségi felületen cél **Talált tárolók**csoportban. Ebben az esetben a három célok (három kötet megfelelő) feltárással tenné.

    ![mpio1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. **Csatlakozás** egy, a StorSimple tömb iSCSI-munkamenet gombra. **Csatlakozás a cél** párbeszédpanel jelenik meg. Jelölje be a **több elem elérési út engedélyezése** jelölőnégyzetet. Kattintson a **Speciális**gombra.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. **Speciális beállítások** párbeszédpanelen tegye a következőket:                                       
    -    A **Helyi kártya** legördülő listára válassza a **Microsoft iSCSI kezdeményező**.
    -    A **Kezdeményező IP** legördülő listára válassza a host IP-címét.
    -    A **Cél portál** IP legördülő listában jelölje be a tömb felületének IP.
    -    Kattintson **az OK gombra** kattintva visszatérhet a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen.

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Kattintson a **Tulajdonságok**gombra. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. A **Tulajdonságok** párbeszédpanelen kattintson a **Munkamenet hozzáadása**elemre.

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. **Csatlakozás célba** párbeszédpanelen jelölje be a **több elem elérési út engedélyezése** jelölőnégyzetet. Kattintson a **Speciális**gombra.
11. A **Speciális beállítások** párbeszédpanelen:                                        
    -  A **helyi kártya** legördülő listára válassza a Microsoft iSCSI kezdeményező.
    -  A **Kezdeményező IP** legördülő listára válassza a host megfelelő IP-címét. Ebben az esetben meg a tömb két hálózati kapcsolatok egyetlen hálózat felhasználói felülete az állomáson csatlakozik. A kapcsolat ezért ugyanaz, mint az első munkamenet megadva.
    -  A **Target IP-portálon** legördülő listában jelölje ki az IP-cím engedélyezve van a tömb második adatok kapcsolathoz.
    -  Kattintson **az OK gombra** kattintva visszatérhet a iSCSI kezdeményező tulajdonságai párbeszédpanelen. A második munkamenet hozzáadta a célba.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Miután megadta a kívánt munkamenetek (elérési út), a **iSCSI kezdeményező tulajdonságai** párbeszédpanelen, jelölje be a cél, és válassza a **Tulajdonságok parancsot**. Az a **tulajdonságokat tartalmazó** párbeszédpanel munkamenetek lapon jegyezze fel a négy munkamenet, amelyek megfelelnek a lehetséges útvonal függvényérték azonosítóját. Munkamenet törléséhez jelölje be a jelölőnégyzetet a munkamenet azonosítóját, és válassza a **kapcsolat bontása**.
 
    - Munkamenetek belül bemutatni eszközök megtekintéséhez válassza az **eszközök** fülre. A kijelölt eszköz MPIO házirend beállításához kattintson a **MPIO**gombra. A **
    -  Részletek** párbeszédpanel jelenik meg. Kattintson a **MPIO** lapján választhatja ki a megfelelő **terheléselosztási házirend** beállítások. Is megtekintheti a **aktív** vagy **készenléti ** elérési út típusát.

10. Ismételje meg a 8-11-es további munkamenetek (elérési út) hozzáadása a cél. A két felületek az állomáson, és a virtuális tömb két négy munkamenetek mindegyik tárolóhoz összesen is hozzáadhat. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. Meg kell ismételje meg ezeket a lépéseket minden mennyiségi (cél felületek).

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. Nyissa meg a **Számítógép-kezelés** való navigálással **Kiszolgálókezelő > irányítópultok > Számítógép-kezelés**. A bal oldali ablaktáblában kattintson a **tároló > lemez Management**. A StorSimple virtuális tömb létrehozott jelenti, amelyeket az állomás vannak új lemez(ek) megjelennek a **Merevlemez-kezelés** .

13. A lemez inicializálni, és hozzon létre egy új mennyiségi. A formátum folyamat során válassza ki az terhelés egység (Ausztráliai) 64 KB. Az összes rendelkezésre álló kötet, ismételje meg az eljárást.

    ![Lemezen kezelése](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. A **Merevlemez-kezelés**felületen kattintson a jobb gombbal a **lemez** , és válassza a **Tulajdonságok parancsot**.

15. **Több elem elérési út lemezre eszköz tulajdonságok** párbeszédpanelen kattintson a **MPIO** fülre.

    ![Lemez tulajdonságai](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. **DSM neve** csoportban kattintson a **Részletek** gombra, és ellenőrizze, hogy az alapértelmezett paramétereket paramétereket vannak beállítva. Az alapértelmezett paramétereket a következők:

    - Elérési út ellenőrizze az időszak = 30
    - Újrapróbálkozások száma = 3
    - OEM eltávolítása időtartam = 20
    - Próbálkozás = 1
    - Elérési út ellenőrzése engedélyezett = bejelölve.

    >[AZURE.NOTE] **Ne módosítsa az alapértelmezett paramétereket.**


## <a name="next-steps"></a>Következő lépések

További tudnivalók [felügyelheti a StorSimple virtuális tömb az StorSimple kezelő szolgáltatás használatával](storsimple-ova-manager-service-administration.md).
 
