<properties
    pageTitle="Azure alkalmazás szolgáltatás webes, mobile és az API-alkalmazások |} Microsoft Azure"
    description="Megtudhatja, miként Azure alkalmazás szolgáltatás segít fejlesztése, üzembe helyezéséhez és kezelheti a webhely és a mobile-alkalmazások."
    keywords="alkalmazás szolgáltatás, azure alkalmazás szolgáltatás, alkalmazás szolgáltatás költség, méretarány méretezhető, alkalmazások telepítésének, azure alkalmazások telepítésének, paas, platform-mint-a-szolgáltatás, webhely, webhely, webhelyén, a mobil azure"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Mi az Azure alkalmazás szolgáltatást?

*Alkalmazás* szolgáltatása [platform-mint-a-szolgáltatás](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) a Microsoft Azure kínáló. Webes és bármely platform vagy eszköz mobilalkalmazások hozhat létre. Az alkalmazások integrálása szoftver megoldások kapcsolatba lépni a helyszíni környezetbe applications és az üzleti folyamatok automatizálása. Azure-alkalmazások fut, a teljes körű felügyelt virtuális gépeken futó (VMs), a választott megosztott virtuális erőforrások vagy dedikált VMs.

Alkalmazás szolgáltatás a webhely és a mobil funkciók, amelyek a azt korábban külön-külön Azure webhelyekre és Azure Mobile Services tartalmazza. Üzleti folyamatok automatizálása és a felhő API-khoz szolgáltatója új funkciókat is tartalmaz. Egyetlen integrált szolgáltatásként alkalmazás szolgáltatás lehetővé teszi különféle összetevőket – webhelyeket, mobilalkalmazás vissza végű, RESTful API-k és üzleti folyamatok--írja egységes megoldássá.

Az alábbi 4 perces videó röviden hogyan alkalmazás szolgáltatásra vonatkozik-e korábbi Azure szeretne rendelni, és azt a újdonságai biztosít.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Miért érdemes használni az alkalmazás szolgáltatást?

Íme, néhány fontosabb funkciói és szolgáltatási alkalmazás lehetőségeit:

- **Több nyelvet és keretek** - szolgáltatási alkalmazás rendelkezik ASP.NET, Node.js, Java, PHP és Python osztályú támogatása. A alkalmazás Service VMs is [a Windows PowerShell és más parancsfájlokat vagy végrehajtható](../app-service-web/web-sites-create-web-jobs.md) futtathatja.

- **DevOps optimalizálás** - állítsa be a [folyamatos integráció és üzembe](../app-service-web/app-service-continuous-deployment.md) Visual Studio Team Services, GitHub vagy BitBucket. Frissítések [vizsgálat](../app-service-web/web-sites-staged-publishing.md)és a környezetek átmeneti előléptetése Hajtsa végre [A és B vizsgálat](../app-service-web/app-service-web-test-in-production-get-start.md). [Azure PowerShell](../powershell-install-configure.md) alrendszerrel vagy a [platformok parancssori kezelőfelületről](../xplat-cli-install.md)kezelheti az alkalmazások App szolgáltatásban.

- **Globális méretezni a magas rendelkezésre** - skála [be](../app-service-web/web-sites-scale.md) vagy [ki](../monitoring-and-diagnostics/insights-how-to-scale.md) automatikusan vagy manuálisan. Szolgáltató a a Microsoft globális adatközponthoz infrastruktúra bárhol az alkalmazásokat, és az alkalmazás szolgáltatás [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) magas elérhetősége ígéretet tesz.

- A **szoftver platformokon és a helyszíni adatok** - az 50-nél több [összekötők](../connectors/apis-list.md) vállalati rendszerekhez (például az SAP, a Siebel és Oracle), válassza a szoftver szolgáltatások (például a Salesforce és az Office 365), és az internetes szolgáltatások (például Facebook és Twitter). Az Access a helyszíni [Hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) és [Azure virtuális hálózatok](../app-service-web/web-sites-integrate-with-vnet.md)az adatoknak.

