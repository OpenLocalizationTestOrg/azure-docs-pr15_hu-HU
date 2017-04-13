<properties 
    pageTitle="Az alkalmazás-szolgáltatási környezetben webes alkalmazás tűzfal (WAF) konfigurálása" 
    description="Megtudhatja, hogy miként konfigurálása webes alkalmazás tűzfalat elé az alkalmazás-szolgáltatási környezetben." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben webes alkalmazás tűzfal (WAF) konfigurálása

## <a name="overview"></a>– Áttekintés ##
A webes alkalmazás tűzfalak, például a [Microsoft Azure piactéren](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) biztonságos a webalkalmazások megvizsgálva bejövő internetes forgalmat SQL-utasítások, Webhelyközi parancsfájlok, kártevőt feltöltések és alkalmazás DDoS és más támadások blokkolása lehetővé teszi a [Barracuda WAF az Azure](https://www.barracuda.com/programs/azure) . Azt is megvizsgálja a válaszokat a háttéradatbázist web-kiszolgálókról az adatok elvesztése megelőzési (DLP-). Együtt a elkülönítési és a környezetek alkalmazás által biztosított további méretezést, ez egy ideális környezetet biztosít host üzleti kritikus webalkalmazások rosszindulatú kérelmek és a nagy forgalom állnia van szükség.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>A telepítő ##
Ehhez a dokumentumhoz, hogy fog konfigurálni az alkalmazás-szolgáltatási környezetben mögött több betöltése, hogy csak a WAF érkező forgalmat tudják elérni az alkalmazás-szolgáltatási környezetben, és nem lesz elérhető a DMZ a meghatározni Barracuda WAF példányát. A Barracuda WAF példányokat terheléselosztó Azure adatközpontokban és régiók elé azt is Azure forgalom Manager. A telepítő magas szintű folyamatábrája alább látható módon lenne.

![Architektúra][Architecture] 

> Megjegyzés: [ILB támogatja az alkalmazás-szolgáltatási környezetben](app-service-environment-with-internal-load-balancer.md)bevezetésével, állítsa be a ASE a DMZ a érhető el, és csak akkor érhető el a személyes hálózathoz. 

## <a name="configuring-your-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben konfigurálása ##
Konfigurálása az alkalmazás-szolgáltatási környezetben olvassa el [a dokumentáció](app-service-web-how-to-create-an-app-service-environment.md) a témában. Ha már létrehozott alkalmazás szolgáltatási környezetben, ebben a környezetben, amely az összes védi az azt konfigurálja a következő szakaszban WAF mögött [Web Apps](app-service-web-overview.md), az [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md) és a [Mobile-alkalmazások](../app-service-mobile/app-service-mobile-value-prop.md) hozhat létre.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>A Barracuda WAF felhőalapú szolgáltatás konfigurálása ##
Barracuda tartalmaz egy [részletes cikk](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) üzembe helyezése a WAF az Azure virtuális gépen. De mivel redundancia szeretné azt, és nem vezet be a hiba egyetlen pontot, üzembe legalább 2 WAF példány VMs be ugyanazt a felhőalapú szolgáltatást, ha kövesse az alábbi utasításokat.

### <a name="adding-endpoints-to-cloud-service"></a>Végpontok cloud szolgáltatás hozzáadása ###
Ha a felhőszolgáltatásában már 2 vagy több WAF virtuális példányok az [Azure portál](https://portal.azure.com/) hozzáadása a http- és HTTPS, az alábbi képen látható módon az alkalmazás által használt végpontokat is használhatja.

![Végpont beállítása][ConfigureEndpoint]

Ha az alkalmazások más végpontok, feltétlenül-e a listán azok hozzáadása. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Barracuda WAF keresztül az adatkezelési portál beállítása ###
Barracuda WAF TCP Port 8000 konfigurációs keresztül az adatkezelési portálon használja. Mivel a WAF VMs több példányban van szüksége lesz a virtuális példányai ismételje meg az alábbi lépésekkel. 


> Megjegyzés: Ha WAF beállításokkal végzett, távolítsa el a TCP/8000 végpont összes a WAF VMs a WAF biztonsága.

Adja hozzá az adatkezelési végpontot, a Barracuda WAF konfigurálásához az alábbi képen látható módon.

![Adatkezelési végpont hozzáadása][AddManagementEndpoint]
 
Tallózással keresse meg a felhőalapú szolgáltatást a felügyeleti végponton használjon másik böngészőt. Ha a felhőalapú szolgáltatás neve test.cloudapp.net, volna érheti el a végpont http://test.cloudapp.net:8000 kiválasztásával. Meg kell jelennie egy bejelentkezési lapjára, mint alább is bejelentkezési a WAF virtuális beállítása szakaszban megadott hitelesítő adataival.

![Adatkezelési bejelentkezési lapja][ManagementLoginPage]

Egyszer, a bejelentkezési meg kell jelennie egy irányítópult, az alábbi képen a azt, amely bemutatja a WAF védelmet egyszerű statisztikájának.

![Adatkezelési irányítópult][ManagementDashboard]

Kattintás a szolgáltatások lapján adhatja meg a szolgáltatások védi az WAF. A Barracuda WAF beállításáról további részletekért olvassa el [a dokumentáció](https://techlib.barracuda.com/waf/getstarted1). Az Azure-Webappokban az alábbi példában a forgalmat a http- és HTTPS szolgáló lett beállítva.

![Adatkezelési szolgáltatások hozzáadása][ManagementAddServices]

> Megjegyzés: Attól függően, hogy az alkalmazások beállításaitól, és milyen szolgáltatások használata érdekli, az alkalmazás-szolgáltatási környezetben, meg kell továbbítása forgalmat a TCP portokon kívül 80 és 443, például egy webalkalmazás IP SSL beállítása esetén. Alkalmazás szolgáltatási környezetben használt hálózati portok listáját olvassa el a [vezérlő bejövő forgalom dokumentáció](app-service-app-service-environment-control-inbound-traffic.md) hálózati portok rész.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurálása a Microsoft Azure forgalom Manager (nem kötelező) ##
Ha az alkalmazás elérhető több régióban, majd történő betölteni kívánt egyenleg őket [Azure forgalom Manager](../traffic-manager/traffic-manager-overview.md)mögött. Ehhez tegye zárólap is hozzáadhat az [Azure klasszikus portál](https://manage.azure.com) használata a Felhőbeli szolgáltatástól nevét a WAF forgalom Manager profilban, az alábbi képen látható módon. 

![Adatforgalom Manager végpont][TrafficManagerEndpoint]

Ha az alkalmazás-hitelesítést igényel, győződjön meg róla, van néhány erőforrás nem minden hitelesítést igénylő a forgalom-kezelő ping csak az alkalmazás elérhetőségét. Az URL-cím beállítása csoportjában adhatja meg az [Azure klasszikus portál](https://manage.azure.com) , alább látható módon.

![Kezelő forgalom konfigurálása][ConfigureTrafficManager]

A forgalom Manager pingelése az alkalmazás a WAF továbbít, szüksége beállítása webhely fordítások a Barracuda WAF a forgalom továbbítani szeretné az alkalmazást, az alábbi példában látható módon.

![Webhely fordítások][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Biztonságossá tétele a forgalmat a hálózati biztonsági csoportok (NSG) alkalmazás-szolgáltatási környezetben##
Kövesse a [vezérlő bejövő forgalom dokumentáció](app-service-app-service-environment-control-inbound-traffic.md) részletekért forgalom az alkalmazás szolgáltatás környezetbe származó korlátozása a WAF csak a felhőalapú szolgáltatás virtuális címének használatával. Íme egy példa a feladat elvégzéséhez a TCP 80-as port Powershell-parancsot.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

A SourceAddressPrefix cserélje le a WAF Felhőszolgáltatásba virtuális IP-címe (virtuális).

> Megjegyzés: A virtuális a felhőalapú szolgáltatás változik, törlése és hozza létre újból a felhőalapú szolgáltatást. Ellenőrizze, hogy a hálózati erőforráscsoport az IP-cím frissítése, miután ezt megtette. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
