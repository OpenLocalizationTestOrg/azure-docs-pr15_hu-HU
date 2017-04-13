<properties
   pageTitle="Üzembe StorSimple virtuális tömb 1 - portál előkészítése"
   description="Első oktatóprogram StorSimple virtuális tömb üzembe magában foglalja a portálon előkészítése"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/24/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>StorSimple virtuális tömb telepítése – a portálon előkészítése

![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>– Áttekintés

Ez a cikk a Microsoft Azure StorSimple virtuális tömbképletek (más néven StorSimple helyszíni virtuális eszköz vagy StorSimple virtuális eszköz) futó március 2016 általános elérhetőség (kiadás) verziójára vonatkozik. Ez az üzembe helyezési oktatóprogram teljesen telepítéséhez a virtuális tömb fájlkiszolgálóra vagy egy iSCSI kiszolgálón szükséges a sorozat az első cikk. Ez a cikk ismerteti az előkészítés létrehozása és konfigurálása a StorSimple kezelő szolgáltatás kiépítési egy virtuális tömböt előtt szükséges. Ez a cikk is kapcsolódik a telepítési konfigurációs ellenőrzőlista, valamint a konfigurációs előfeltételek.

Rendszergazdai jogosultságokkal a beállítás és konfiguráció befejezéséhez lesz szüksége. Azt javasoljuk, olvassa el a telepítési konfigurációs ellenőrzőlista megkezdése előtt. A portál előkészítése kisebb, mint a 10 percig tart.

Ez a cikk közzétett információkra Azure klasszikus portál, valamint a Microsoft Azure kormányzati Cloud StorSimple virtuális tömbök telepítését vonatkozik.

### <a name="get-started"></a>Első lépések

A telepítési munkafolyamat, ahol a Felkészülés a portálon, egy virtuális tömböt a virtualizált környezetben kiépítési és a telepítést. Első lépések a StorSimple virtuális tömb példányban, mint egy fájlt vagy iSCSI-kiszolgáló, szüksége lesz a következő tabulált erőforrások (cikkek és videók) vonatkoznak.

#### <a name="deployment-articles"></a>Telepítési cikkek

Olvassa el az alábbi cikkekben a StorSimple virtuális tömb üzembe előírt sorrendben.

| **#** | **Ebben a lépésben**                          | **Ez hajt végre.**                                                         | **Használja ezeket a dokumentumokat.**|
|------|-------------------------------------------|--------------------------------------------------------------------------------|------------------------|
|1.   | **Az Azure klasszikus portál beállítása**       | Létrehozása és konfigurálása a StorSimple kezelő szolgáltatás kiépítési StorSimple virtuális eszköz előtt.  |[Felkészülés a portálon](storsimple-ova-deploy1-portal-prep.md)|
|2.   | **A virtuális tömb kiépítése**           | A Hyper-V kiépítése, és csatlakozzon a Hyper-V futó Windows Server 2012 R2, Windows Server 2012-ben vagy Windows Server 2008 R2 rendszeren host StorSimple virtuális eszköz. <br></br> <br></br> VMware kiépítése és StorSimple helyszíni virtuális eszköz host operációs rendszert futtató VMware ESXi 5.5 és a fenti csatlakozni.<br></br>| [Egy virtuális tömböt a Hyper-V kiépítése](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [A virtuális tömbben VMware kiépítése](storsimple-ova-deploy2-provision-vmware.md)|
|3.    | **A virtuális tömb beállítása**              | A fájl kiszolgálói kezdeti telepítés végrehajtásához, regisztrálhatja a StorSimple fájlkiszolgálóra és az eszköz telepítéséhez. Kis-és Középvállalatok megosztások kiépítése majd akkor is. <br></br> <br></br> A iSCSI Server végezze el a kezdeti beállítás regisztrálni a StorSimple iSCSI kiszolgálót és az eszköz telepítéséhez. Akkor is kiépítése iSCSI kötet majd.| [Fájl kiszolgálójával virtuális tömb beállítása](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[ISCSI kiszolgálójával virtuális tömb beállítása](storsimple-ova-deploy3-iscsi-setup.md)|

#### <a name="deployment-videos"></a>Telepítési videók

| **Ez a lépés...** |  **Ez a videó.**|
|----------------|-------------|
| Első lépések a StorSimple virtuális tömb lépésenkénti utasításokat. | [Első lépések a StorSimple virtuális tömb](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/)|
| Lépésenkénti utasítások egy StorSimple virtuális tömböt a Hyper-V hozhatók létre.|[Hozzon létre egy StorSimple virtuális tömb](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
|Lépésenkénti útmutatást és regisztrálhatja a StorSimple virtuális tömb|[Egy StorSimple virtuális tömböt konfigurálása](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/)|
|Lépésenként oszthat, készítsen biztonsági másolatot a megosztás, és egy fájl konfigurálva StorSimple virtuális tömböt adatairól|[A StorSimple virtuális tömb használata](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/)|
|Lépésenkénti útmutatást adunk a StorSimple virtuális tömb feladatátvétel és katasztrófa helyreállítás|[StorSimple virtuális tömb vészhelyreállítás](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/)

Most már megkezdheti az Azure klasszikus portál beállítása.

## <a name="configuration-checklist"></a>Konfigurációs ellenőrzőlista

A konfigurációs ellenőrzőlista a szükséges információkat, mielőtt beállítaná a szoftver StorSimple eszközén összegyűjtéséhez ismerteti. Ez az információ időszakokra előkészítése súgó korszerűsíthessék az üzembe helyezése a StorSimple eszköz a környezetben. Attól függően, hogy StorSimple virtuális eszköze telepítik fájlkiszolgálóra vagy egy iSCSI kiszolgáló, szüksége lesz az alábbi feladatlisták közül.

-   Töltse le a [StorSimple virtuális tömb fájl kiszolgálói konfigurációs ellenőrzőlista](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).

-   Töltse le a [kiszolgáló konfigurációs ellenőrzőlista StorSimple virtuális tömb iSCSI](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Előfeltételek

Itt találja a StorSimple kezelő szolgáltatás, a StorSimple virtuális eszköz és az Adatközpont hálózati konfigurációja előfeltételei.

### <a name="for-the-storsimple-manager-service"></a>A StorSimple kezelő szolgáltatás

Mielőtt elkezdené, győződjön meg arról, hogy:

-   Ha a Microsoft-fiók hozzáférési hitelesítő adatokkal.

-   Van Microsoft Azure tároló fiókja, az access hitelesítő adatokkal.

-   Microsoft Azure-előfizetése StorSimple-kezelő szolgáltatást engedélyezni kell.

### <a name="for-the-storsimple-virtual-device"></a>A StorSimple virtuális eszköz

Mielőtt beállítaná a virtuális eszköz, győződjön meg arról, hogy:

-   Hozzáférhet a Windows Server 2008 R2 vagy újabb Hyper-V szolgáltatást futtató host rendszer vagy VMware (ESXi 5,5 vagy újabb), amely egy kiépítése használt eszköz.

-   A host rendszer nem tudja az alábbi forrásokban hozhatók létre virtuális eszköze jelöl ki:

    -   Legalább 4 magmintákat.

    -   Legalább 8 GB RAM.

    -   Egy hálózati kapcsolat.

    -   500 GB virtuális lemez rendszeradatokat.

### <a name="for-the-datacenter-network"></a>Az Adatközpont hálózathoz

Mielőtt elkezdené, győződjön meg arról, hogy:

-   A hálózat a adatközpontban StorSimple eszköze hálózati követelményei szerint úgy van beállítva. További tudnivalókért olvassa el a [StorSimple virtuális tömb rendszerkövetelményei](storsimple-ova-system-requirements.md)című témakört.

-   Virtuális StorSimple eszköze egy dedikált 5 MB Internet-sávszélesség (vagy több) van rendelkezésre álló állandóan látszódjon. A sávszélesség más alkalmazásokkal nem lehet megosztani.

## <a name="step-by-step-preparation"></a>Részletes előkészítése

A portálon Felkészülés a StorSimple kezelő szolgáltatás használja a következő lépésenkénti útmutatót.

## <a name="step-1-create-a-new-service"></a>Lépés: 1: Hozzon létre egy új szolgáltatás

Egy példányát a StorSimple kezelő szolgáltatás és több StorSimple 1200 is kezelheti. A következő lépésekkel hozzon létre egy új példányát az StorSimple kezelő szolgáltatás. Ha egy meglévő StorSimple-kezelő szolgáltatást a 1200 eszközök kezeléséhez, a lépés kihagyását, és lépjen [2.lépésben: a szolgáltatás regisztrációs kulcs első](#step-2-get-the-service-registration-key).

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [AZURE.IMPORTANT]
>
> Ha az automatikus létrehozása a tárterület-fiók nem engedélyezte a szolgáltatással, szüksége lesz legalább egy tárterület-fiók létrehozása, miután létrehozott egy szolgáltatás sikeresen.
>

> - Ha nem Ön hozta létre a tárterület-fiók automatikusan, nyissa meg [a szolgáltatás új tároló fiók konfigurálása](#optional-step-configure-a-new-storage-account-for-the-service) a részletes útmutatást.
>

> - Ha engedélyezte a tárterület-fiók automatikus létrehozása, nyissa meg [2 lépés: a szolgáltatás regisztrációs kulcs első](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Lépés: 2: A szolgáltatás regisztrációs kulcs beszerzése


Miután a StorSimple kezelő szolgáltatás lépéseket, szüksége lesz a szolgáltatás regisztrációs kulcs első. A kulcs regisztrálhat, és az StorSimple eszköz kapcsolatba lépni a szolgáltatást használják.

Hajtsa végre az alábbi lépéseket az [Azure klasszikus portálon](https://manage.windowsazure.com/).


[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [AZURE.NOTE]
>
> A szolgáltatás regisztrációs kulcs regisztrálhatja a StorSimple kezelő szolgáltatás regisztrálnia kell StorSimple-kezelő eszközök használják.

## <a name="step-3-download-the-virtual-device-image"></a>Lépés 3: Töltse le a virtuális eszköz képe

Után a szolgáltatás regisztrációs kulcs, szüksége lesz töltse le a megfelelő virtuális eszköz képet a host rendszeren virtuális eszköz hozhatók létre. A virtuális eszköz képek operációs rendszer adott és letölthető az Azure klasszikus portál az első lépések lapján.

> [AZURE.IMPORTANT] A szoftver fut a StorSimple virtuális tömb csak használható együtt a Storsimple kezelő szolgáltatás.


Hajtsa végre az alábbi lépéseket az [Azure klasszikus portálon](https://manage.windowsazure.com/).

#### <a name="to-get-the-virtual-device-image"></a>Virtuális eszköz képlista

1.  **StorSimple kezelő szolgáltatás** lapján kattintson az Ön által létrehozott szolgáltatás. Ezzel eljut az **Első lépések** lap. (A rövid útmutató az első ikonra ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) , amelyen az **Első lépések** bármikor.)

1.  Kattintson a képre, amelyet a Microsoft Download Center töltse le a megfelelő hivatkozásra. A kép fájlok körülbelül 4.8 GB.

    -   A Windows Server 2012-ben, és később Hyper-v VHDX

    -   Virtuális Hyper-v a Windows Server 2008 R2 és újabb verziók

    -   VMDK VMWare ESXi 5.5 és újabb verziók

2.  Töltse le és bontsa ki a fájlt egy helyi meghajtóra, és készít egy megjegyzés, ahol a kicsomagolt fájl található.

![videó ikon](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **Videó érhető el**

A videó az első lépések a StorSimple virtuális tömb lépésenkénti útmutatást.

> [AZURE.VIDEO get-started-with-the-storsimple-virtual-array]



## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Tetszőleges lépés: a szolgáltatás új tároló fiók beállítása

Ez a egy kell elvégezni, csak akkor, ha az automatikus létrehozása a tárterület-fiók nem engedélyezte a szolgáltatással, hogy nem kötelező lépés.

Ha egy másik régióbeli Azure tároló fiók létrehozása kell tájékozódhat [tárterület-fiók létrehozása](storage-create-storage-account.md#create-a-storage-account) részletes útmutatást.

Microsoft Azure tároló meglévő fiók felvétele lapon a StorSimple szolgáltatás [Azure klasszikus portál](https://manage.windowsazure.com/) , hajtsa végre az alábbi lépéseket.

#### <a name="to-add-a-storage-account"></a>Tárterület-fiók hozzáadása

1.  A céloldal StorSimple kezelő szolgáltatása jelölje ki azt a szolgáltatást, és kattintson rá duplán. Ezzel eljut az **Első lépések** lap. Jelölje be **a lap** .

2.  Kattintson a **tárterület-fiók hozzáadása/módosítása**gombra. **Tárterület-fiók hozzáadása/szerkesztése** párbeszédpanelen tegye a következőket:

    1.  Kattintson a **Hozzáadás új**.

    1.  Adja meg a tárterület-fiókja nevét.

    1.  Adja meg a Microsoft Azure tároló fiók **Hívóbetű** elsődleges.

    1.  Válassza a **mód engedélyezése SSL** hozhat létre az eszközön, és a felhő között a hálózati kommunikációs biztonságos csatornát. Csak akkor, ha saját felhő belül működik, törölje a jelet az **Enable SSL módban** jelölőnégyzetet.

    1.  Kattintson az ellenőrzés ikon ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). Kap értesítést jelenít meg, a tárhely fiók sikeres létrehozását követően.

        ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)

1.  Az újonnan létrehozott tároló fiók jelennek meg a **tárterület-fiókok** **beállítása** lapon. **Mentse** az újonnan létrehozott tároló fiók mentése gombra. Kattintson az **OK gombra** , amikor a program megerősítést kér.


## <a name="next-step"></a>Következő lépés

A következő lépésként StorSimple virtuális eszköze virtuális gép hozhatók létre. A host az operációs rendszertől függően a részletes útmutatást találhat:

-   [Építse be a Hyper-V egy StorSimple virtuális tömb](storsimple-ova-deploy2-provision-hyperv.md)

-   [Egy StorSimple VMware virtuális tömbben kiépítése](storsimple-ova-deploy2-provision-vmware.md)
