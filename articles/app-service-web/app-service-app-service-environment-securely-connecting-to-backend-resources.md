<properties 
    pageTitle="Alkalmazás szolgáltatás környezetből forrá Kódmentes biztonságosan összekötő" 
    description="Tudnivalók arról, hogy miként biztonságosan csatlakoztathassa forrá kódmentes alkalmazás szolgáltatás környezetből." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Alkalmazás szolgáltatás környezetből forrá Kódmentes biztonságosan összekötő #

## <a name="overview"></a>– Áttekintés ##
Mivel az alkalmazás-szolgáltatási környezetben mindig jön létre a **vagy** az erőforrás-kezelő Azure virtuális hálózati **vagy** klasszikus környezetet alakítana modell [virtuális hálózati][virtualnetwork], kódmentes anyagainak kimenő kapcsolatok alkalmazás szolgáltatás környezetből is flow kizárólag a virtuális hálózaton keresztül.  A legutóbbi módosítás június 2016-ban készült ASEs is telepíthető be használó nyilvános címtartományok vagy RFC1918 cím szóközt (azaz virtuális hálózatok a magánjellegű címeket).  

Ha például lehetnek olyan a fürthöz a porthoz zárolva 1433 a virtuális gépeken futó SQL Server.  A végpont lehet, hogy csak a virtuális hálózaton a más forrásokból hozzáférés engedélyezése ACLd.  

Másik példaként a bizalmas végpontok futtathatja a helyszíni, és csatlakozzon az Azure keresztül vagy [Webhely] [ SiteToSite] vagy [a készült Azure ExpressRoute] [ ExpressRoute] kapcsolatokat.  Csak a virtuális hálózatok csatlakozik a webhely, illetve készült ExpressRoute alagutak erőforrások eredményeként hozzáférhetnek a helyszíni végpontok lesz.

Az összes ilyen helyzetekkel ismerkedhet apps az alkalmazás-szolgáltatási környezetben futó tudja, hogy biztonságosan csatlakoztathassa a különböző kiszolgálók és erőforrások lesz.  Az alkalmazás-szolgáltatási környezetben virtuális ugyanabba a hálózatba magánjellegű végpontok operációs rendszert futtató alkalmazásokból kimenő forgalom (vagy a virtuális hálózatához), csak a folyamat lesz a virtuális hálózaton keresztül.  Kimenő forgalmának magánjellegű végpontokhoz nem enged a nyilvános interneten keresztül.

Egy bővítve vonatkozik kimenő forgalmának alkalmazás szolgáltatás környezetből végpontokhoz virtuális hálózaton belül.  Alkalmazás környezetek nem éri el a virtuális gépeken futó **ugyanahhoz** az alhálózathoz, mint az alkalmazás-szolgáltatási környezetben található végpontjait.  Ez általában nem kell probléma mindaddig, amíg az alkalmazás szolgáltatási környezetben van telepítve, az alkalmazás szolgáltatási környezetben számára kizárólagos fenntartva alhálózat be.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Kimenő kapcsolatok és a DNS-követelményei ##
Az alkalmazás szolgáltatás csak akkor működik megfelelően környezetben szükséges különböző végpontok kimenő elérését. A külső, egy ASE által használt végpontokat teljes listáját a "Hálózati kapcsolat szükséges" szakasz a [Hálózati konfigurálásról számára készült ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) cikk szerepel.

Alkalmazás szolgáltatási környezetben konfigurálva a virtuális hálózati érvényes DNS-infrastruktúrát szükség.  Ha a DNS-konfigurációs módosítják az alkalmazás-szolgáltatási környezetben létrehozását követően bármilyen okból fejlesztők kényszerítheti az alkalmazás-környezetből felveszi az új DNS-konfigurációs.  A működés közben környezet újra kell indítani a portálon az alkalmazás-szolgáltatási környezetben kezelése lap tetején található "Újraindítás" ikonnal kiváltó okoz a környezet felveszi az új DNS-konfigurációs.

