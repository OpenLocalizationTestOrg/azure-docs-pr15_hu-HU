<properties 
    pageTitle="Virtuális hálózat támogatása prémium Azure vgx.dll gyorsítótár beállítása |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hozhat létre és kezelhet a prémium réteg Azure vgx.dll gyorsítótár-példányok virtuális hálózat támogatása" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Prémium Azure vgx.dll gyorsítótár virtuális hálózati támogatásának konfigurálása
Azure vgx.dll gyorsítótár tartalmaz a gyorsítótár méretét és funkciói, köztük az új prémium réteg választható rugalmasságot biztosít, amelyek különböző gyorsítótár szeretne rendelni.

Az Azure vgx.dll gyorsítótár prémium réteg szolgáltatásai fürtözés adatmegőrzési és támogatási virtuális hálózati (VNet). Egy VNet magánhálózat a felhőben. Azure vgx.dll gyorsítótár-példány egy VNet van beállítva, amikor nem nyilvánosan címmel rendelkező, és csak a virtuális gépeken futó és a VNet belül alkalmazások érhetők el. Ez a cikk ismerteti, hogyan virtuális hálózati támogatás az Azure vgx.dll gyorsítótár prémium példány beállításához.

>[AZURE.NOTE] Azure vgx.dll gyorsítótár támogatja mindkét klasszikus és ARM VNets.

