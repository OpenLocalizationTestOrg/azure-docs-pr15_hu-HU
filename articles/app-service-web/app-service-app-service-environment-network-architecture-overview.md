<properties 
    pageTitle="Alkalmazás környezetek hálózat architektúrája áttekintése" 
    description="Hálózati topológiája ofApp környezetek építészeti áttekintése." 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>Alkalmazás környezetek hálózat architektúrája áttekintése

## <a name="introduction"></a>– Bevezetés ##
Alkalmazás-szolgáltatási környezetben mindig jönnek létre [virtuális hálózati] alhálózat[ virtualnetwork] -alkalmazás szolgáltatási környezetben működik alkalmazások kommunikálni az azonos virtuális hálózati topológiája található személyes végpontok.  Mivel ügyfelek lehet zárolni a virtuális hálózati infrastruktúrát részei, célszerű az alkalmazás-szolgáltatási környezetben előforduló hálózati kommunikációs flow típusai.

## <a name="general-network-flow"></a>Általános hálózati továbbításához ##
 
Egy alkalmazás szolgáltatási környezetben (SKB) alkalmazások használ egy nyilvános virtuális IP-cím (virtuális), az összes bejövő forgalmat meg, hogy nyilvános virtuális elküldésük után.  Ide tartoznak a http- és HTTPS-forgalmat-alkalmazások és más forgalom az FTP, távoli hibakeresési szolgáltatás és Azure adatkezelési műveletek.  Az adott portokat (kötelező és nem kötelező) egy teljes listát, a nyilvános virtuális a rendelkezésre álló ismertető a [bejövő szabályozása] [ controllinginboundtraffic] -alkalmazás szolgáltatási környezetben. 

Alkalmazás szolgáltatási környezetben is támogat, amely csak a virtuális hálózati belső címet, más néven ILB (belső terheléselosztó) címre kötve futó alkalmazások.  A egy ILB engedélyezett ASE, HTTP és HTTPS-forgalmat alkalmazások, valamint a távoli hibakeresési hívásokat, mire eljut az üzenetem a ILB címen.  Leggyakoribb ILB-ASE konfigurációk esetén FTP/FTPS forgalom is érkezik a ILB címen.  Azure adatkezelési műveletek fog továbbra is flow portokra 454/455 meg a nyilvános virtuális egy ILB, azonban engedélyezve van a ASE.

Az alábbi ábrán egy alkalmazás szolgáltatási környezetben, ahol az alkalmazások kötődnek egy nyilvános virtuális IP-cím a különböző bejövő és kimenő hálózati folyamatokra áttekintése látható:

![Általános hálózati flow][GeneralNetworkFlows]

Az alkalmazás-szolgáltatási környezetben magánjellegű ügyfél végpontok számos kommunikálhat.  Az alkalmazás-szolgáltatási környezetben futó alkalmazások például adatbázis-kiszolgálói a azonos virtuális hálózati topológia IaaS virtuális gépeken futó csatlakozhat.

>[AZURE.IMPORTANT] Megjeleníti a hálózat diagramon, a "egyéb kiszámítania erőforrások" telepítik egy másik alhálózat az alkalmazás-szolgáltatási környezetben. Üzembe helyezése a ASE ugyanahhoz az alhálózathoz erőforrásainak blokkolja és ezek az erőforrások (kivéve az adott belüli-ASE Útválasztás) ASE közötti kapcsolatot. Beállítaná egy másik alhálózat helyette (az azonos VNET). Az alkalmazás-szolgáltatási környezetben lesz képes csatlakozni. További beállítás nem szükséges.

Alkalmazás szolgáltatási környezetben is kommunikálni Sql DB és Azure tárolási erőforrások kezelésére és az alkalmazás-szolgáltatási környezetben működő szükséges.  A Sql és tárolási forrásokhoz, hogy az alkalmazás-szolgáltatási környezetben kommunikál találhatók az ugyanazon régió szerint az alkalmazás-szolgáltatási környezetben, míg mások távoli Azure régióban találhatók.  Emiatt az internethez kimenő kapcsolatok mindig szükség az alkalmazás-környezetből működik megfelelően. 

Mivel az alhálózathoz telepíti az alkalmazás-szolgáltatási környezetben, hálózati biztonsági csoportok szabályozhatja az alhálózathoz bejövő forgalom használható.  A bejövő forgalom az alkalmazás-szolgáltatási környezetben, hogy miként című témakör tartalmaz a következő [cikk][controllinginboundtraffic].

Engedélyezze a kimenő internetkapcsolat az alkalmazás-szolgáltatási környezetben olvashat a következő témakör [Express útvonal]használatáról a[ExpressRoute].  A cikkben ismertetett ugyanezt a megközelítést érvényes, ha a webhely kapcsolat használata, és segítségével kényszerített tunneling.

## <a name="outbound-network-addresses"></a>Kimenő hálózati címek ##
Amikor az alkalmazás-szolgáltatási környezetben a kimenő hívások lehetővé teszi, a IP-címének mindig társítva a kimenő hívások.  Az adott IP-cím használt attól függ, hogy a hívott végpont helyezkedik el a virtuális hálózati topológiája és a virtuális hálózati topológiája kívül.

