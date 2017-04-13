<properties
    pageTitle="Azure CDN-áttekintés |} Microsoft Azure"
    description="Megtudhatja, mi az Azure tartalom kézbesítési hálózati (CDN), és használhat az előadáshoz nagy sávszélességű tartalom szerint gyorsítótárazás használatához BLOB és statikus tartalommá."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Az Azure Tartalomkézbesítési hálózatai (CDN) – áttekintés

> [AZURE.NOTE] Ez a dokumentum ismerteti, hogy mi az Azure tartalom kézbesítési hálózati (CDN) van, működéséről és Azure CDN termékenként funkcióját.  Ha azt szeretné, ez az információ kihagyásával közvetlenül az oktatóanyagot szeretne, hogy miként hozhat létre egy CDN-végpontot, lásd: [Azure CDN használatával](cdn-create-new-endpoint.md).  Az aktuális CDN-csomópont helyek listájának megtekintéséhez, című témakörben olvashat [Azure CDN POP helyek](cdn-pop-locations.md).

Azure tartalom kézbesítési hálózati (CDN) gyorsítótárát statikus webes tartalmak stratégiai elhelyezett helyeken nyújt a maximális sebesség tartalom továbbítása a felhasználók számára.  A CDN felajánlja a fejlesztők kipróbálására nagy sávszélességű tartalmat úgy, hogy a tartalom a fizikai csomópontok gyorsítótárazás világszerte globális megoldást. 

A gyorsítótár webhely eszközök a CDN előnyei többek között:

- Jobb teljesítményt és a felhasználói felület végfelhasználók számára, különösen akkor, amikor az alkalmazások használata, ha több állapotát a körbejárásokhoz szükséges tartalom betöltése.
- Nagy jobban méretezés kezelheti a pillanatnyi nagy terhelés, például egy termék indítási események elején.
- Felhasználói kérések terjesztése és tartalmak az peremhálózat-kiszolgálói szerepkör, kisebb forgalmat a rendszer elküldi az origin.


## <a name="how-it-works"></a>Működése

![CDN – áttekintés](./media/cdn-overview/cdn-overview.png)

1. A felhasználó (Anna) kéréseket (más néven tárgyi eszköz) fájl URL-cím használata speciális tartománynevet, például: `<endpointname>.azureedge.net`.  DNS a legjobb elvégzéséhez pont jelenléti (POP) helyét a kérelem irányítja.  Ez általában a POP, amely a felhasználó földrajzi legközelebb esik.

2. Ha a POP a peremhálózat-kiszolgálói nem rendelkezik a fájlt a gyorsítótárban lévő, a biztonsági kiszolgálójának kér a fájl az origin.  Az origin lehet az Azure-Webappokban, Azure Felhőszolgáltatásában, Azure tárterület-fiók vagy bármely nyilvánosan hozzáférhető webkiszolgálóra.

3. Az origin a fájlt a biztonsági kiszolgálójának, a választható HTTP fejlécekkel azt mutatja be, a fájl Time-to-Live (TTL) adja vissza.

4. A biztonsági kiszolgálójának gyorsítótárát a fájlt, és az eredeti kérési (Anna) és adja meg a fájlt..  A TTL (élettartam) lejár marad a biztonsági kiszolgálón tárolt a fájlt.  Ha az origin nem adja meg a TTL (élettartam), az alapértelmezett TTL (élettartam) hét napig tart.

5. A többi felhasználó kérjen ugyanannak a fájlnak, hogy egy URL-címet használ, és is előfordulhat, hogy ugyanazt a POP a irányítja.

6. Ha a TTL (élettartam), a fájl importálása nem járt le, a biztonsági kiszolgálójának a fájlt a gyorsítótár adja eredményül.  Ez egy gyorsabb, gyorsabb felhasználói felület eredményez.


## <a name="azure-cdn-features"></a>Azure CDN-szolgáltatások

Vannak olyan három Azure CDN-termékek: **Azure CDN szabványos a Akamai**, **Azure CDN szabványos a Verizon**és **Azure CDN Premium a Verizon**.  Az alábbi táblázat az egyes termékek szolgáltatásait.

|       | Szabványos Akamai | Szabványos Verizon | Prémium Verizon |
|-------|-----------------|------------------|-----------------|
| Könnyen funkciók integrálása az Azure szolgáltatások, például a [tárhely](cdn-create-a-storage-account-with-cdn.md), a [Cloud Services](cdn-cloud-service-with-cdn.md), a [Web Apps alkalmazások](../app-service-web/cdn-websites-with-cdn.md)és a [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;**|
| Adatkezelési [REST API-t](https://msdn.microsoft.com/library/mt634456.aspx), a [.NET rendszerhez](./cdn-app-dev-net.md), a [Node.js](./cdn-app-dev-node.md)vagy a [PowerShell](./cdn-manage-powershell.md)használatával. | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| HTTPS-támogatás | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Terheléselosztás | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [DDOS](https://www.us-cert.gov/ncas/tips/ST04-015) védelme | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Kettős-Papírhalom IPv4 és az IPv6 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Egyéni tartomány neve támogatás](cdn-map-content-to-custom-domain.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [A lekérdezés gyorsítótár-karakterlánc](cdn-query-string.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [GEO szűrése](cdn-restrict-access-by-country.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Gyors törlése](cdn-purge-endpoint.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [A digitális eszköz kiválasztása előzetes betöltése](cdn-preload-endpoint.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Alapvető analytics](cdn-analyze-usage-patterns.md) |  | **& #x 2713;** | **& #x 2713;** |
| [HTTP-/ 2 támogatás](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x 2713;**  |  |  |
| [Speciális HTTP-jelentések](cdn-advanced-http-reports.md) | | | **& #x 2713;** |
| [Valós idejű stat](cdn-real-time-stats.md) | | | **& #x 2713;** |
| [Valós idejű értesítések](cdn-real-time-alerts.md) | | | **& #x 2713;** |
| [Testre szabható, szabály alapú tartalomkézbesítési motor](cdn-rules-engine.md) | | | **& #x 2713;** |
| Gyorsítótár/fejléc beállítások (a [szabályok motor](cdn-rules-engine.md)használata)  | | | **& #x 2713;** |
| URL-CÍMÉT az átirányítási/átírása (a [szabályok motor](cdn-rules-engine.md)használata) | | | **& #x 2713;** |
| Mobileszköz szabályok (a [szabályok motor](cdn-rules-engine.md)használata)  | | | **& #x 2713;** |

>[AZURE.TIP] Van-e Azure CDN látni szeretné egy szolgáltatás?  [Küldjön visszajelzést](https://feedback.azure.com/forums/169397-cdn)! 

## <a name="next-steps"></a>Következő lépések

Első lépések a CDN, olvassa el az [Azure CDN használatával](./cdn-create-new-endpoint.md).

Ha egy meglévő CDN-ügyfél, kezelheti az a CDN-végpontok [PowerShell](cdn-manage-powershell.md), vagy a [Microsoft Azure-portálon](https://portal.azure.com) keresztül.

A művelet a CDN megtekintéséhez kivétele a [Videó az összeállítás 2016 munkamenet](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Megtudhatja, hogy miként Azure CDN [.NET](./cdn-app-dev-net.md) vagy [Node.js](./cdn-app-dev-node.md)automatizálása.

Árinformációkat, [CDN árak](https://azure.microsoft.com/pricing/details/cdn/)talál.