- **Biztonság és megfelelőség** - szolgáltatási alkalmazást, az [ISO, a SOC, és a PCI kompatibilis](https://www.microsoft.com/TrustCenter/).

- **Alkalmazássablonok** - alkalmazás sablonok a [Microsoft Azure piactéren](https://azure.microsoft.com/marketplace/) , amelyekkel varázsló segítségével például WordPress, Joomla és Drupal népszerű Megnyitás-forrás szoftverek telepítése, egy teljes körű listából választhatja ki.

- **Integráció a visual Studio** - Visual Studio dedikált eszközeinek egyszerűsítheti létrehozása, telepítés és hibakeresés munkáját.

## <a name="app-types-in-app-service"></a>Alkalmazás típusú alkalmazás szolgáltatásban

Alkalmazás-szolgáltatás *típusú alkalmazás*, amelyek egy adott típusú terhelést tárolni íródott kínál:

- [**Web Apps**](../app-service-web/app-service-web-overview.md) - webhelyekre és webalkalmazások szolgáltatója.

- [**Mobilalkalmazások**](../app-service-mobile/app-service-mobile-value-prop.md) Mobilalkalmazás elhelyezésére biztonsági véget ér.

- [**Alkalmazások API**](../app-service-api/app-service-api-apps-why-best-platform.md) - szolgáltatója RESTful API-khoz.

- [**Logika alkalmazások**](../app-service-logic/app-service-logic-what-are-logic-apps.md) – az üzleti folyamatok automatizálása és rendszerek és az adatok integrálása keresztül felhőket kódírás nélkül.

A word *alkalmazás* Itt a kitűzött célja, futó terhelési üzemeltetési erőforrások hivatkozik. Véve "webalkalmazás" szerepel példaként, megszokta, valószínűleg a számítási erőforrások és a közös funkciók kézbesítése böngészőben alkalmazás kódját webalkalmazást "témájú szeretne. De az alkalmazás szolgáltatás egy *webalkalmazás* a számítási erőforrások, amelybe a alkalmazás kód szolgáltatója Azure. Ha az alkalmazás áll az weblapon előtér- és RESTful API készítsen biztonsági vége, mindkettőt webalkalmazást sikerült rendszerbe vagy webalkalmazást, és az API-alkalmazásokban a háttéradatbázist kód sikerült telepít az előtér-kódot. Az alkalmazás különböző típusú több alkalmazás szolgáltatás alkalmazások állhat.

## <a name="app-service-plans"></a>App milyen szolgáltatáscsomagok

Az alkalmazások futó számítási erőforrások típusú [Alkalmazás szolgáltatás tervek](azure-web-sites-web-hosting-plans-in-depth-overview.md) adja meg. Ha várhatóan egyszerűsített forgalmi terhelés, megosztott VMs (**ingyenes** , **megosztott** rétegek árak) is használhatja. Az újabb terhelések választhat számos méretű dedikált VMs (rétegek**egyszerű**, **normál**és **Premium** ). Több alkalmazás szolgáltatás alkalmazások megoszthatja ugyanarra a csomagra, és azok méretezni a árak rétegek mezőjében a közös a csomagban.

Ha további méretezhetőség és hálózati elkülönítési van szüksége, az alkalmazások [Alkalmazás szolgáltatási környezetben](../app-service-web/app-service-app-service-environment-intro.md)futtathatja.

## <a name="pricing"></a>Árak

Mennyi alkalmazás szolgáltatás költségekkel kapcsolatos további tudnivalókért olvassa el a [Alkalmazás szolgáltatás árak](https://azure.microsoft.com/pricing/details/app-service/)című témakört.

## <a name="test-drive-app-service"></a>Alkalmazás szolgáltatás kipróbálására

[Egy minta webalkalmazás létrehozása, mobilalkalmazással logika alkalmazás](http://go.microsoft.com/fwlink/?LinkId=523751) és a lejátszás vele az 1 óra értéket, a nincs hitelkártya szükséges, nem nyilatkozatát, nincs bajlódnia.

Nyissa meg a [szabad Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)és az első lépések oktatóanyagok próbálja meg:

* [Oktatóprogram: Hozzon létre egy web App alkalmazásban](../app-service-web/app-service-web-get-started.md)
* [Oktatóprogram: Létrehozása mobilalkalmazásban](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Oktatóprogram: Az API-alkalmazás létrehozása](../app-service-api/app-service-api-dotnet-get-started.md)
* [Oktatóprogram: Logika alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)
