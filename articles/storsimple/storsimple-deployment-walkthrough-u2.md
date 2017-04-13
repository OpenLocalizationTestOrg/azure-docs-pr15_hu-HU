<properties 
   pageTitle="Az StorSimple eszköz (frissítés 2) telepítése |} Microsoft Azure"
   description="Gyakorlati tanácsok a StorSimple frissítés 2 eszköz és a szolgáltatás üzembe helyezése és lépéseit ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="deploy-your-on-premises-storsimple-device-update-2"></a>A helyszíni StorSimple eszköz (frissítés 2) telepítése

> [AZURE.SELECTOR]
- [2 és későbbi frissítése](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [1 frissítése](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [Kiadás megjelenés](../articles/storsimple/storsimple-deployment-walkthrough.md)

## <a name="overview"></a>– Áttekintés

Üdvözli a Microsoft Azure StorSimple eszköz telepítési. Telepítési oktatóanyagokban StorSimple 8000 sorozat frissítés 2 vonatkoznak. Oktatóanyagok sorozata konfigurációs ellenőrzőlista konfigurációs Előfeltételek és StorSimple mobileszközére részletes konfigurálási lépéseket tartalmazza.

Az oktatóanyagokban információkat feltételezi, hogy rendelkezik a biztonságra, felül és kibontotta, racked, és StorSimple eszköze csatlakoztatva. Ha továbbra is kell azokat a feladatokat, kezdje a [biztonságra](storsimple-safety.md)áttekintése. Kövesse az eszköz adott kicsomagolása csatlakoztatási állvány és az eszköz kábelmodemek utasításokat.

- [Kicsomagolása csatlakoztatási állvány és a 8100 kábelmodemek](storsimple-8100-hardware-installation.md)
- [Kicsomagolása csatlakoztatási állvány és a 8600 kábelmodemek](storsimple-8600-hardware-installation.md)

Rendszergazdai jogosultságokkal a beállítás és konfiguráció befejezéséhez lesz szüksége. Azt javasoljuk, olvassa el a konfigurációs ellenőrzőlista megkezdése előtt. A telepítési és konfigurációs folyamat befejezéséhez egy kis időt vehet igénybe.

> [AZURE.NOTE] A StorSimple telepítési információ, a Microsoft Azure webhelyen közzétett StorSimple 8000 sorozat eszközök vonatkozik. Az adatsor 7000 eszközökkel kapcsolatos részletes információkért kattintson a: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 7000 sorozat telepítési információért [StorSimple rendszer rövid útmutató az első lépésekhez](http://onlinehelp.storsimple.com/111_Appliance/)talál.

## <a name="deployment-steps"></a>Telepítési lépéseket.

Állítsa be az StorSimple eszközt, és csatlakoztassa a StorSimple kezelő szolgáltatás szükséges lépések elvégzését. A szükséges lépéseket kívül vannak választható lépések és eljárások, előfordulhat, hogy a telepítés során. A részletes telepítési utasításokat végrehajtásakor kell a választható lépések jelzik.


| Lépés                                                                                   | Leírás                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ELŐFELTÉTELEK**                                                                      | Ezek kell elvégezni a közelgő telepítéshez előkészítése.                                                                                        |
| [Telepítési konfigurációs ellenőrzőlista](#deployment-configuration-checklist)                                                     | Ezzel az ellenőrzőlistával segítségével összegyűjtése és információk előtt és a telepítés során.                                                                       |
| [Telepítési előfeltételek](#deployment-prerequisites)                                                               | Ezek az érvényesítés a környezet telepítéshez készen áll a.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **RÉSZLETES TELEPÍTÉSI**                                                                   | Ezeket a lépéseket a telepítéshez használni kívánt gyártási StorSimple eszközt szükségesek.                                                                                      |
| [Lépés: 1: Hozzon létre egy új szolgáltatás](#step-1-create-a-new-service)                                                         | Állítsa be a felhő kezelését és tárolását StorSimple mobileszközére. *Kihagyhatja ezt a lépést, ha egy meglévő szolgáltatást más StorSimple eszközökre van*.                |
| [Lépés: 2: A szolgáltatás regisztrációs kulcs beszerzése](#step-2-get-the-service-registration-key)                                               | A kulcs segítségével regisztrálhatja és StorSimple eszköze kapcsolatba lépni a management szolgáltatásra.                                                                         |
| [Lépés 3: Konfigurálása és a Windows Powershellen keresztül az eszköz regisztrálhat StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)    | Az eszköz csatlakozzon a hálózathoz és regisztrálja az Azure az adatkezelési szolgáltatással telepítéséhez.                                            |
| [Lépés: 4: Minimális eszköz telepítéséhez](#step-4-complete-minimum-device-setupd)</br>[Nem kötelező: A StorSimple eszköz frissítése](#scan-for-and-apply-updates)      | Az alkalmazáskezelési szolgáltatás használatával az eszköz telepítéséhez, és engedélyezze, hogy tárhelyet biztosít.                                                                      |
| [5 lépés: A mennyiségi tároló létrehozása](#step-5-create-a-volume-container)                                                      | A tároló rendelkezést kötet létrehozása. Mennyiségi tároló tároló partner, a sávszélesség és a titkosítási beállításainak lévő összes kötet tartalmaz.    |
| [Lépés a 6: Kötet létrehozása](#step-6-create-a-volume)                                                                | Építse tároló kötet(ek) a kiszolgálók StorSimple eszközre.                                                                                        |
| [7 lépés: Csatlakoztatása inicializálni és kötet formázása](#step-7-mount-initialize-and-format-a-volume)</br>[Nem kötelező: MPIO konfigurálása](storsimple-configure-mpio-windows-server.md)            | Csatlakozás a kiszolgálókat az eszköz által biztosított iSCSI tárolására. Igény szerint állítsa be az annak érdekében, hogy a kiszolgálók elviseli hivatkozás és hálózati kapcsolat hiba MPIO.                                                                                                                                                              |
| [8 lépés: A biztonsági másolat készítése](#step-8-take-a-backup)                                                                  | Az adatok védelme érdekében a biztonsági másolat házirend beállítása                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **MÁS ELJÁRÁSOK**                                                                   | Előfordulhat, hogy hivatkozni ezeket a műveleteket, mint amikor a megoldást üzembe helyez.                                                                                        |
| [A szolgáltatás új tároló fiók beállítása](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [GITT használata a soros eszköz-konzolon](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [A Windows Server állomás IQN beszerzése](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Hozzon létre egy kézi biztonsági mentése](#create-a-manual-backup)                                                                 | 


## <a name="deployment-configuration-checklist"></a>Telepítési konfigurációs ellenőrzőlista

Mielőtt beállítaná az eszközön, szüksége lesz a szoftver konfigurálása StorSimple eszközén vonatkozó információk gyűjtését. Felkészülés az adatok időszakokra egy részét súgó korszerűsíthessék az üzembe helyezése a StorSimple eszköz a környezetben. Töltse le és telepít, az eszköz beállításainak részleteit lefelé Megjegyzés: Ezzel az ellenőrzőlistával használatával.

- [Töltse le a StorSimple telepítési konfigurációs ellenőrzőlista](http://www.microsoft.com/download/details.aspx?id=49159)


## <a name="deployment-prerequisites"></a>Telepítési előfeltételek

Az alábbi szakaszok ismertetik a StorSimple kezelő szolgáltatás és az StorSimple eszköz konfigurációs előfeltételei.

### <a name="for-the-storsimple-manager-service"></a>A StorSimple kezelő szolgáltatás

Mielőtt elkezdené, győződjön meg arról, hogy:

- Ha a Microsoft-fiók hozzáférési hitelesítő adatokkal.

- Van Microsoft Azure tároló fiókja, az access hitelesítő adatokkal.

- Microsoft Azure-előfizetése engedélyezve van a StorSimple kezelő szolgáltatás. Az előfizetés az [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/)keresztül kell vásárolni.

- Terminálemuláció szoftver például gitt hozzáférése van.

### <a name="for-the-device-in-the-datacenter"></a>Az eszköz a adatközponthoz

Az eszköz beállítása, előtt győződjön meg róla, hogy az eszköz teljesen kibontotta, állványon csatlakoztatott és power, hálózati és leírtak szerint a soros access teljesen csatlakoztatva:

-  [Kicsomagolása csatlakoztatási állvány és kábelmodemek 8100 eszköze](storsimple-8100-hardware-installation.md)
-  [Kicsomagolása csatlakoztatási állvány és kábelmodemek 8600 eszköze](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>A hálózati a adatközponthoz

Mielőtt elkezdené, győződjön meg arról, hogy:

- Az az Adatközpont tűzfal portokat engedélyezni szeretné a iSCSI, és a felhő forgalmat a [hálózati StorSimple eszközére vonatkozó követelmények](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)ismertetett módon lettek megnyitva.


## <a name="step-by-step-deployment"></a>Részletes telepítési

A következő lépésenkénti utasítások használatával a adatközponthoz StorSimple eszközére.

## <a name="step-1-create-a-new-service"></a>Lépés: 1: Hozzon létre egy új szolgáltatás

Egy StorSimple kezelő szolgáltatás több StorSimple is kezelheti. A következő lépésekkel hozzon létre egy új példányát az StorSimple kezelő szolgáltatás.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Ha az automatikus létrehozása a tárterület-fiók nem engedélyezte a szolgáltatással, szüksége lesz legalább egy tárterület-fiók létrehozása, miután létrehozott egy szolgáltatás sikeresen. Ez a tárterület-fiók a mennyiségi tároló létrehozásakor lesz. 
>
> * Ha nem Ön hozta létre a tárterület-fiók automatikusan, nyissa meg [a szolgáltatás új tároló fiók konfigurálása](#configure-a-new-storage-account-for-the-service) a részletes útmutatást. 
> * Ha engedélyezte a tárterület-fiók automatikus létrehozása, nyissa meg [2 lépés: a szolgáltatás regisztrációs kulcs első](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Lépés: 2: A szolgáltatás regisztrációs kulcs beszerzése

Miután a StorSimple kezelő szolgáltatás lépéseket, szüksége lesz a szolgáltatás regisztrációs kulcs első. A kulcs regisztrálhat, és az StorSimple eszköz kapcsolatba lépni a szolgáltatást használják.

Hajtsa végre az alábbi lépéseket az adatkezelési portálon.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Lépés 3: Konfigurálása és a Windows Powershellen keresztül az eszköz regisztrálhat StorSimple

Használja a Windows PowerShell-StorSimple StorSimple eszköztől az alábbi eljárás magyarázata kezdeti telepítéséhez. Meg kell terminálemulációs szoftver használatával ezt a lépést. További tudnivalókért lásd: [A soros eszköz-konzol csatlakozhat használata gitt](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Lépés: 4: Minimális eszköz telepítéséhez

Az StorSimple eszközön minimális eszköz konfigurálását akkor szükségesek: 

- Állítsa be a másodlagos DNS-kiszolgáló.
- Engedélyezze a iSCSI legalább egy hálózati kapcsolaton.
- Rögzített IP-címek hozzárendelése mindkét vezérlők.

A felügyeleti portálon a minimális eszköz telepítéséhez kövesse az alábbi lépéseket.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>5 lépés: A mennyiségi tároló létrehozása

Mennyiségi tároló tároló partner, a sávszélesség és a titkosítási beállításainak lévő összes kötet tartalmaz. Meg kell létrehozása a mennyiségi tároló kötet StorSimple eszközén kiépítési megkezdése előtt. 

Az adatkezelési portálon mennyiségi tároló létrehozásához kövesse az alábbi lépéseket.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Lépés a 6: Kötet létrehozása

Miután létrehozta a mennyiségi tároló, meg is kötet létrehozása tároló StorSimple az eszközre a kiszolgálók. Az adatkezelési portálon kötet létrehozásához kövesse az alábbi lépéseket.

> [AZURE.IMPORTANT] StorSimple Manager mindkét vékony, és a teljes mértékben kiépített kötet hozhat létre. Részben kiépített kötet azonban nem hozhatók létre. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>7 lépés: Csatlakoztatása inicializálni és kötet formázása

Az alábbi lépéseket a Windows Server állomáson történik. 


> [AZURE.IMPORTANT]

> - A magas, a StorSimple megoldás rendelkezésre állásának ajánlott úgy beállítani, hogy MPIO kiszolgálókon a host (nem kötelező) iSCSI beállítása előtt. Host kiszolgálókon MPIO konfigurációs biztosítja, hogy a kiszolgálókat elviseli egy hivatkozást, hálózati vagy felület hiba.

> - MPIO és iSCSI telepítési és konfigurálási utasítások Windows Server állomáson lépjen a [Konfigurálása MPIO StorSimple mobileszközére](storsimple-configure-mpio-windows-server.md). Ez is magában foglalja az csatlakoztatásához, inicializálni és formázhatja StorSimple őket a lépéseket.

> - Utasításokért MPIO és iSCSI telepítési és konfigurálási Linux állomáson nyissa meg [A StorSimple Linux szolgáltatója konfigurálása MPIO](storsimple-configure-mpio-on-linux.md)

Ha úgy dönt, nem kell konfigurálni MPIO, hajtsa végre az alábbi lépések elvégzésével csatlakoztatása inicializálni és formázhatja a StorSimple őket a Windows Server állomáson.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>8 lépés: A biztonsági másolat készítése

Biztonsági másolatok mennyiségének pont és az idő védelmet nyújtanak, és javíthatja a helyreállítás: visszaállítása időpontok minimalizálása közben. Kétféle biztonsági másolatot készíthet az StorSimple eszközön: helyi állapotaira és a felhő pillanatképek. **Ütemezett** és a **kézi**mindegyik biztonsági másolat is lehet. 

A következő lépésekkel ütemezett biztonsági másolat létrehozása a felügyeleti portálon.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Készíthet egy kézi biztonsági bármikor. Lépjen a bemutatókból, létrehozása [egy kézi biztonsági mentést](#create-a-manual-backup). 

## <a name="configure-a-new-storage-account-for-the-service"></a>A szolgáltatás új tároló fiók beállítása

Ez a egy nem kötelező lépés, amely csak akkor, ha az automatikus létrehozása a tárterület-fiók nem engedélyezte a szolgáltatással végrehajtásához. A Microsoft Azure tároló fiók StorSimple mennyiségi tároló létrehozásához szükséges.

Ha szeretne egy másik régióbeli Azure tároló fiókot létrehozni, olvassa el a [Azure tároló fiókokat kapcsolatban](../storage/storage-create-storage-account.md) részletes útmutatást.

Hajtsa végre az alábbi lépéseket az adatkezelési portálon **StorSimple kezelő szolgáltatás** lapon.

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>GITT használata a soros eszköz-konzolon

Kapcsolódás a Windows PowerShell-StorSimple, kell például gitt terminálemuláció szoftver használatához. Az eszköz közvetlenül a soros konzol, vagy úgy, hogy megnyitja a telnet munkamenet távoli számítógépről történő elérésekor gitt használhatja.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]


## <a name="scan-for-and-apply-updates"></a>Keressen és -frissítések telepítése

Az eszköz frissítése több órát is igénybe vehet. Hajtsa végre az alábbi lépéseket a frissítések keresése és az eszközön.
<!--can take 1-4 hours--> 

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Az eszköz frissítése

1.  Eszköz **Első lépések** lapon kattintson az **eszközök**. Válassza ki a fizikai eszközt, kattintson a **Karbantartás** elemre, és kattintson a **Beolvasás frissítések**.  

2.  Az elérhető frissítések keresése a feladat jön létre. Ha frissítések érhetők el, a **Frissítések beolvasása a** **Frissítések telepítése**változik. Kattintson a **frissítések telepítése**gombra. 

3.  Egy frissítési feladat jön létre. A frissítés állapotának figyelése navigálással **feladatokat**.

    > [AZURE.NOTE] A frissítés feladat indításakor azonnal megjeleníti az állapot 50 százalékos értékként. A 100 %-os csak a frissítés feladat befejeződése után állapota megváltozik. A frissítési folyamat nem valós idejű állapota nem.

4.  Miután sikeresen frissül az eszközt, adatok 2 és 3 adatok hálózati kapcsolatok engedélyezése, ha ezek le voltak tiltva.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## <a name="get-the-iqn-of-a-windows-server-host"></a>A Windows Server állomás IQN beszerzése

A következő lépésekkel veheti a iSCSI minősített a nevét (IQN) a Windows Server® 2012-kiszolgáló állomás Windows.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Hozzon létre egy kézi biztonsági mentése

Az adatkezelési portálon hozhat létre az igény szerinti kézi biztonsági mentést egyetlen kötet StorSimple eszközén, hajtsa végre az alábbi lépéseket.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]


## <a name="next-steps"></a>Következő lépések

- [Virtuális eszköz](storsimple-virtual-device-u2.md)beállítása.

- A [StorSimple kezelő szolgáltatás](storsimple-manager-service-administration.md) használatával StorSimple eszköze kezelését.
 
