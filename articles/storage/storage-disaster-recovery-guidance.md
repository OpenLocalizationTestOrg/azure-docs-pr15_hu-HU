<properties
    pageTitle="Mi a teendő az Azure tároló üzemszünetek megelőzve |} Microsoft Azure"
    description="Mi a teendő az Azure tároló üzemszünetek megelőzve"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Mi a teendő, ha egy Azure tároló üzemszünetek jelentkezik

A Microsoft azt munkára szigorú ellenőrizze, hogy a szolgáltatások mindig elérhetők. Időnként előfordulhat kezd túl a vezérlő hatása us nem tervezett szolgáltatás kimaradások okozó egy vagy több régióban módon. Segít a ritka előfordulások kezelni, kínálunk, amely az Azure tárterület-szolgáltatások a következő magas szintű útmutatást.

## <a name="how-to-prepare"></a>Hogyan előkészítése 

Rendkívül fontos minden saját Vészhelyreállítási terv elkészítése elkészítéséhez vevő. A szokásos helyreállítása a tárhely üzemszünetek munkamennyiség magában foglalja a műveletek személyzeti, mind az automatikus eljárások annak érdekében, hogy aktiválja újra az alkalmazások működését állapotban. Olvassa el az alábbi létrehozásához a saját Vészhelyreállítási terv elkészítése Azure át:

-   [Vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Azure tűrőképessége technikai útmutató](../resiliency/resiliency-technical-guidance.md)

-   [Azure webhely helyreállítási szolgáltatás](https://azure.microsoft.com/services/site-recovery/)

-   [Azure tároló replikációs](storage-redundancy.md)

-   [Azure biztonságimásolat-szolgáltatás](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Hogyan feltárása 

A javasolt megállapíthatja az Azure szolgáltatás állapota e az [Azure állapotjelző irányítópultja](https://azure.microsoft.com/status/)előfizetéséhez.

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Mi a teendő, ha egy tároló üzemszünetek fordul elő

Ha egy vagy több tárterületet szolgáltatás átmenetileg nem érhető el egy vagy több régiójában, két lehetőség a bemutatókkal. Ha az adatok azonnali hozzáférést kíván, fontolja meg 2 lehetőséget.

### <a name="option-1-wait-for-recovery"></a>1 beállítást: Helyreállítási megvárja, amíg

Ebben az esetben nincs további teendő hajtana szükség. Dolgozunk szorgalmasan az Azure szolgáltatáselérhetőség visszaállításához. A szolgáltatás állapota az [Azure állapotjelző irányítópultja](https://azure.microsoft.com/status/)figyelheti meg.

### <a name="option-2-copy-data-from-secondary"></a>2 a beállítás: Az adatok másolása másodlagos

Ha úgy döntött, hogy a tárterület-fiókok (ajánlott) [olvasásra geo felesleges tároló (TS-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) , a másodlagos régióból akkor olvasási hozzáférést az adatok. Használható eszközök, például [AzCopy](storage-use-azcopy.md) [Azure PowerShell](storage-powershell-guide-full.md)és a [Azure adatok mozgását tár](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) olyan adatokat másol egy másik tároló fiók unimpacted területen a másodlagos területről, és pontra az alkalmazások szeretné a fiókhoz társított tároló mindkét olvasása és írása elérhetősége.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Mire számíthat, ha a tárhely feladatátvétel

Ha úgy döntött, [Geo felesleges tároló (GRS)](storage-redundancy.md#geo-redundant-storage) vagy az [olvasásra geo felesleges tároló (TS-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (ajánlott), Azure tároló fog szeretné tartani az adatokat tartós két régióban (elsődleges és másodlagos). A két régió a Azure tároló folyamatosan az adatok több replikáit tárolja.

Amikor területi katasztrófa hatással van az elsődleges régió, azt először megpróbálja a szolgáltatást az adott régióban visszaállítása. A katasztrófa és annak megközelítés hatásai: az egyes ritka esetekben jellegét függ azt nem lehet az elsődleges régió visszaállításához. Ezen a ponton geo átváltó végez azt. Idegen-régió adatai replikációs, egy aszinkron amely kattintana, szükség lesz, késleltetést, lehetséges, hogy a módosításokat, amelyeket van nem került még bele a másodlagos régió elveszhetnek. A ["utolsó szinkronizálás" a tárhely fiókja](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) a replikáció állapotát a részleteket is lekérdezhetők.

A tároló geo átváltó felület jellemzőit néhány:

-   Tárterület geo átváltó csak indított az Azure tároló csoport – nem felhasználói beavatkozás nélkül.

-   A meglévő tároló szolgáltatás végpontokat BLOB, a táblázatok, a sorok és a fájlok változatlan marad után az erőforrás DLL. a DNS-bejegyzés kell frissíteni kell a másodlagos régió az elsődleges régió váltani.

-   Előtte és a geo feladatátvételkor írási hozzáféréssel a tárterület-fiókjába, milyen következményekkel járnak a katasztrófa miatt nem van, de továbbra is erről a másodlagos a Ha TS-GRS a tárterület-fiók van konfigurálva.

-   Amikor befejeződött a geo átváltó és propagálja a DNS-módosításokat, az olvasási és írási hozzáféréssel a tárterület-fiókjába fog folytatható. ["Utolsó Geo feladatátvételi idő" a tárhely fiók](https://msdn.microsoft.com/library/azure/ee460802.aspx) további részleteket is lekérdezhetők.

-   A feladatátvételt, után a tárterület-fiókja teljesen működik, de "csökkentett" állapotú, ahogy ténylegesen üzemelteti egy különálló régiójában nem geo replikációs lehet. A kockázat csökkentése érdekében, hogy fog visszaállítása az eredeti elsődleges régió, és végezze el a geo visszaállás az eredeti állapotának visszaállításához. Ha az eredeti elsődleges régió helyrehozhatatlan, azt egy másik másodlagos terület osztja fel.
Azure tároló geo replikációs infrastruktúra további részletekért olvassa el a cikk a tárterület-termékcsapat blogjában [redundancia beállítások](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)és TS-GRS.

##<a name="best-practices-for-protecting-your-data"></a>Gyakorlati tanácsok az adatok védelme

Vannak bizonyos adatok biztonsági mentéséhez a tárhely rendszeresen ajánlott eljárások.

-   Virtuális lemez – az [Azure biztonsági másolat szolgáltatás](https://azure.microsoft.com/services/backup/) segítségével biztonsági másolatot az Azure virtuális gépeken futó által használt virtuális lemezt.

-   Továbbfejlesztett fájlblokkolás BLOB – egy [pillanatkép](https://msdn.microsoft.com/library/azure/hh488361.aspx) egyes blokk blob, és a BLOB-tárolóhoz egy másik fiók [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)vagy az [Azure adatok mozgását tár](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)egy másik tartományban lévő példány létrehozása.

-   Táblák – [AzCopy](storage-use-azcopy.md) segítségével a táblázat-adatok exportálása egy másik tartományban lévő másik tárterület-fiókba.

-   Fájlok – [AzCopy](storage-use-azcopy.md) vagy [Azure PowerShell](storage-powershell-guide-full.md) használatával a fájlok másolása egy másik tartományban lévő másik tárterület-fiókjába.
