<properties 
    pageTitle="Alkalmazás-szolgáltatási környezetben – bevezetés" 
    description="Tudjon meg többet az alkalmazás szolgáltatási környezetben szolgáltatás, hogy a biztonságos, VNet csatlakozott, saját méretarányra egység összes alkalmazás futtatásához." 
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

# <a name="introduction-to-app-service-environment"></a>Alkalmazás-szolgáltatási környezetben – bevezetés

## <a name="overview"></a>– Áttekintés ##
Az alkalmazás-szolgáltatási környezetben egy [prémium] [ PremiumTier] Azure alkalmazás szolgáltatása, amely egy teljesen elszigetelt és dedikált környezet biztonságosan futtatásáról Azure alkalmazás szolgáltatás alkalmazások magas skála, beleértve a [Web Apps]csomag lehetőségét szolgáltatás[WebApps], [Mobile-alkalmazások][MobileApps], és az [API-alkalmazások][APIApps].  

Alkalmazás környezetek ideálisak az alkalmazás munkaterhelésekből megkövetelése:

- Nagyon magas skála
- Elkülönítési és biztonságos hálózati hozzáférés

Ügyfelek hozhat létre több alkalmazás szolgáltatási környezetben egyetlen Azure terület belül, illetve több Azure területek között.  Ideális vízszintesen méretezés az állapot-kevésbé alkalmazás rétegek támogató magas RPS munkaterhelésekből így alkalmazás szolgáltatási környezetben.

Alkalmazás környezetek elkülönítik csak egyetlen ügyfél-alkalmazások, és mindig telepítik egy virtuális hálózatba.  Ügyfelek mindkét bejövő és kimenő alkalmazás hálózati forgalmának engedélyezésére külön hozzáférésük van, és az alkalmazások nagy sebességű biztonságos kapcsolatokat hozhat létre virtuális hálózatokon helyszíni vállalati erőforrásokra mutató.

Az összes cikk, és hogyan – a hangpostáján kapcsolatos alkalmazás környezetek érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Hogyan alkalmazás környezetek magas skála engedélyezése és a biztonságos hozzáférés a hálózati című témakörben talál a [AzureCon-mély merülési] [ AzureConDeepDive] alkalmazás szolgáltatás környezetekkel kapcsolatos!

Mély a vízszintesen méretezést merülési egy több alkalmazás környezetek ismertető egy [alkalmazás geo elosztott helyigénye]beállítása a[GeodistributedAppFootprint].

Ha látni szeretné a biztonsági architektúrája, a AzureCon-mély merülési látható beállításaitól, ismertető végrehajtási egy [biztonsági architektúrája réteges](app-service-app-service-environment-layered-security.md) az alkalmazás-szolgáltatási környezetben.

Alkalmazás-szolgáltatási környezetben futó alkalmazások elérhetik a felsőbb szintű eszközök, például a webes alkalmazás tűzfalak (WAF) által kezdeményezett.  [Egy WAF az alkalmazás környezetek](app-service-app-service-environment-web-application-firewall.md) beállításáról a cikk bemutatja, ebben az esetben. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Saját számítási erőforrások ##
Az összes alkalmazás szolgáltatási környezetben számítási erőforrás van hozzárendelve kizárólag egyetlen előfizetés, és az alkalmazás-szolgáltatási környezetben egy bizonyos alkalmazás beállítható erőforrásokkal legfeljebb ötven (50) számítási kizárólagos használatra.

Az alkalmazás-szolgáltatási környezetben előtér-számítási erőforráskészlet, valamint egy-három dolgozó számítási erőforráskészletek tevődik össze. 

Az előtér-készlet számítási erőforrásokat, az alkalmazás-összehívások alkalmazás szolgáltatási környezetben is automatikus terheléselosztás SSL lemondási felelős tartalmazza. 

Minden dolgozó készlet tartalmaz számítási feladatoknak [Alkalmazás szolgáltatás tervek][AppServicePlan], amely viszont tartalmaz egy vagy több Azure alkalmazás szolgáltatás alkalmazások.  Mivel lehet legfeljebb három különböző dolgozó készletek az alkalmazás-szolgáltatási környezetben, kattintson az egyes dolgozó készlet különböző számítási erőforrások rugalmasan van.  

