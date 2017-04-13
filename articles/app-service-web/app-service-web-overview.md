<properties
    pageTitle="A Web Apps alkalmazások áttekintése |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure alkalmazás szolgáltatás fejlesztése, illetve segít host webalkalmazások"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Web Apps alkalmazások áttekintése

*Alkalmazás szolgáltatás Web Apps alkalmazások* egy teljes körű felügyelt számítási platform, amely a webhelyek és webalkalmazások tárolására van optimalizálva. A [platform-mint-a-szolgáltatás](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) ajánlja fel, a Microsoft Azure lehetővé koncentrálhat az üzleti logikai funkcióinak közben Azure gondoskodik az infrastruktúra futtatásához és az alkalmazások méretezni.

Az alábbi 5 perces videó Azure alkalmazás szolgáltatás Web Apps alkalmazások vezet be.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Mi az, hogy az alkalmazás szolgáltatás webalkalmazást?

Alkalmazás szolgáltatásban a *web App alkalmazásban* a számítási erőforrások, amelybe a webhelyén vagy a webes alkalmazás szolgáltatója Azure.  

Lehet, hogy a számítási erőforrások megosztott vagy dedikált virtuális gépeken (VMs), attól függően, hogy a választott árak réteg. Az alkalmazás kódja fut egy felügyelt virtuális, amelyek elkülönítik más felhasználóktól.

A kód bármelyik nyelvi vagy [Azure alkalmazás szolgáltatás](../app-service/app-service-value-prop-what-is.md), például ASP.NET, Node.js, Java, PHP vagy Python által támogatott keretrendszer lehet. Parancsfájlok [PowerShell és más parancsfájlok nyelvek](web-sites-create-web-jobs.md#acceptablefiles) webalkalmazást használó is futtathatók.

Példák, amelyek segítségével használhatja a Web Apps tipikus alkalmazás forgatókönyvek című témakörben talál [Web app esetek](https://azure.microsoft.com/documentation/scenarios/web-app/) és [Azure alkalmazás szolgáltatás, a virtuális gépeken futó, szolgáltatás, és Cloud Services összehasonlító](choose-web-site-cloud-service-vm.md#scenarios) **forgatókönyveket és javaslatok** szakaszát.

## <a name="why-use-web-apps"></a>Miért érdemes használni a Web Apps alkalmazások?

Az alábbiakban a kulcsfontosságú funkciókkal alkalmazás szolgáltatás alkalmazása a Web Apps alkalmazások:

- **Több nyelvet és keretek** - szolgáltatási alkalmazás rendelkezik ASP.NET, Node.js, Java, PHP és Python osztályú támogatása. A alkalmazás szolgáltatás VMs is [PowerShell és más parancsfájlokat vagy végrehajtható](../app-service-web/web-sites-create-web-jobs.md) futtathatja.

- **DevOps optimalizálás** - állítsa be a [folyamatos integráció és üzembe](../app-service-web/app-service-continuous-deployment.md) Visual Studio Team Services, GitHub vagy BitBucket. Frissítések [vizsgálat](../app-service-web/web-sites-staged-publishing.md)és a környezetek átmeneti előléptetése Hajtsa végre [A és B vizsgálat](../app-service-web/app-service-web-test-in-production-get-start.md). [Azure PowerShell](../powershell-install-configure.md) alrendszerrel vagy a [platformok parancssori kezelőfelületről](../xplat-cli-install.md)kezelheti az alkalmazások App szolgáltatásban.

- **Globális méretezni a magas rendelkezésre** - skála [be](../app-service-web/web-sites-scale.md) vagy [ki](../monitoring-and-diagnostics/insights-how-to-scale.md) automatikusan vagy manuálisan. Szolgáltató a a Microsoft globális adatközponthoz infrastruktúra bárhol az alkalmazásokat, és az alkalmazás szolgáltatás [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) magas elérhetősége ígéretet tesz.

- A **szoftver platformokon és a helyszíni adatok** - az 50-nél több [összekötők](../connectors/apis-list.md) vállalati rendszerekhez (például az SAP, a Siebel és Oracle), válassza a szoftver szolgáltatások (például a Salesforce és az Office 365), és az internetes szolgáltatások (például Facebook és Twitter). Az Access a helyszíni [Hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) és [Azure virtuális hálózatok](../app-service-web/web-sites-integrate-with-vnet.md)az adatoknak.

- **Biztonság és megfelelőség** - szolgáltatási alkalmazást, az [ISO, a SOC, és a PCI kompatibilis](https://www.microsoft.com/TrustCenter/).

- **Alkalmazássablonok** - alkalmazás sablonok a [Microsoft Azure piactéren](https://azure.microsoft.com/marketplace/) , amelyekkel varázsló segítségével például WordPress, Joomla és Drupal népszerű Megnyitás-forrás szoftverek telepítése, egy teljes körű listából választhatja ki.

- **Integráció a visual Studio** - Visual Studio dedikált eszközeinek egyszerűsítheti létrehozása, telepítés és hibakeresés munkáját.

Ezeken kívül webalkalmazást is kihasználhatja (például CORS támogatja), az [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md) által kínált szolgáltatások és a [Mobile-alkalmazások](../app-service-mobile/app-service-mobile-value-prop.md) (például a leküldéses értesítések). Többet szeretne tudni az alkalmazás típusú alkalmazás szolgáltatásban a [Azure alkalmazás szolgáltatás áttekintése](../app-service/app-service-value-prop-what-is.md)című témakörben találhat.

Web Apps alkalmazások alkalmazás szolgáltatás mellett Azure felajánlja a más szolgáltatásokhoz, webhelyek és webalkalmazások elhelyezésére használható. Ha a legtöbb esetben a Web Apps alkalmazások a legjobb választás.  Microservice architektúrája fontolja meg a [Szolgáltatás háló](https://azure.microsoft.com/documentation/services/service-fabric), és ha további beállítási lehetőségekre a VMs, a kód futó van szüksége, vegye figyelembe az [Azure virtuális gépeken futó](https://azure.microsoft.com/documentation/services/virtual-machines/). Között az alábbi Azure szolgáltatások válassza a további tudnivalókért lásd: [Azure alkalmazás szolgáltatás, virtuális gépeken futó, szolgáltatás, és Cloud Services összehasonlító](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Első lépések

Kezdésként minta programkódot telepít egy új web App alkalmazás szolgáltatásban, hajtsa végre a [központi telepítés 5 percig tart az Azure az első web app](app-service-web-get-started.md) oktatóprogram. Egy ingyenes Azure-fiókkal kell.

Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.
