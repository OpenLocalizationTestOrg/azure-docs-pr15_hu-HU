<properties
   pageTitle="Kormányzati portálon StorSimple eszköz telepítése |} Microsoft Azure"
   description="Gyakorlati tanácsok a StorSimple frissítés 2 eszköz és az Azure kormányzati portálon szolgáltatás üzembe helyezése és lépéseit ismerteti."
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
   ms.date="08/16/2016"
   ms.author="v-sharos" />

# <a name="deploy-your-on-premises-storsimple-device-in-the-government-portal-update-2"></a>A kormányzati portálon (frissítés 2), a helyszíni StorSimple eszköz telepítése

[AZURE.INCLUDE [storsimple-version-selector-deploy-gov](../../includes/storsimple-version-selector-deploy-gov.md)]

## <a name="overview"></a>– Áttekintés

Üdvözli a Microsoft Azure StorSimple eszköz telepítési. Telepítési oktatóanyagokban alkalmazása a frissítés 2 szoftver fut az Azure kormányzati portálon StorSimple 8000 sorozat. Oktatóanyagok sorozata konfigurációs ellenőrzőlista, konfigurációs Előfeltételek és részletes konfigurálási lépéseket StorSimple mobileszközére listáját tartalmazza.

Az oktatóanyagokban információkat feltételezi, hogy rendelkezik a biztonságra, felül és kibontotta, racked, és StorSimple eszköze csatlakoztatva. Ha továbbra is kell azokat a feladatokat, kezdje a [biztonságra](storsimple-safety.md)áttekintése. Az eszköz-specifikus útmutatás kicsomagolása, csatlakoztatási állvány és kábelmodemek az eszközön.

- [Kicsomagolása csatlakoztatási állvány és a 8100 kábelmodemek](storsimple-8100-hardware-installation.md)
- [Kicsomagolása csatlakoztatási állvány és a 8600 kábelmodemek](storsimple-8600-hardware-installation.md)

Rendszergazdai jogosultságokkal a beállítás és konfiguráció befejezéséhez lesz szüksége. Azt javasoljuk, olvassa el a konfigurációs ellenőrzőlista megkezdése előtt. A telepítési és konfigurációs folyamat befejezéséhez egy kis időt vehet igénybe.