Például ez lehetővé teszi, hogy hozzon létre egy dolgozó készlet kisebb hatékony számítási erőforrásokkal az alkalmazás szolgáltatás tervek fejlesztés és vizsgálatot alkalmazás szánt.  A második (vagy akár a harmadik) dolgozó készletébe alkalmazás szolgáltatás tervek futtasson gyártási alkalmazásokat szánt több hatékony számítási erőforrások használhatja.

A számítási erőforrás elérhető az előtér és dolgozó készletek mennyisége, lásd: [az alkalmazás-szolgáltatási környezetben beállítása][HowToConfigureanAppServiceEnvironment].  

A rendelkezésre álló kapcsolatos részletekért kiszámítása az erőforrás méretű támogatott alkalmazás szolgáltatási környezetben, tekintse meg az [App szolgáltatás árak] [ AppServicePricing] lapon, és tekintse át a réteg árak prémium az alkalmazás környezetek lehetőségeiről.

## <a name="virtual-network-support"></a>Virtuális hálózat támogatása ##
Az alkalmazás-szolgáltatási környezetben hozható létre a **vagy** az erőforrás-kezelő Azure virtuális hálózati **vagy** klasszikus környezetet alakítana modell virtuális hálózati ([További információ a virtuális hálózatokon][MoreInfoOnVirtualNetworks]).  Mivel az alkalmazás-szolgáltatási környezetben mindig a virtuális hálózatban létezik, és pontosabban tartalmazó alhálózat virtuális hálózat, mind a bejövő és kimenő hálózati kommunikáció szabályozhatja virtuális hálózatok biztonsági funkciók is kihasználhatja.  

Az alkalmazás-szolgáltatási környezetben vagy egy nyilvános IP-címet, vagy a belső szemben lévő csak egy belső betöltés terheléselosztó Azure (ILB) címet az internetes lehet.

Használhatja a [hálózati biztonsági csoportok] [ NetworkSecurityGroups] bejövő hálózati kommunikáció az alkalmazás-szolgáltatási környezetben helye az alhálózathoz korlátozhatja.  Így mögött felsőbb szintű eszközök és szolgáltatások, például a webes alkalmazás tűzfalaiban és a hálózati szoftver szolgáltatók alkalmazások futtatásához.

Alkalmazások is gyakran kell vállalati erőforrások, például a belső adatbázisok elérése és webes szolgáltatások.  A közös megközelítés, ezeket a végpontokat elérhetővé csak az Azure virtuális hálózatból folyó belső hálózati forgalmának engedélyezésére.  Amikor az alkalmazás-szolgáltatási környezetben csatlakozik a belső szolgáltatásként virtuális ugyanabba a hálózatba, alkalmazások fut a környezetben is elérheti őket, többek között a [Webhely-webhelyen] keresztül érhető el a végpontok[ SiteToSite] és [a készült Azure ExpressRoute] [ ExpressRoute] kapcsolatok.

Az alkalmazás-szolgáltatási környezetben működése a virtuális hálózatok és a helyszíni hálózatok rendszeren, olvassa el az alábbi cikkekben a [Hálózat architektúrája][NetworkArchitectureOverview], [Bejövő forgalom szabályozása][ControllingInboundTraffic], és a [Biztonságos kapcsolatot háttérkiszolgálókon][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Első lépések

Első lépések az alkalmazás-szolgáltatási környezetben, lásd: [Hogyan kattintva hozzon létre egy alkalmazás szolgáltatási környezetben][HowToCreateAnAppServiceEnvironment]

Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Az Azure alkalmazás szolgáltatás platform kapcsolatos további tudnivalókért lásd a [Azure alkalmazás szolgáltatás][AzureAppService].

Az alkalmazás-szolgáltatási környezetben hálózati architektúra áttekintése című témakörben találhat a [Hálózat architektúrája áttekintése] [ NetworkArchitectureOverview] cikk.

A készült ExpressRoute, az alkalmazás-szolgáltatás környezet használatával című témakör tartalmaz a következő cikk [Express továbbításához]és az alkalmazás-szolgáltatási környezetben[NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
