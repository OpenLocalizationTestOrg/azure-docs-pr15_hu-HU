<properties 
    pageTitle="Alkalmazás-szolgáltatási környezetben |} Microsoft Azure" 
    description="Mi az Azure-alkalmazás szolgáltatási környezetben? Bevezetés az alkalmazás-szolgáltatási környezetben." 
    keywords="Azure alkalmazás szolgáltatási környezetben, virtuális hálózati, biztonságos hálózatok"
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

# <a name="app-service-environment-documentation"></a>Alkalmazás szolgáltatás környezet dokumentációja

Az alkalmazás-szolgáltatási környezetben egy [prémium] [ PremiumTier] Azure alkalmazás szolgáltatása, amely egy teljesen elszigetelt és dedikált környezet biztonságosan futtatásáról Azure alkalmazás szolgáltatás alkalmazások magas skála, beleértve a [Web Apps]csomag lehetőségét szolgáltatás[WebApps], [Mobile-alkalmazások][MobileApps], és az [API-alkalmazások][APIApps].  

Alkalmazás környezetek ideálisak az alkalmazás munkaterhelésekből megkövetelése:

- Nagyon magas skála
- Elkülönítési és biztonságos hálózati hozzáférés

Ügyfelek hozhat létre több alkalmazás szolgáltatási környezetben egyetlen Azure terület belül, illetve több Azure területek között.  Ideális vízszintesen méretezés az állapot-kevésbé alkalmazás rétegek támogató magas RPS munkaterhelésekből így alkalmazás szolgáltatási környezetben.

Alkalmazás környezetek elkülönítik csak egyetlen ügyfél-alkalmazások, és mindig telepítik egy virtuális hálózatba.  Ügyfelek mindkét bejövő és kimenő alkalmazás hálózati forgalmának engedélyezésére [hálózati biztonsági csoportok]használata külön hozzáférésük van[NetworkSecurityGroups].  Alkalmazások is hozhat létre a nagy sebességű biztonságos kapcsolat a helyszíni vállalati erőforrásokra mutató virtuális hálózatokon.

Vállalati erőforrások, például a belső adatbázisok eléréséhez, és a szolgáltatások webes gyakran kell alkalmazások.  Futó alkalmazás környezetek alkalmazások hozzáférhetnek a [Webhely-webhelyen] keresztül érhető el erőforrások[ SiteToSite] VPN és [a készült Azure ExpressRoute] [ ExpressRoute] kapcsolatok.

* [Mi az, hogy az alkalmazás-szolgáltatási környezetben?](../app-service-web/app-service-app-service-environment-intro.md)
* [Az alkalmazás-szolgáltatási környezetben létrehozása](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Az alkalmazás-szolgáltatási környezetben alkalmazások létrehozása](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Létrehozásával és használatával egy belső terheléselosztó alkalmazás szolgáltatás környezetekhez](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Alkalmazás szolgáltatás környezet beállítása](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Az alkalmazás-szolgáltatási környezetben méretezési alkalmazások](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [A hálózati biztonság és architektúra](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Hogyan meg

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Videók
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