Ha a hívott végpont **kívül esik** , a virtuális hálózati topológiája, a kimenő címét (más néven a kimenő hálózati Címfordítást), amely a nyilvános virtuális, az alkalmazás-szolgáltatási környezetben.  A cím Tulajdonságok lap alkalmazás szolgáltatás környezethez portál felhasználói felületén található.
 
![Kimenő IP-cím][OutboundIPAddress]

A cím is, amelyek csak egy nyilvános virtuális az alkalmazás-szolgáltatási környezetben az alkalmazások létrehozását, és kattintson az alkalmazás címét végrehajtása az *nslookup* ASEs kell meghatározni. Az eredményül kapott IP-cím, mind a nyilvános virtuális, valamint az alkalmazás szolgáltatási környezetben kimenő hálózati Címfordítást címet.

Ha a hívott végpont **belül** , a virtuális hálózati topológiája, a hívó alkalmazás kimenő címe lesz az egyes számítási erőforrás az alkalmazást futtató belső IP-címét.  Azonban nem szerepel a virtuális hálózati belső IP-címek és alkalmazások állandó megfeleltetés.  Alkalmazások különböző számítási források és az erőforrások az alkalmazás-szolgáltatási környezetben módosíthatja a méretezés műveletek miatt elérhető számítási készlete mozgathatók.

Azonban az alkalmazás-szolgáltatási környezetben alhálózat mindig helyezkedik el, mivel meg vannak garantálni, hogy a számítási erőforrás alkalmazást futtató belső IP-címe mindig elhelyezkednie alhálózat CIDR tartományon belül.  Emiatt külön hozzáférés-vezérlési listák vagy hálózati biztonsági csoportok használata esetén a virtuális hálózaton belül más végpontok való hozzáférés biztonságossá tétele, az alkalmazás-szolgáltatási környezetben tartalmazó alhálózat tartományra kell kell adni a hozzáférést.

Az alábbi ábra mutatja az alábbi fogalmak részletesen:

![Kimenő hálózati címek][OutboundNetworkAddresses]

Ha a fenti diagramban:

- Mivel az alkalmazás-szolgáltatási környezetben, a nyilvános virtuális 192.23.1.2, ez az "Internet" végpontokhoz híváskezdeményezési használt kimenő IP-címét.
- Az alkalmazás-szolgáltatási környezetben tartalmazó alhálózat CIDR tartománya 10.0.1.0/26.  Más végpontok belül az azonos virtuális hálózati infrastruktúrát alkalmazásokból hívások látnak származó valahol a cím tartományon belül.

## <a name="calls-between-app-service-environments"></a>Alkalmazás környezetek közötti hívások ##
Példa az összetettebb akkor fordulhat elő, ha telepítése több alkalmazás környezetek virtuális ugyanabba a hálózatba, és egy másik alkalmazás szolgáltatási környezetben egyik alkalmazás szolgáltatás környezetből kimenő hívások.  Az ilyen típusú közötti hívások is számít "Internetes" hívások alkalmazás szolgáltatási környezetben.

A következő ábrán egy példa architektúra egy alkalmazás szolgáltatási környezetben (például a "Elülső ajtó" web Apps alkalmazások) alkalmazást az alkalmazások felhívja a második alkalmazás szolgáltatási környezetben (például belső háttéradatbázist API-alkalmazások nem lesznek elérhetők az internetről szándékozik). 

![Alkalmazás környezetek közötti hívások][CallsBetweenAppServiceEnvironments] 

A fenti példa az alkalmazás-szolgáltatási környezetben "ASE egy" 192.23.1.2 kimenő IP-címet tartalmaz.  Ha az alkalmazás ez futó alkalmazás szolgáltatási környezetben hoz létre egy kimenő hívást egy másik alkalmazás szolgáltatási környezetben ("ASE két") futó alkalmazás található virtuális hálózatához, a kimenő hívás lesz egy "Internet" hívás számít.  Emiatt a hálózati forgalmának engedélyezésére, a második érkező alkalmazás szolgáltatási környezetben jelennek meg, származó 192.23.1.2 (tehát nem a alhálózat címtartományokat az első alkalmazás-környezet).

Annak ellenére, hogy az alkalmazás-szolgáltatás különböző környezetekben közötti hívások számít "Internet" hívásokat, ha két alkalmazás szolgáltatási környezetben ugyanabban a Azure régióban találhatók a hálózati forgalmat továbbra is a regionális Azure hálózaton, és fizikailag nem enged a nyilvános interneten keresztül.  Eredményt segítségével hálózati biztonsági csoport a második alkalmazás-környezet az alhálózat Tevékenységfrissítések engedélyezése csak a bejövő hívások a az első alkalmazás szolgáltatás-környezetből (amelynek kimenő IP-címet a 192.23.1.2), ezáltal biztosítva a alkalmazás szolgáltatási környezetben közötti biztonságos kommunikáció.

## <a name="additional-links-and-information"></a>További hivatkozások és információk ##
Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Részletes tudnivalókat bejövő alkalmazás környezetek által használt portokat és a hálózati biztonsági csoportok használatával szabályozhatja a bejövő érhető el [az alábbi][controllinginboundtraffic].

Használatáról a felhasználó által definiált útvonalak adja meg az alkalmazás-szolgáltatási környezetben kimenő Internet-hozzáféréssel érhető el ez a [cikk][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