Ajánlott is, hogy minden olyan egyéni DNS-kiszolgálók a vnet telepíthető az alkalmazás-szolgáltatási környezetben létrehozása előtt időszakokra.  Ha egy virtuális hálózat DNS konfigurálása az alkalmazás-szolgáltatási környezetben létrehozása közben megváltozik, hogy az alkalmazás-szolgáltatási környezetben létrehozási folyamat meghibásodása eredményezi.  Egy hasonló tekintettel az egyéni DNS-kiszolgáló megtalálható-e VPN az átjárók másik végét, és a DNS-kiszolgáló nem érhető el vagy nem érhető el, ha az alkalmazás-szolgáltatási környezetben létrehozásának folyamatát is sikertelen lesz.

## <a name="connecting-to-a-sql-server"></a>Csatlakozás SQL Server
A leggyakoribb SQL Server-konfiguráció port 1433 figyelését zárólap foglalja magában:

![Az SQL Server-végpontot][SqlServerEndpoint]

Kétféleképpen a forgalmat a végpont korlátozása:


- [Hálózati hozzáférés listákat] [ NetworkAccessControlLists] (hálózati hozzáférés-vezérlési listák)

- [Hálózati biztonsági csoportok][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>A hálózat vezérlés hozzáférés korlátozása

A port 1433 hálózati hozzáférés-vezérlési lista védhetők.  Whitelists ügyfél az alábbi példában egy virtuális hálózaton belül származó címek, és blokkolja a többi ügyfelek számára.

![Hálózati hozzáférés vezérlő listában például][NetworkAccessControlListExample]

Az SQL Servert futtató virtuális ugyanabba a hálózatba alkalmazás-szolgáltatási környezetben futó alkalmazások tud csatlakozni az SQL Server-példányt a **belső VNet** IP-cím használatával az SQL Server virtuális gép lesz.  

A példa kapcsolati karakterláncot az alábbi hivatkozások az SQL Server magánjellegű IP-címére.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Habár a virtuális gép egy nyilvános végpontot, a nyilvános IP-cím használatával kapcsolatfelvételi elutasításra a hálózat vezérlés miatt. 

## <a name="restricting-access-with-a-network-security-group"></a>Biztonsági csoport hálózati hozzáférés korlátozása
Hozzáférés biztosítása érdekében alternatív megközelítés hálózati biztonsági csoport lesz.  Hálózati biztonsági csoportok alkalmazható egyes virtuális gépeken futó, vagy egy virtuális gépeken futó tartalmazó alhálózat.

Először hálózati biztonsági csoport kell hozható létre:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

A hálózati biztonsági csoport rendkívül egyszerű csak VNet belső forgalom való hozzáférés korlátozása.  Az alapértelmezett szabályokat, a hálózati biztonsági csoport csak virtuális ugyanabba a hálózatba más hálózati ügyfelekről engedélyezi a hozzáférést.

Egyszerűen, a hálózati biztonsági csoport alapértelmezett szabályaival alkalmazása vagy a virtuális gépeken futó SQL Server vagy a alhálózat, ahol az a virtuális gépeken futó operációs rendszert futtató eredményt zárolása az access SQL Server.

Az alábbi példa a tartalmazó alhálózat hálózati biztonsági csoport vonatkozik:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
A végeredmény egy olyan biztonsági szabályokat, a külső hozzáférés letiltása VNet belső access téve:

![Alapértelmezett hálózati biztonsági szabályok][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Első lépések
Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Az alkalmazás-szolgáltatási környezetben, című cikkben ismerkedhet [alkalmazás szolgáltatási környezetben – bevezetés][IntroToAppServiceEnvironment]

Bejövő forgalom az alkalmazás szolgáltatás környezetbe szabályozása körül részletekért olvassa el [az alkalmazás-szolgáltatási környezetben bejövő forgalmat szabályozása][ControlInboundASE]

Az Azure alkalmazás szolgáltatás platform kapcsolatos további tudnivalókért lásd a [Azure alkalmazás szolgáltatás][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