További információkért más prémium gyorsítótár című témakörben [az Azure-gyorsítótár prémium vgx.dll réteg](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Miért VNet?
[Azure virtuális hálózati (VNet)](https://azure.microsoft.com/services/virtual-network/) telepítési fokozott biztonság és az Azure vgx.dll gyorsítótár, valamint alhálózat, hozzáférési szabályok és más funkciók további elérésének korlátozására Azure vgx.dll gyorsítótár elkülönítési biztosít.

## <a name="virtual-network-support"></a>Virtuális hálózat támogatása
Virtuális hálózati (VNet) támogatás gyorsítótár létrehozása során az **Új vgx.dll gyorsítótárat** a lap van beállítva. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Ha be van jelölve az egy réteg árak prémium, beállíthatja a Azure-gyorsítótár VNet vgx.dll integrációs jelöljön ki egy előfizetést és a helyen, ahol a gyorsítótárban lévő VNet. Egy új VNet használatához előbb létrehozása [az Azure portálon virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) vagy [egy virtuális hálózati (klasszikus) létrehozása az Azure portál használatával](../virtual-network/virtual-networks-create-vnet-classic-portal.md) című témakör lépéseit követve, és térjen vissza az **Vgx.dll gyorsítótár új** lap létrehozása és konfigurálása a prémium gyorsítótár.

Az új gyorsítótár a VNet konfigurálásához **Virtuális hálózati** kattintson az **Új vgx.dll gyorsítótárat** a lap, és válassza ki a kívánt VNet a legördülő listából.

![Virtuális hálózati][redis-cache-vnet]

Jelölje ki a kívánt alhálózat a **alhálózat** legördülő listában, és adja meg a kívánt **statikus IP-cím**. Ha a klasszikus VNet használja a **statikus IP-cím** mező kitöltése nem kötelező, és ha nincs megadva, akkor egy közül a kijelölt alhálózat kiválasztani.

>[AZURE.IMPORTANT] Az Azure vgx.dll gyorsítótár-ARM VNet való telepítésekor, amely nem tartalmaz más erőforrásokat kivételével Azure vgx.dll gyorsítótár-példányok dedikált alhálózat a gyorsítótár kell lennie. Ha a kísérlet az Azure vgx.dll gyorsítótár-ARM VNet egy alhálózathoz telepítéshez használni, amely, tartalmazza, más erőforrások: a telepítés nem sikerült.

![Virtuális hálózati][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] Az első négy címek alhálózat fenntartva, és nem használhatók. További tudnivalókért lásd: [van valamilyen korlátozás használatáról a következő alhálózat belüli IP-címek?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

A gyorsítótár létrehozását követően megtekintheti az adatokat a VNet **Virtuális hálózati** kattintson a **Beállítások** lap a.

![Virtuális hálózati][redis-cache-vnet-info]


Szeretne csatlakozni az Azure vgx.dll gyorsítótár-példányt, egy VNet használatakor, adja meg a gyorsítótár állomásneve a kapcsolati karakterláncban, az alábbi példában látható módon.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure vgx.dll gyorsítótár VNet – gyakori kérdések

Az alábbi lista tartalmazza az Azure vgx.dll gyorsítótár méretezés kapcsolatos gyakori kérdésekre adott válaszok.

-   [Mik azok a gyakori helytelen konfigurálása problémákat Azure vgx.dll gyorsítótár és VNets?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Használhatom-e VNets egy szabványos vagy egyszerű gyorsítótár?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Miért nem vgx.dll-gyorsítótár létrehozása sikertelen az egyes alhálózat, de nem az összes?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Amikor egy VNET a gyorsítótárban szolgáltatója működnek összes gyorsítótár-szolgáltatás?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Mik azok a gyakori helytelen konfigurálása problémákat Azure vgx.dll gyorsítótár és VNets?

Azure vgx.dll gyorsítótár fájlkiszolgálón található egy VNet, amikor a rendszer az alábbi táblázat a portokat használja. Ha ezeket a portokat blokkolt, előfordulhat, hogy a gyorsítótár nem működik megfelelően. Egy vagy több letiltott portokhoz, akkor a leggyakoribb helytelen konfigurálása probléma vgx.dll gyorsítótár Azure egy VNet használata esetén.

| Port     | Szövegirány        | Protokoll | Cél                                                                           | Távoli IP                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443-as     | Kimenő         | TCP                | Vgx.dll függőségek a Azure tároló/nyilvános kulcsú infrastruktúra (Internet)                                | *                                   |
| 53          | Kimenő         | TCP/UDP            | Függőségek (internetes/VNet) DNS vgx.dll                                         | *                                   |
| 6379, 6380  | Bejövő          | TCP                | Ügyfél közötti kommunikáció vgx.dll Azure terheléselosztás                               | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 8443        | Bejövő/kimenő | TCP                | A részletek vgx.dll végrehajtása                                                   | VIRTUAL_NETWORK                     |
| 8500        | Bejövő          | TCP/UDP            | Azure terheléselosztási                                                              | AZURE_LOADBALANCER                  |
| 10221-10231 | Bejövő/kimenő | TCP                | A részletek vgx.dll (korlátozhatja távoli végpont VIRTUAL_NETWORK) végrehajtása | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 13000-13999 | Bejövő          | TCP                | Ügyfél közötti kommunikáció vgx.dll fürt Azure terheléselosztás                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 15000-15999 | Bejövő          | TCP                | Ügyfél közötti kommunikáció vgx.dll fürt Azure terheléselosztás                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 16001       | Bejövő          | TCP/UDP            | Azure terheléselosztási                                                              | AZURE_LOADBALANCER                  |
| 20226       | Bejövő + kimenő | TCP                | A részletek végrehajtása vgx.dll fürt                                          | VIRTUAL_NETWORK                     |


Előfeltételei hálózati kapcsolat Azure vgx.dll gyorsítótár, előfordulhat, hogy nem kell először éri el a virtuális hálózatban. Azure vgx.dll gyorsítótára a következő elemek mindegyikének annak érdekében, hogy megfelelően működnek, ha a virtuális hálózaton belül.

-  Azure tároló végpontok világszerte kimenő hálózati kapcsolat. Ide tartoznak a végpontok azonos régió az Azure vgx.dll gyorsítótár-példányt, valamint a **más** található tároló végpontok Azure régiók található. Azure tároló végpontok megoldásához a következő DNS-tartományok: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*és *file.core.windows.net*. 
-  Kimenő a hálózati kapcsolat *ocsp.msocsp.com*, *mscrl.microsoft.com* és *crl.microsoft.com*. Ez a kapcsolat támogatja az SSL-funkciókat van szükség.
-  A virtuális hálózati DNS-konfigurációjának képes feloldása az összes végpontok és a korábbi pontok említett tartományok kell lennie. Is érvényes DNS-infrastruktúrát van beállítva, és a virtuális hálózat fenntartani biztosításával éri el a DNS feltétel.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Használhatom-e VNets egy szabványos vagy egyszerű gyorsítótár?

VNets csak akkor használható, a prémium gyorsítótárát.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Miért nem vgx.dll-gyorsítótár létrehozása sikertelen az egyes alhálózat, de nem az összes?

Ha egy Azure vgx.dll gyorsítótár-ARM VNet telepít, a gyorsítótár más erőforrástípus tartalmazó dedikált alhálózat kell lennie. Ha a kísérlet az Azure vgx.dll gyorsítótár-ARM VNet alhálózat telepítéshez használni, amely, tartalmazza, más erőforrások: a telepítés nem sikerült. A meglévő erőforrások belül az alhálózathoz törölnie kell, mielőtt új vgx.dll gyorsítótár hozható létre.

A klasszikus VNet több típusú erőforrás telepítheti, feltéve, hogy van elegendő elérhető IP-címek.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Amikor egy VNET a gyorsítótárban szolgáltatója működnek összes gyorsítótár-szolgáltatás?

A gyorsítótárat a VNET része, ha csak a VNET-ügyfélprogramok hozzáférhetnek a gyorsítótár. Az alábbi gyorsítótár-felügyeleti funkciókat eredményeként egyelőre nem működnek.

-   Konzol vgx.dll – vgx.dll konzol a vgx.dll cli.exe ügyfél, amelyek nem szerepelnek a VNET VMs is használ, mert nem tud kapcsolódni a gyorsítótár.


## <a name="use-expressroute-with-azure-redis-cache"></a>Azure vgx.dll gyorsítótár készült ExpressRoute használata

Ügyfelek csatlakozhat az [Azure készült ExpressRoute](https://azure.microsoft.com/services/expressroute/) áramkör a virtuális hálózati infrastruktúrát, így Azure kiterjesztése a helyszíni hálózaton. 

Alapértelmezés szerint egy újonnan létrehozott készült ExpressRoute áramkör meghirdetése alapértelmezett útvonal, amely lehetővé teszi a kimenő internetkapcsolat. Ebben a konfigurációban ügyfélalkalmazásokban csatlakozhat más Azure végpontok, többek között az Azure vgx.dll gyorsítótár állnak.

Azonban a közös ügyfél konfiguráció, hogy a saját alapértelmezett útvonal (0.0.0.0/0), amely kezd kimenő internetes forgalmat inkább a helyszíni megadása. A forgalom folyamat eredményeként töréspontok Azure vgx.dll gyorsítótár kapcsolatokkal, mert a kimenő forgalmának vagy letiltott a helyszíni, vagy hálózati Címfordítást szeretne egy címet, amely nem működnek a különböző Azure végpontokhoz nem felismerhető halmazára.

A megoldás, ha egy (vagy több) felhasználói útvonalak (UDRs) megadása az alhálózat, amely tartalmazza az Azure vgx.dll gyorsítótárból. Egy UDR lesz az alapértelmezett útvonal helyett kell fogadja alhálózat-specifikus útvonalak határozza meg.

Ha lehetséges javasolt a következő beállításokat használja:

- A készült ExpressRoute konfiguráció meghirdetése 0.0.0.0/0 és alapértelmezés szerint a kötelező az összes kimenő forgalmat a helyszíni bújtatja.
- Az alhálózathoz az Azure vgx.dll gyorsítótár tartalmazó alkalmazza a UDR (ilyen például a is megtalálhatók lefelé, a jelen cikkben) internetes következő azonosítható típusú 0.0.0.0/0 határozza meg.

Ezeket a lépéseket a kombinált hatása, hogy UDR alhálózat szintjét elsőbbséget tunneling, kényszerített készült ExpressRoute fölé ezáltal biztosítva az Azure vgx.dll gyorsítótárból kimenő Internet-hozzáférés.

Bár csatlakoztatása az Azure vgx.dll gyorsítótár-példány a egy helyszíni készült ExpressRoute használó nem egy tipikus használatát forgatókönyv miatt teljesítmény érvek (a legjobb teljesítmény elérése érdekében Azure vgx.dll gyorsítótár ügyfelek kell lennie az Azure vgx.dll gyorsítótár azonos régió) ebben az esetben a kimenő hálózati elérési út nem utazási belső vállalati proxyk keresztül, és nem is kell a helyszíni bújtatott hatályba. Ha így módosítja a kimenő hálózati forgalmának engedélyezésére hatékony hálózati Címfordítást címét az Azure vgx.dll gyorsítótárból. Az Azure vgx.dll gyorsítótár hálózati Címfordítást címének megváltoztatása példány a kimenő hálózati forgalmának engedélyezésére hatására a fent felsorolt végpontok számos kapcsolódási hibák. Ez a sikertelen Azure vgx.dll gyorsítótár létrehozási kísérletek eredményez.

**Fontos:**  A útvonalak definiált UDR **kell** , hogy jellemző elsőbbséget élveznek bármely a készült ExpressRoute konfiguráció közzététel útvonalak. Az alábbi példa a széles 0.0.0.0/0 címtartományokat használja, és olyan esetleg véletlenül felülbírálhatja útvonal hirdetések szűkebb címtartományokat használja.

**Nagyon fontos:**  Azure vgx.dll gyorsítótár azonban nem támogatott, hogy **nem megfelelően nyilvános peering elérési útját a magánjellegű peering elérési útvonalak határokon meghirdetése**készült ExpressRoute konfigurációk. Van beállítva, nyilvános peering készült ExpressRoute konfigurációk kap útvonal hirdetések a Microsoft a Microsoft Azure IP-címtartományok nagy számú. Ha ezek címtartományok helytelenül határokon-közzététel a magánjellegű peering úthoz tartozó, a végeredmény található, hogy az összes kimenő hálózati csomagok az Azure vgx.dll gyorsítótár-példányt alhálózat helytelenül hatályba-bújtatott ügyfél helyszíni hálózati infrastruktúrájába. A hálózati folyamat töréspontok Azure vgx.dll gyorsítótár. Ez a probléma megoldása le szeretné állítani a nyilvános peering elérési útvonalak vektoriális-hirdetési magánjellegű peering elérési.

A felhasználó által definiált útvonalak háttér-információkat a [áttekintése](../virtual-network/virtual-networks-udr-overview.md)érhető el. 

Készült ExpressRoute kapcsolatos további tudnivalókért olvassa el a [készült ExpressRoute technikai áttekintés](../expressroute/expressroute-introduction.md) című témakört.

## <a name="next-steps"></a>Következő lépések
Megtudhatja, hogy miként prémium-gyorsítótár további funkcióinak használata.

-   [Az Azure-gyorsítótár prémium vgx.dll réteg – bevezetés](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