> [AZURE.NOTE] A StorSimple telepítési információ, a Microsoft Azure webhelyen közzétett StorSimple 8000 sorozat eszközök vonatkozik. Az adatsor 7000 eszközökkel kapcsolatos részletes információkért kattintson a: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 7000 sorozat telepítési információért [StorSimple rendszer rövid útmutató az első lépésekhez](http://onlinehelp.storsimple.com/111_Appliance/)talál.

## <a name="deployment-steps"></a>Telepítési lépéseket.

Állítsa be az StorSimple eszközt, és csatlakoztassa a StorSimple kezelő szolgáltatás szükséges lépések elvégzését. A szükséges lépéseket kívül vannak választható lépések és eljárások, előfordulhat, hogy a telepítés során befejezéséhez. A részletes telepítési utasításokat végrehajtásakor kell a választható lépések jelzik.


| Lépés                                                                                   | Leírás                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ELŐFELTÉTELEK**                                                                      | Ezek kell elvégezni a közelgő telepítéshez előkészítése.                                                                                        |
|[Telepítési konfigurációs ellenőrzőlista](#deployment-configuration-checklist)                                                     | Ezzel az ellenőrzőlistával segítségével összegyűjtése és információk előtt és a telepítés során.                                                                       |
| [Telepítési előfeltételek](#deployment-prerequsites)                                                               | Ezek ellenőrzése, hogy telepítéshez készen áll-e a környezetet.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **RÉSZLETES TELEPÍTÉSI**                                                                   | Ezeket a lépéseket a telepítéshez használni kívánt StorSimple eszközt gyártási szükségesek.                                                                                      |
| [Lépés: 1: Hozzon létre egy új szolgáltatás](#step-1-create-a-new-service)                                                         | Állítsa be a felhő kezelését és tárolását StorSimple mobileszközére. *Kihagyhatja ezt a lépést, ha egy meglévő szolgáltatást más StorSimple eszközökre van*.              |
| [Lépés: 2: A szolgáltatás regisztrációs kulcs beszerzése](#step-2-get-the-service-registration-key)                                               | A kulcs használatával regisztrálni és StorSimple eszköze kapcsolatba lépni a alkalmazáskezelési szolgáltatás.                                                                         |
| [Lépés 3: Konfigurálása és a Windows Powershellen keresztül az eszköz regisztrálhat StorSimple](#step 3-configure-and-register-the-device-through-windows-powershell-for-storsimple) | Az eszköz csatlakozzon a hálózathoz és regisztrálja az Azure az adatkezelési szolgáltatással telepítéséhez.                                            |
| [Lépés: 4: A legkisebb eszköz telepítéséhez](#step-4-complete-the-minimum-device-setup) </br>Nem kötelező: Az StorSimple eszköz frissítése | Az alkalmazáskezelési szolgáltatás használatával az eszköz telepítéséhez, és engedélyezze, hogy tárhelyet biztosít.                                                                      |
| [5 lépés: A mennyiségi tároló létrehozása](#step-5-create-a-volume-container)                                                      | A tároló rendelkezést kötet létrehozása. Mennyiségi tároló tároló partner, a sávszélesség és a titkosítási beállításainak lévő összes kötet tartalmaz.    |
| [Lépés a 6: Kötet létrehozása](#step-6-create-a-volume)                                                               | Építse tároló kötet(ek) a kiszolgálók StorSimple eszközre.                                                                                        |
| [7 lépés: Csatlakoztatása inicializálni és kötet formázása](#step-7-mount-initialize-and-format-a-volume) </br>Nem kötelező: Állítsa be a MPIO.            | Csatlakozás a kiszolgálókat az eszköz által biztosított iSCSI tárolására. Tetszés szerint állítsa be az annak érdekében, hogy a kiszolgálók elviseli a hivatkozást, a hálózati és a felület hiba MPIO.                                                                                                                                                              |
| [8 lépés: A biztonsági másolat készítése](#step-8-take-a-backup)                                                                  | Az adatok védelme érdekében a biztonsági másolat házirend beállítása                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **MÁS ELJÁRÁSOK**                                                                   | Előfordulhat, hogy hivatkozni ezeket a műveleteket, mint amikor a megoldást üzembe helyez.                                                                                    |
| [A szolgáltatás új tároló fiók beállítása](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [GITT használata a soros eszköz-konzolon](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [Keressen és -frissítések telepítése](#scan-for-and-apply-updates)                                                   |                                                                                                                                                               |
| [A Windows Server állomás IQN beszerzése](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Hozzon létre egy kézi biztonsági mentése](#create-a-manual-backup)                                                                 |
| [MPIO konfigurálása](#configure-mpio)                                                                          |

## <a name="deployment-configuration-checklist"></a>Telepítési konfigurációs ellenőrzőlista

Mielőtt beállítaná a StorSimple eszköz, szüksége lesz a szoftver konfigurálása mobileszközön vonatkozó információk gyűjtését. Felkészülés az adatok időszakokra egy részét, megtanulása hozzásegíti korszerűsíthessék az üzembe helyezése a StorSimple eszköz a környezetben. Töltse le és tartsa szem előtt a konfigurációs, az eszköz telepítése ezzel az ellenőrzőlistával használatával.  

[Töltse le a StorSimple telepítési konfigurációs ellenőrzőlista](http://www.microsoft.com/download/details.aspx?id=49159)  

## <a name="deployment-prerequisites"></a>Telepítési előfeltételek

Az alábbi szakaszok ismertetik a StorSimple kezelő szolgáltatás és az StorSimple eszköz konfigurációs előfeltételei.

### <a name="for-the-storsimple-manager-service"></a>A StorSimple kezelő szolgáltatás

Mielőtt elkezdené, győződjön meg arról, hogy:

- Ha a Microsoft-fiók hozzáférési hitelesítő adatokkal.

- Van Microsoft Azure tároló fiókja, az access hitelesítő adatokkal.

- Microsoft Azure-előfizetése engedélyezve van a StorSimple kezelő szolgáltatás. Az előfizetés az [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/)keresztül kell vásárolni.

- Terminálemuláció szoftver például gitt hozzáférése van.

### <a name="for-the-device-in-the-datacenter"></a>Az eszköz a adatközponthoz

Az eszköz beállítása, előtt győződjön meg arról, hogy:

- Az eszköz teljesen kibontotta, állványon csatlakoztatott és teljesen csatlakoztatva power, a hálózati és a soros access leírtak szerint:

    -  [Kicsomagolása csatlakoztatási állvány és kábelmodemek 8100 eszköze](storsimple-8100-hardware-installation.md)
    -  [Kicsomagolása csatlakoztatási állvány és kábelmodemek 8600 eszköze](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>A hálózati a adatközponthoz

Mielőtt elkezdené, győződjön meg arról, hogy:

- Az az Adatközpont tűzfal portokat engedélyezni szeretné a iSCSI, és a felhő forgalmat a [hálózati StorSimple eszközére vonatkozó követelmények](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)ismertetett módon lettek megnyitva.

## <a name="step-by-step-deployment"></a>Részletes telepítési

A következő lépésenkénti utasítások használatával a adatközponthoz StorSimple eszközére.

## <a name="step-1-create-a-new-service"></a>Lépés: 1: Hozzon létre egy új szolgáltatás

Egy StorSimple kezelő szolgáltatás több StorSimple is kezelheti. A következő lépésekkel hozzon létre egy új példányát az StorSimple kezelő szolgáltatás.

[AZURE.INCLUDE [storsimple-create-new-service-gov](../../includes/storsimple-create-new-service-gov.md)]

> [AZURE.IMPORTANT] Ha az automatikus létrehozása a tárterület-fiók nem engedélyezte a szolgáltatással, szüksége lesz legalább egy tárterület-fiók létrehozása, miután létrehozott egy szolgáltatás sikeresen. Ez a tárterület-fiók a mennyiségi tároló létrehozásakor lesz.
>
> * Ha nem Ön hozta létre a tárterület-fiók automatikusan, nyissa meg [a szolgáltatás új tároló fiók konfigurálása](#configure-a-new-storage-account-for-the-service) a részletes útmutatást.
> * Ha engedélyezte a tárterület-fiók automatikus létrehozása, nyissa meg [2 lépés: a szolgáltatás regisztrációs kulcs első](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Lépés: 2: A szolgáltatás regisztrációs kulcs beszerzése

Miután a StorSimple kezelő szolgáltatás lépéseket, szüksége lesz a szolgáltatás regisztrációs kulcs első. A kulcs regisztrálhat, és csatlakoztassa az StorSimple eszközt a szolgáltatást használják.

Hajtsa végre az alábbi lépéseket a kormányzati portálon.

[AZURE.INCLUDE [storsimple-get-service-registration-key-gov](../../includes/storsimple-get-service-registration-key-gov.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Lépés 3: Konfigurálása és a Windows Powershellen keresztül az eszköz regisztrálhat StorSimple

Használja a Windows PowerShell-StorSimple StorSimple eszköztől az alábbi eljárás magyarázata kezdeti telepítéséhez. Meg kell terminálemulációs szoftver használja ezt a lépést. További tudnivalókért lásd: [A soros eszköz-konzol csatlakozhat használata gitt](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-gov](../../includes/storsimple-configure-and-register-device-gov-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Lépés: 4: Minimális eszköz telepítéséhez

Az StorSimple eszközön minimális eszköz konfigurálását akkor szükségesek:

- Állítsa be a másodlagos DNS-kiszolgáló.
- Engedélyezze a iSCSI legalább egy hálózati kapcsolaton.
- Rögzített IP-címek hozzárendelése mindkét vezérlők.

A kormányzati portálon a minimális eszköz telepítéséhez kövesse az alábbi lépéseket.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>5 lépés: A mennyiségi tároló létrehozása

Mennyiségi tároló tároló partner, a sávszélesség és a titkosítási beállításainak lévő összes kötet tartalmaz. Meg kell létrehozása a mennyiségi tároló kötet StorSimple eszközén kiépítési megkezdése előtt.

A kormányzati portálon mennyiségi tároló létrehozásához kövesse az alábbi lépéseket.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Lépés a 6: Kötet létrehozása

Miután létrehozta a mennyiségi tároló, meg is kötet létrehozása tároló StorSimple az eszközre a kiszolgálók. A kormányzati portálon kötet létrehozásához kövesse az alábbi lépéseket.

> [AZURE.IMPORTANT] Azure StorSimple csak vékonyan kiépített kötet hozható létre.  Nem hozhatók létre kiépített teljesen vagy részben kiépített kötet az Azure StorSimple rendszeren.

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>7 lépés: Csatlakoztatása inicializálni és kötet formázása

Hajtsa végre ezeket a lépéseket a Windows Server állomáson.

> [AZURE.IMPORTANT]

> - A magas, a StorSimple megoldás rendelkezésre állásának ajánlott úgy beállítani, hogy MPIO kiszolgálókon a host (nem kötelező) iSCSI beállítása előtt. Host kiszolgálókon MPIO konfigurációs biztosítja, hogy a kiszolgálókat elviseli egy hivatkozást, hálózati vagy felület hiba.

> - MPIO és iSCSI telepítési és konfigurálási utasítások Windows Server állomáson lépjen a [Konfigurálása MPIO StorSimple mobileszközére](storsimple-configure-mpio-windows-server.md). Ez is magában foglalja az csatlakoztatásához, inicializálni és formázhatja StorSimple őket a lépéseket.

> - Utasításokért MPIO és iSCSI telepítési és konfigurálási Linux állomáson nyissa meg [A StorSimple Linux szolgáltatója konfigurálása MPIO](storsimple-configure-mpio-on-linux.md)

Ha úgy dönt, nem kell konfigurálni MPIO, hajtsa végre az alábbi lépésekkel csatlakoztatásához, inicializálni és formázhatja a StorSimple őket a Windows Server állomáson.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>8 lépés: A biztonsági másolat készítése

Biztonsági másolatok mennyiségének pont és az idő védelmet nyújtanak, és javíthatja a helyreállítás: visszaállítása időpontok minimalizálása közben. Kétféle biztonsági másolatot készíthet az StorSimple eszközön: helyi állapotaira és a felhő pillanatképek. **Ütemezett** és a **kézi**mindegyik biztonsági másolat is lehet.

A következő lépésekkel ütemezett biztonsági másolat létrehozása a kormányzati portálon.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Készíthet egy kézi biztonsági bármikor. Lépjen a bemutatókból, létrehozása [egy kézi biztonsági mentést](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-the-service"></a>A szolgáltatás új tároló fiók beállítása

Ez a egy nem kötelező lépés, amely csak akkor, ha az automatikus létrehozása a tárterület-fiók nem engedélyezte a szolgáltatással végrehajtásához. A Microsoft Azure tároló fiók StorSimple mennyiségi tároló létrehozásához szükséges.

Ha szeretne egy másik régióbeli Azure tároló fiókot létrehozni, olvassa el a [Azure tároló fiókokat kapcsolatban](../storage/storage-create-storage-account.md) részletes útmutatást.

Hajtsa végre az alábbi lépéseket a kormányzati portálon **StorSimple kezelő szolgáltatás** lapon.

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>GITT használata a soros eszköz-konzolon

Kapcsolódás a Windows PowerShell-StorSimple, kell például gitt terminálemuláció szoftver használatához. Az eszköz közvetlenül a soros konzol, vagy úgy, hogy megnyitja a telnet munkamenet távoli számítógépről történő elérésekor gitt használhatja.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Keressen és -frissítések telepítése

Az eszköz frissítése több órát is igénybe vehet. Hajtsa végre az alábbi lépéseket a frissítések keresése és az eszközön.

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Az eszköz frissítése

1.  Eszköz **Első lépések** lapon kattintson az **eszközök**. Válassza ki a fizikai eszközt, kattintson a **Karbantartás** elemre, és kattintson a **Beolvasás frissítések**.  
2.  Az elérhető frissítések keresése a feladat jön létre. Ha frissítések érhetők el, a **Frissítések beolvasása a** **Frissítések telepítése**változik. Kattintson a **frissítések telepítése**gombra.
3.  Egy frissítési feladat jön létre. A frissítés állapotának figyelése navigálással **feladatokat**.

    > [AZURE.NOTE] A frissítés feladat indításakor azonnal megjeleníti az állapot 50 százalékos értékként. A 100 %-os csak a frissítés feladat befejeződése után állapota megváltozik. A frissítési folyamat nem valós idejű állapota nem.

4.  Miután sikeresen frissül az eszközt, adatok 2 és 3 adatok hálózati kapcsolatok engedélyezése, ha ezek le voltak tiltva.

## <a name="get-the-iqn-of-a-windows-server-host"></a>A Windows Server állomás IQN beszerzése

A következő lépésekkel veheti a iSCSI minősített a nevét (IQN) a Windows Server® 2012-kiszolgáló állomás Windows.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Hozzon létre egy kézi biztonsági mentése

Hajtsa végre az alábbi lépéseket a kormányzati portál létrehozása az igény szerinti kézi biztonsági mentést egyetlen kötet StorSimple eszközén.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup-gov.md)]

## <a name="configure-mpio"></a>MPIO konfigurálása

Többszörös I/O (MPIO) választható funkció, és a Windows Server alapértelmezés szerint nincs telepítve. Szolgáltatásként Kiszolgálókezelő segítségével kell telepíteni. MPIO telepítési utasításokat lépjen a [Konfigurálása MPIO StorSimple mobileszközére](storsimple-configure-mpio-windows-server.md).

MPIO telepítési útmutatója Linux állomáshoz való csatlakozás StorSimple eszközt lépjen a [MPIO konfigurálása a Linux szolgáltatója](storsimple-configure-mpio-on-linux.md).

> [AZURE.NOTE] MPIO az Azure virtuális StorSimple eszközön nem támogatott.

## <a name="next-steps"></a>Következő lépések

- [Virtuális eszköz](storsimple-virtual-device-u2.md)beállítása.

- A [StorSimple kezelő szolgáltatás](https://msdn.microsoft.com/library/azure/dn772396.aspx) használatával StorSimple eszköze kezelését.
