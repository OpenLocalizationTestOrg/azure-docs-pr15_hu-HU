<properties
   pageTitle="Első lépések az Azure DNS |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre a DNS-zónák Azure DNS-hez. Ez a egy a Step by step az első DNS-zóna elindítani a PowerShell használatá tartománya DNS-annak létrehozott eléréséhez."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-a-dns-zone-using-powershell"></a>Hozzon létre egy DNS-zóna Powershell használatával

> [AZURE.SELECTOR]
- [Azure portál](dns-getstarted-create-dnszone-portal.md)
- [A PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)

Ez a cikk azt ismerteti, hogy a műveletek elvégzéséhez hozhat létre a DNS-zóna PowerShell használatával. A DNS-zóna CLI vagy az Azure portal segítségével is létrehozhat.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Tudnivalók a Etags és címkék

### <a name="etags"></a>Etags

Tegyük fel, hogy két személy vagy két folyamatok próbálja meg a DNS-rekord módosítása egyszerre. Melyik wins? És a győztes tudja, hogy azok már csak felülíródnak valaki más készítette módosítások?

Azure DNS ugyanaz az erőforrás egyidejű módosításai biztonságosan kezeléséhez használja a Etags. Minden DNS resource (a zóna vagy a rekord beállítása) tartalmaz egy Etag társítva. Amikor egy erőforrás veszi, annak Etag is keresi. Erőforrás frissítésekor továbbítani vissza az Etag Azure DNS ellenőrizheti, hogy a Etag a kiszolgáló egyezéseket lehetősége van. Minden erőforrás frissítése a folyik Etag eredményez, mivel a egy Etag eltérés azt jelzi, egyidejű módosítás történt. Etags is használják új erőforrás létrehozásakor annak érdekében, hogy az erőforrás még nem létezik.

Alapértelmezés szerint Azure DNS PowerShell zónákba egyidejű változtatások letiltása, és rögzítse beállítása Etags használja. A választható *-felülírása* kapcsoló Etag ellenőrzések mellőzése használható, ebben az esetben egyidejű előfordult módosításokat felülírja.

Az Azure DNS REST API-t a szintjén Etags megjelennek a HTTP-fejlécek használatával.  Az alábbi táblázat viselkedése számítható ki:

|Élőfej|Működése|
|------|--------|
|Nincs lehetőség|Mindig a sikeres (nincs Etag ellenőrzések) helyezése|
|Ha-egyezés<etag>|HELYEZÉSE csak sikerül, ha az erőforrás van, és Etag megfelelő|
|Ha-hol.van *     | HELYEZÉSE csak sikerül, ha létezik erőforrás|
|Ha-nincs – egyezés * |  HELYEZÉSE csak sikerül, ha nem létezik erőforrás|

### <a name="tags"></a>Címkék

Címkék Etags eltérnek. Címkék neve – érték párokká listáját, és Azure-kezelő által címke erőforrásokhoz számlázási vagy csoportosítási célokat szolgálnak. További információt a címkék [használata címkék rendszerezheti az Azure erőforrások](../resource-group-using-tags.md)megtekintése

Azure DNS PowerShell címkék támogatja a zónákkal és a rekord beállítása a megadott beállítások segítségével `-Tag` paraméter.


## <a name="before-you-begin"></a>Első lépések

Ellenőrizze, hogy rendelkezik-e az alábbi elemek a konfigurációban megkezdése előtt.

- Egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).

- Telepítse az Azure erőforrás-kezelő PowerShell-parancsmagok (1.0 vagy újabb) legújabb verzióját kell. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.

## <a name="step-1---sign-in"></a>Lépés: 1 – bejelentkezés

Nyissa meg a PowerShell konzolt, és csatlakozni a fiókjához. További tudnivalókért olvassa el a [Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md)című témakört.

Használja az alábbi példa csatlakozni:

    Login-AzureRmAccount

Jelölje be az előfizetések a fiókhoz.

    Get-AzureRmSubscription

Adja meg az előfizetést, szeretné használni.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Lépés: 2 - erőforrás csoport létrehozása

Azure erőforrás-kezelő igényel, hogy az összes erőforráscsoport adjon meg egy helyet. Ez az erőforrások erőforráscsoport az alapértelmezett helye szolgál. Jó helyen jár, mivel összes DNS-erőforrás a globális és nem regionális, erőforrás csoport helyét választható nincs hatással van Azure DNS.

Kihagyhatja ezt a lépést, ha egy meglévő erőforráscsoport használja.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Lépés a 3 - nyilvántartásban

Az Azure DNS-szolgáltatás a Microsoft.Network erőforrás szolgáltató kezeli. Azure-előfizetése van szüksége, ez az erőforrás-szolgáltató használata az Azure DNS használata előtt regisztrálását. Az egyes előfizetés egyszeri műveletet.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Lépés: 4 - hozzon létre egy DNS-zóna

A DNS-zóna használatával hozzák létre a `New-AzureRmDnsZone` parancsmag. Nincsenek példák alatti hozhat létre a DNS-zóna, vagy a címkék nélkül. További információt a címkék című [címkék](#tags) ebben a cikkben.

>[AZURE.NOTE] Az Azure DNS zone nevét kell megadni, egy megszakítása nélkül **"."**. Például: "**a contoso.com**" helyett "**contoso.com.**".

### <a name="to-create-a-dns-zone"></a>A DNS-zóna létrehozása

Az alábbi példában hoz létre a DNS-zóna *contoso.com* nevű *MyResourceGroup*néven erőforrás csoportjában található. A példa hozhat létre a DNS-zóna, helyettesítése a saját értékeit.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>A DNS-zóna létrehozását címkék

A következő példa bemutatja, hogyan hozhat létre a DNS-zóna két címkékkel *project = bemutató* és *Boríték = teszt*. A példa hozhat létre a DNS-zóna, helyettesítése a saját értékeit.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Rekordok megtekintése

Is létrehozásához a DNS-zóna hozza létre a következő DNS-rekordokat:

- A *Szervezet indítása* (SOA) rekord. Ez a bemutató minden DNS-zóna gyökérszintjén.

- A mérvadó névkiszolgálói (NS) rekordok. Ezek megjelenítése, mely névkiszolgálók üzemeltet a zóna. Azure DNS névkiszolgálók erőforráskészlethez tartozik használja, és így más névkiszolgálók is rendelt különböző zónák Azure DNS-ben. Lásd: [a tartomány Azure DNS delegálása](dns-domain-delegation.md) további információt.

Ezeket a rekordokat tekintheti `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Rekord beállítása a legfelső szintű (vagy a *csúcs*) a DNS-zóna használati **@** szerint a rekord nevét.


## <a name="test"></a>Teszt

A DNS-zóna tesztelheti a DNS-eszközök, például az nslookup, alaposabban vagy a [Feloldás-Dns_név PowerShell-parancsmag](https://technet.microsoft.com/library/jj590781.aspx)használatával.

Hogy Ön még nem még a tartomány használata az új zóna Azure DNS-ben, szüksége lesz irányítsa át a DNS-lekérdezés közvetlenül az egyik a Névkiszolgálók a zóna. A Névkiszolgálók a zóna szerepelnek a Névkiszolgáló-rekordok által megjelenített `Get-AzureRmDnsRecordSet` felett. Ügyeljen a helyette a megfelelő értékeket, a zóna be az alábbi parancsot.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Következő lépések

Miután létrehozta a DNS-zóna, létrehozása [rekord beállítása és a rekordok](dns-getstarted-create-recordset.md) névfeloldás tartománya internetes indításához.

