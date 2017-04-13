<properties
    pageTitle="Microsoft Azure-előfizetés és szolgáltatás korlátozások, kvóták és korlátozások"
    description="Itt közös Azure előfizetés és szolgáltatás korlátozások, kvóták és kényszerek listáját. Ide tartoznak a növelése és maximális értékek korlátai olvashat."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure előfizetés és szolgáltatás korlátozások, kvóták és korlátozások

## <a name="overview"></a>– Áttekintés

A dokumentum adja meg a leggyakrabban használt Microsoft Azure korlátozások egy része. Ez jelenleg nem tárgyalja Azure szolgáltatások. Az idő múlásával ezek a korlátok fog kibontható vagy terjed ki annak a platform a frissített.

Kérjük, keresse fel az [Azure árak áttekintése](https://azure.microsoft.com/pricing/) , ha többet szeretne tudni az Azure árak. Itt is költségbecslés a használatával a [Árak Számológép](https://azure.microsoft.com/pricing/calculator/) vagy a szolgáltatás (például a [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)) árak Részletek lap a megtalálhatók.

> [AZURE.NOTE] Ha szeretne emelje feletti **Alapértelmezett korlát**a korlátot, [Nyissa meg az online felhasználói támogatáskérés költség nélkül](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)is. A korlátok nem léptethető alábbi táblázatoknak **Maximális** érték fölé. Ha nincs **Számára vonatkozó maximális érték** oszlop, az adott erőforrás nincsenek korlátozások állítható.

## <a name="limits-and-the-azure-resource-manager"></a>Korlátozások és az Azure erőforrás-kezelő

Mostantól egyetlen Azure erőforrás csoport a több Azure erőforrások egyesítéséhez lehetséges. Az erőforrás csoportok használatakor, korlátozások, amelyekre egyszer globális regionális szinten a az Azure-kezelő eszközzel válnak kezeli. További információt a Azure erőforráscsoport [Azure erőforrás-kezelő áttekintése](azure-resource-manager/resource-group-overview.md)című témakörben találhat.

Az alábbi korlátozások, az új tábla lett hozzáadva bármely korlátok eltérései tükrözi, az Azure erőforrás-kezelő használata esetén. Ha például van egy **Előfizetési korlátok** és egy **Előfizetési korlátok - Azure erőforrás-kezelő** tábla. Korlátozás vonatkozik mindkét esetek, amikor csak az első tábla látható. Hiányában korlátai mindenütt globális minden területen.

> [AZURE.NOTE] Fontos, hogy ki szeretné emelni, hogy erőforrásában található erőforrás-csoportok Azure kvóták per-régióban elérhető az előfizetés, és nem egy előfizetés, a szolgáltatás felügyeleti kvóták vannak. Példaként core kvóták használata. Kell kérése egy kvóta növelése magmintákat támogatása, ha azt szeretné, hogy melyik régióban, és végezze el az összegek és régiók, amelyet egy adott kérelemre Azure erőforráscsoport core kvóták hány magmintákat döntse el szeretne. Ezért, ha szüksége van; az alkalmazás futtatásához használja a 30 magmintákat nyugati Európában 30 magmintákat nyugati Európában kifejezetten kérjék. De más régióban növelése core kvóta nem lesz – a csak a nyugati Europe 30-core kvóta lesz.
<!-- -->
Eredményt adja akkor előfordulhat, hogy fontolja meg a Mi az Azure erőforráscsoport kvóták kell lennie a terhelés bármely egy régióban kiválasztásához hasznos, és kérni, hogy az egyes régiókra, amelybe tervezi telepítési összeg. Lásd:, a [telepítési problémák elhárítása](./resource-manager-common-deployment-errors.md) az adott régióban aktuális kvóták felfedezése további segítséget.

## <a name="service-specific-limits"></a>Szolgáltatás-specifikus korlátai

- [Az Active Directory](#active-directory-limits)
- [API-kezelés](#api-management-limits)
- [Alkalmazás szolgáltatás](#app-service-limits)
- [Alkalmazás átjáró](#application-gateway-limits)
- [Alkalmazás Hírcsatornájában](#application-insights-limits)
- [Automatizálási](#automation-limits)
- [Azure vgx.dll gyorsítótár](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Biztonsági másolat](#backup-limits)
- [Köteg](#batch-limits)
- [BizTalk szolgáltatások](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Cloud Services](#cloud-services-limits)
- [Adatok gyári](#data-factory-limits)
- [Adatok tó Analytics](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Esemény hubok](#event-hubs-limits)
- [IoT központi](#iot-hub-limits)
- [Fő tárolóból elemre](#key-vault-limits)
- [Media Services](#media-services-limits)
- [Mobil tetszés szerint elmélyedhet](#mobile-engagement-limits)
- [Mobil szolgáltatások](#mobile-services-limits)
- [Figyelése](#monitoring-limits)
- [Többtényezős hitelesítés](#multi-factor-authentication)
- [Hálózati](#networking-limits)
- [Értesítési központi szolgáltatás](#notification-hub-service-limits)
- [Műveleti Hírcsatornájában](#operational-insights-limits)
- [Erőforráscsoport](#resource-group-limits)
- [A Feladatütemező](#scheduler-limits)
- [Keresés](#search-limits)
- [Szolgáltatás Bus](#service-bus-limits)
- [Webhely-helyreállítás](#site-recovery-limits)
- [SQL-adatbázis](#sql-database-limits)
- [Tárhely](#storage-limits)
- [StorSimple rendszer](#storsimple-system-limits)
- [Értékáram-elemzés](#stream-analytics-limits)
- [Előfizetés](#subscription-limits)
- [Adatforgalom Manager](#traffic-manager-limits)
- [Virtuális gépeken futó](#virtual-machines-limits)
- [Virtuális gép skála beállítása](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Előfizetés korlátai
#### <a name="subscription-limits"></a>Előfizetés korlátai
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Előfizetési korlátok - Azure erőforrás-kezelő

Az alábbi korlátozások vonatkoznak az Azure erőforrás-kezelő és Azure erőforráscsoport használata esetén. Korlátozások, amelyek nem megváltozott a az Azure-kezelő eszközzel nem felsorolása olvasható. Nézze meg ezeket a korlátokat az előző táblázatot.

Kezelésére vonatkozó korlátok erőforrás-kezelő kérések tudni lásd: az [erőforrás-kezelő szabályozásának kérések](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Erőforráscsoport korlátai

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Virtuális gépeken futó korlátai
#### <a name="virtual-machine-limits"></a>Virtuális gép korlátai
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Virtuális gépeken futó korlátai - Azure erőforrás-kezelő

Az alábbi korlátozások vonatkoznak az Azure erőforrás-kezelő és Azure erőforráscsoport használata esetén. Korlátozások újdonsággal nem az az Azure erőforrás-kezelő nem felsorolása olvasható. Nézze meg ezeket a korlátokat az előző táblázatot.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Virtuális gép skála készletek korlátai

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Hálózati korlátozások

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Hálózati korlátozások
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Alkalmazás átjáró korlátozza

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Adatforgalom Manager korlátai

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-korlátai

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Tárterületkorlátok

A fiók tárterületkorlátok további részletekért olvassa el [Azure tároló méretezhetőség és a teljesítmény célokat](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Tárterületkorlátok szolgáltatás

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Virtuális gép lemez korlátai

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

További részleteket lásd: a [virtuális gép méretét](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Szabványos tárterület-fiókok**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Prémium verzió tároló használata**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Tárterületkorlátok erőforrás-szolgáltató

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Cloud Services korlátai

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Alkalmazás szolgáltatás korlátai
Az alábbi alkalmazás szolgáltatás határértékeket korlátai tartalmazzák a Web Apps alkalmazások, a Mobile-alkalmazások, a API-alkalmazások és az alkalmazások összefüggés.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>A Feladatütemező korlátai

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Köteg korlátai

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>BizTalk szolgáltatások korlátai
A következő táblázat mutatja a korlátok, az Azure Biztalk szolgáltatások.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>DocumentDB korlátai

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Megjelenik egy csillag (*) [Azure ügyfélszolgálat megkeresésével módosítható](./documentdb/documentdb-increase-limits.md)és kvóták.

### <a name="mobile-engagement-limits"></a>Mobil tetszés szerint elmélyedhet korlátai

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Keresési korlátok

Árak rétegek határozza meg a beosztását, és a keresési szolgáltatás korlátai. Réteg a következők:

- *Ingyenes* több bérlői szolgáltatás, más Azure előfizetők megosztott szánt értékelése és a kis fejlesztési projektek.
- *Egyszerű* nyújt a dedikált számítógépes erőforrások gyártási munkaterhelésekből kisebb méretarány, a könnyen hozzáférhető lekérdezés munkaterhelésekből legfeljebb három kópiákkal.
- *A standard (S1, S2, S3, S3 magas sűrűség)* nagyobb gyártási munkaterhelésekből szolgál. Többszintű, hogy egy erőforrás-különböző forgatókönyvekben beállításait választhatja a szokásos réteg tartalmaz.

**Előfizetésenként korlátai**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Keresési szolgáltatás használati korlátai**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

További korlátozások, beleértve a dokumentum mérete lekérdezések száma a második, billentyűk, kérések és válaszok, részletesebb információt című cikkben találhat [szolgáltatás Azure keresés](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services korlátai

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN-korlátai

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Mobil szolgáltatások korlátai

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Figyelés korlátai

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Értesítés központi szolgáltatás korlátai

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Esemény hubok korlátai

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Szolgáltatás Bus korlátai

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT központi korlátai

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Adatok gyári korlátozása

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Adatok tó Analytics korlátozása
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Értékáram-elemzés korlátozza
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Az Active Directory korlátai

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure RemoteApp korlátai

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple rendszer korlátai

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Műveleti háttérismeretek korlátai

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Biztonsági korlátozások

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Webhely helyreállítási korlátai

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Alkalmazás háttérismeretek korlátai

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API-kezelés korlátai

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure vgx.dll gyorsítótárból korlátai

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Kulcs tárolóra korlátai

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Többtényezős hitelesítés
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automatizálási korlátai
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>SQL-adatbázis korlátai

SQL-adatbázis korlátozások című cikkben találhat [SQL adatbázis erőforrás](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Lásd még:

[Azure korlátai és nő ismertetése](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuális gép és felhőalapú szolgáltatást méretű az Azure](virtual-machines/virtual-machines-linux-sizes.md)

[A Cloud Services méretek](cloud-services/cloud-services-sizes-specs.md)
