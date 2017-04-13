<properties 
    pageTitle="Hálózati konfigurációs részleteket a Express továbbítására" 
    description="Hálózati konfiguráció adatai futó alkalmazás környezetek egy virtuális hálózatok egy készült ExpressRoute áramkör csatlakozik." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Hálózati konfigurációs részleteket az alkalmazás szolgáltatás környezetek készült ExpressRoute 

## <a name="overview"></a>– Áttekintés ##
Ügyfelek csatlakoztatása az [Azure készült ExpressRoute] [ ExpressRoute] áramkör a virtuális hálózati infrastruktúrájába, így Azure kiterjesztése a helyszíni hálózaton.  Az alkalmazás-szolgáltatási környezetben hozhat létre [virtuális hálózati] alhálózat[ virtualnetwork] infrastruktúra.  Az alkalmazás-szolgáltatási környezetben futó alkalmazások majd hozhat létre a biztonságos kapcsolatok háttéradatbázist erőforrásokhoz elérhető csak az készült ExpressRoute-kapcsolaton keresztül.  

Az alkalmazás-szolgáltatási környezetben hozható létre a **vagy** az erőforrás-kezelő Azure virtuális hálózati **vagy** klasszikus környezetet alakítana modell virtuális hálózati.  A legutóbbi módosítás június 2016-ban készült ASEs most is elvégezhető virtuális használó hálózatok nyilvános címtartományok vagy RFC1918 cím szóközt (azaz a a magánjellegű címeket). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Szükséges a hálózati kapcsolat ##
Előfeltételei hálózati kapcsolat, előfordulhat, hogy nem kell először éri el a virtuális hálózatban csatlakozik egy készült ExpressRoute alkalmazás szolgáltatási környezetben.  Alkalmazás környezetek annak érdekében, hogy a megfelelő működéshez csak a következő lépéseket:


-  Azure tároló végpontjait világszerte mindkét 80 és 443-as port kimenő hálózati kapcsolat.  Ide tartoznak a végpontok az alkalmazás-szolgáltatási környezetben, valamint a tárhely végpontok található **más** Azure régiók ugyanabban a régióban található.  Azure tárolás végpontok megoldásához a következő DNS-tartományok: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* és *file.core.windows.net*.  
-  A port 445 Azure fájlok szolgáltatást kimenő hálózati kapcsolat.
-  Kimenő hálózati kapcsolat Sql-adatbázis végpontok ugyanabban a régióban, mint az alkalmazás-szolgáltatási környezetben található.  SQL-adatbázis végpontok feloldása csoportban a következő tartomány: *database.windows.net*.  Ehhez a hozzáférést portokhoz 1433, 11000-11999 és 14000-14999 megnyitása.  Lásd: [Ez a cikk a Sql-adatbázis V12 port használatát](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Kimenő hálózati kapcsolat az Azure management sík végpontokat (ASM és a ARM végpontok).  Ide tartoznak a kimenő kapcsolódási *management.core.windows.net* és *management.azure.com*. 
-  Kimenő a hálózati kapcsolat *ocsp.msocsp.com*, *mscrl.microsoft.com* és *crl.microsoft.com*.  A szükséges SSL-funkciókat támogatja.
-  A virtuális hálózati DNS-konfigurációjának képes feloldása az összes végpontok és a korábbi pontok említett tartományok kell lennie.  Ha ezeket a végpontokat nem oldható meg, alkalmazás szolgáltatási környezetben létrehozási kísérletek sikertelenek lesznek, és a meglévő alkalmazás környezetek megjelöli a sérült.
-  A port 53 kimenő hozzáférés szükség a DNS-kiszolgálók való kommunikáció.
-  Ha egy egyéni DNS-kiszolgáló megtalálható a virtuális Magánhálózati az átjárók másik végét, a DNS-kiszolgáló érhetők el az alkalmazás-szolgáltatási környezetben tartalmazó alhálózat felosztással kell lennie. 
-  A kimenő hálózati elérési út nem utazási belső vállalati proxyk keresztül, és nem is kell a helyszíni bújtatott hatályba.  Ha így módosítja a hatékony hálózati Címfordítást cím kimenő hálózati forgalmának az alkalmazás-szolgáltatási környezetben.  Alkalmazás szolgáltatási környezetben kimenő hálózati forgalmat a hálózati Címfordítást címe módosításakor a fent felsorolt végpontok számos kapcsolódási hibák.  Ennek eredményeképpen sikertelen alkalmazás szolgáltatási környezetben létrehozási kísérletek, valamint a korábban megfelelő alkalmazás szolgáltatás környezetekben a sérült éppen megjelölt.  
-  Hálózati hozzáférés szükséges portokon bejövő, az alkalmazás-szolgáltatási környezetben kell tenni, a jelen [cikkben]leírt módon[requiredports].

A DNS követelményeknek is teljesülniük érvényes DNS-infrastruktúrát van beállítva, és a virtuális hálózat fenntartani biztosításával.  Ha a DNS-konfigurációs módosítják az alkalmazás-szolgáltatási környezetben létrehozását követően bármilyen okból fejlesztők kényszerítheti az alkalmazás-környezetből felveszi az új DNS-konfigurációs.  Az [Azure portál] az alkalmazás szolgáltatási környezetben kezelése lap tetején található a működés közben környezet újra kell indítani az "Újraindítása" ikonnal kiváltó[ NewPortal] a környezet felveszi az új DNS-konfigurációja miatt.

A bejövő hálózati hozzáférés követelményeknek is teljesülniük [hálózati biztonsági csoport] konfigurálásával[ NetworkSecurityGroups] a szükséges hozzáférés engedélyezése a jelen [cikkben]ismertetett módon az alkalmazás szolgáltatási környezetben alhálózat[requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben kimenő a hálózati kapcsolat engedélyezése##
Alapértelmezés szerint egy újonnan létrehozott készült ExpressRoute áramkör meghirdetése alapértelmezett útvonal, amely lehetővé teszi a kimenő internetkapcsolat.  Ebben a konfigurációban csatlakozhat más Azure végpontok lesz az alkalmazás-szolgáltatási környezetben.

Azonban a közös ügyfél konfiguráció, hogy a saját alapértelmezett útvonal (0.0.0.0/0), amely kezd kimenő internetes forgalmat inkább a helyszíni megadása.  A forgalom folyamat eredményeként töréspontok alkalmazás szolgáltatási környezetben, mert a kimenő forgalmának vagy letiltott a helyszíni, vagy hálózati Címfordítást szeretne egy címet, amely nem működnek a különböző Azure végpontokhoz nem felismerhető halmazára.

A megoldás, ha egy (vagy több) a felhasználó által definiált útvonalak (UDRs) megadása az alhálózat, amely tartalmazza az alkalmazás-szolgáltatási környezetben.  Egy UDR lesz az alapértelmezett útvonal helyett kell fogadja alhálózat-specifikus útvonalak határozza meg.

Ha lehetséges javasolt a következő beállításokat használja:

- A készült ExpressRoute konfiguráció meghirdetése 0.0.0.0/0 és alapértelmezés szerint a kötelező az összes kimenő forgalmat a helyszíni bújtatja.
- Az alkalmazás-szolgáltatási környezetben tartalmazó az alhálózathoz alkalmazza a UDR (ilyen például a is megtalálhatók lefelé, a jelen cikkben) internetes következő azonosítható típusú 0.0.0.0/0 határozza meg.

Ezeket a lépéseket a kombinált hatása, hogy UDR alhálózat szintjét elsőbbséget élveznek tunneling, kényszerített készült ExpressRoute ezáltal biztosítva kimenő Internet-hozzáférés az alkalmazás szolgáltatás környezetből.

> [AZURE.IMPORTANT] A útvonalak definiált UDR **kell** , hogy jellemző elsőbbséget élveznek bármely utakat, a készült ExpressRoute konfiguráció közzététel.  Az alábbi példában a széles 0.0.0.0/0 címtartományokat használja, és olyan esetleg véletlenül felülbírálhatja útvonal hirdetések szűkebb címtartományokat használja.
>
>Alkalmazás-szolgáltatási környezetben, amelyek nem támogatott, hogy **nyilvános peering elérési útját a magánjellegű peering elérési útvonalak határokon meghirdetése**készült ExpressRoute konfigurációk.  Van beállítva, nyilvános peering készült ExpressRoute konfigurációk kap útvonal hirdetések a Microsoft a Microsoft Azure IP-címtartományok nagy számú.  Ha ezek címtartományok határokon közzététel a személyes peering útvonalon, a befejezési eredménye, hogy az alkalmazás szolgáltatási környezetben alhálózat az összes kimenő hálózati csomagok hatályba bújtatott ügyfél helyszíni hálózati infrastruktúrájába lesz.  A hálózati folyamat jelenleg nem támogatja az alkalmazás-szolgáltatási környezetben.  A probléma megoldása egy le szeretné állítani a nyilvános peering elérési útvonalak határokon-hirdetési magánjellegű peering elérési.

A felhasználó által definiált útvonalon háttér-információk érhető el az [Áttekintés][UDROverview].  

Részletes tudnivalókat létrehozása és konfigurálása a felhasználó által definiált útvonalak érhető el ez [Útmutatókat][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Példa UDR konfigurációs-szolgáltatási alkalmazás környezetben ##

**Előzetes követelmények**

1. Azure Powershell telepítése a [Azure letöltési lapját] [ AzureDownloads] (dátummal június 2015-ös vagy újabb).  A "Parancssori eszközök" nincs a telepíti a legújabb Powershell-parancsmagok "Windows Powershell" egy "Telepítés" hivatkozás.

2. Javasoljuk, hogy egyedi alhálózat hozta kizárólagos használható az alkalmazás-szolgáltatási környezetben.  Ezzel biztosíthatja, hogy az alhálózathoz alkalmazott UDRs csak nyílik meg a kimenő forgalom az alkalmazás szolgáltatási környezetben.
3. **Fontos**: az alkalmazás-szolgáltatási környezetben nem telepítése **után** az alábbi konfigurálási lépéseket betartják-ig.  Ez biztosítja, hogy kimenő a hálózati kapcsolat érhető el az alkalmazás-szolgáltatási környezetben üzembe előtt.

**Lépés: 1: Elnevezett útvonal táblázat létrehozása**

A következő kódtöredékének táblát hoz létre útvonal "DirectInternetRouteTable" nevű a nyugati US Azure tartományban lévő:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Lépés: 2: Hozzon létre egy vagy több útvonalak az útvonal táblában**

Szüksége lesz egy vagy több útvonalak hozzáadása az útvonal táblázat engedélyezze a kimenő Internet-hozzáférés érdekében.  

A javasolt megközelítés kimenő Internet-hozzáféréssel konfigurációs célja meghatározása útvonal 0.0.0.0/0 alább látható módon.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Ne feledje, hogy 0.0.0.0/0 széles cím tartomány, és olyan felülbírálják a készült ExpressRoute által közzététel szűkebb címtartományok.  Újra találta a korábbi ajánlást, egy UDR 0.0.0.0/0 útvonal kell használni, valamint 0.0.0.0/0 csak meghirdetése ExressRoute beállításokkal együtt. 

Alternatív megoldásként töltheti le egy teljes és a frissített felsorolást CIDR Azure használja.  Az XML-fájlt tartalmazó összes az Azure IP-címtartományok érhető el a [Microsoft letöltőközpontból][DownloadCenterAddressRanges].  

Vegye figyelembe, hogy ezeket a tartományokat időről időre változnak, így szükségessé periodikus manuális frissítésről, hogy a felhasználó által definiált útvonalak szinkronizálásához.  Is mivel alapértelmezett felső legfeljebb 100 útvonalak egy egyetlen UDR, "Összegzés" az Azure IP-címtartományai 100 útvonal keretei igazítása kell szem előtt tartva, hogy UDR definiált útvonalak kell lenniük, mint útvonalak szűkebb közzététel által a készült ExpressRoute.  


**3 lépés: A hozzárendeléssel útvonalat az alhálózathoz tartalmazó az alkalmazás-szolgáltatási környezetben**

Az utolsó konfigurációs lépésként társíthatja az útvonal táblázat az alhálózathoz hol az alkalmazás-szolgáltatási környezetben rendszerbe állított.  A következő parancsot a "DirectInternetRouteTable" a "ASESubnet"-alkalmazás szolgáltatási környezetben ahányat tartalmazó való társít.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Lépés: 4: Utolsó lépései**

Miután az útvonal táblázat az alhálózathoz kötött, javasolt először ellenőrzi, és erősítse meg a kívánt effektust.  Például egy virtuális számítógépre telepítése a alhálózat be, és győződjön meg arról, hogy:


- Azure és nem Azure végpontok kimenő forgalmat a korábbiak a jelen cikkben **nem** halad a készült ExpressRoute áramkör lefelé.  Nagyon fontos, hogy ez a jelenség, ellenőrizze, mivel az alhálózathoz kimenő forgalom esetén továbbra is kényszerített bújtatott a helyszíni alkalmazás szolgáltatási környezetben mindig sikertelen lesz. 
- DNS-kereséseket, a végpontok korábbiak összes feloldása megfelelően. 

Ha a fenti lépések megerősítést, szüksége lesz törölheti a virtuális gépen, mert a alhálózat szükséges "üres" létrejön az alkalmazás-szolgáltatási környezetben időben.
 
Kattintson az alkalmazás-szolgáltatási környezetben létrehozása folytatásához!

## <a name="getting-started"></a>Első lépések
Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Az alkalmazás-szolgáltatási környezetben, című cikkben ismerkedhet [alkalmazás szolgáltatási környezetben – bevezetés][IntroToAppServiceEnvironment]

Az Azure alkalmazás szolgáltatás platform kapcsolatos további tudnivalókért lásd a [Azure alkalmazás szolgáltatás][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
