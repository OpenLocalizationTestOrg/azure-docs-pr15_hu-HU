<properties
   pageTitle="Erőforrás-kezelő támogatott szolgáltatások |} Microsoft Azure"
   description="Az erőforrás-szolgáltatók, amelyek támogatják az erőforrás-kezelő, hogy a sémák és rendelkezésre álló API-verziók és üzemeltetheti az erőforrások régiók ismerteti."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Erőforrás-kezelő szolgáltatók régiók, API-verziók sémák és

Azure erőforrás-kezelő új megoldást szeretné üzembe helyezéséhez és kezeléséhez, a szolgáltatások, az alkalmazások alkotó. A legtöbb, de nem az összes services támogatja az erőforrás-kezelő, és néhány szolgáltatás csak részben támogatja az erőforrás-kezelő. Ez a témakör a támogatott erőforrás-szolgáltatót az Azure erőforrás parancsra.

Az erőforrások telepítésekor is kell tudnia azokat a területeket támogatják ezeket az erőforrásokat, és mely API-verziók az erőforrások érhetők el. A [támogatott régiók](#supported-regions) szakasz bemutatja, hogyan megtudhatja, hogy melyik régiók munka az előfizetést és a források. A szakasz [támogatott API-verziók](#supported-api-versions) megtudhatja, miként állapítható meg, mely API-verziók is használhatja.

Mely szolgáltatásokat támogatja a az Azure-portál és -klasszikus portál talál további [Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/). Mely szolgáltatásokat támogatja a mozgó erőforrások talál további [erőforrások új erőforráscsoport vagy-előfizetésre áthelyezése](resource-group-move-resources.md).

Az alábbi táblázatokban felsoroljuk, mely Microsoft szolgáltatások támogatása telepítési és erőforrás-kezelő és a nem kezelése. Az a oszlopban **Quickstart útmutató sablonok** hivatkozásra az Azure quickstart útmutató sablonok tárházba lekérdezés küld az adott erőforrás-szolgáltatóhoz. Quickstart útmutató sablonok hozzáadott és gyakran frissülnek. Hivatkozás egy adott szolgáltatás jelenlétét nem feltétlenül jelenti a lekérdezés a tárházba sablonok adja vissza. Erőforrás-kezelő támogató számos külső erőforrás-szolgáltatók is vannak. Megismerheti, hogy hogyan tekintheti meg a [erőforrás szolgáltatók és típus](#resource-providers-and-types) csoport erőforrás-szolgáltatók.


## <a name="compute"></a>Számítási

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------------------------ |-------- | ------ | ------ |
| Köteg   | igen     | [A köteg többi](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [2015-12-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|A tároló | igen | [Tároló szolgáltatás többi](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016 – 03 – 30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics életciklus-szolgáltatások | igen  |      |        |  |
| Méretezés beállítása | igen | [Skála megadása a többi](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Szolgáltatás háló | igen  | [Szolgáltatás háló többi](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Virtuális gépeken futó | igen | [VIRTUÁLIS TÖBBI](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Virtuális gépeken futó (klasszikus) | Korlátozva van | - | - | - |
| Távoli alkalmazás | nem      | -  | - | - |
| Cloud Services (klasszikus) | Korlátozva van (lásd alább) | - | - | - |

Virtuális gépeken futó (klasszikus) erőforrások bevezetett keresztül a klasszikus telepítési modell helyett az erőforrás-kezelő telepítési modell keresztül hivatkozik. Általánosságban elmondható ezek az erőforrások nem támogatja az erőforrás-kezelő műveletek, de vannak engedélyezve van, bizonyos műveletek. Az alábbi telepítési modellek kapcsolatos további tudnivalókért lásd: [erőforrás-kezelő Pivotbeli üzembe és a klasszikus telepítési](resource-manager-deployment-model.md). 

Cloud Services (klasszikus) is használható más klasszikus erőforrásokkal; azonban klasszikus erőforrások nem kihasználhatja az összes erőforrás-kezelő szolgáltatás, nem jó jövőbeli megoldások lehetőséget. Ehelyett megfontolni a alkalmazás infrastruktúra Microsoft.Compute Microsoft.Storage és Microsoft.Network névtér erőforrásokat.


## <a name="networking"></a>Hálózati

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | -------  | -------- | ------ | ------ |
| Alkalmazás átjáró | igen | [Alkalmazás átjáró többi](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS     | igen | [DNS-TÖBBI](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| Készült ExpressRoute | igen  | [Készült ExpressRoute többi](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Terheléselosztó | igen  | [Terheléselosztó többi](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Adatforgalom Manager | igen | [Adatforgalom Manager többi](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [2015-11-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtuális hálózatok | igen| [Virtuális hálózat többi](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| Virtuális Magánhálózati átjáró | igen | [Hálózati átjáró többi](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[kapcsolatok](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Tárhely

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Tárhely | igen  | [További tárterület](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Tárterület-fiók](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | igen  |         |        |  |

## <a name="databases"></a>Adatbázisok

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | igen  | [DocumentDB többi](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Gyorsítótár vgx.dll | igen |   | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| SQL-adatbázis | igen | [SQL-adatbázis többi](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014-04-01 – előzetes verzió](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL-adatraktár | igen |   |      |


## <a name="web--mobile"></a>Webes és a Mobile

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------- | ------ | ------ |
| API-alkalmazások | igen |   | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [API-alkalmazások](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API-kezelés | igen  | [REST API-kezelés](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016 – 07-07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Tartalom moderátori | igen |   |   |   |
| Alkalmazás függvény | igen |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Logika alkalmazások | igen   | [Munkafolyamat szolgáltatás REST API-val](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobilalkalmazások | igen |  | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Mobil Előjegyzések | igen  |  [Mobil részvételét többi](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Keresés | igen  | [Keresési REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Web Apps alkalmazások | igen |          | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Elemző

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | -------  | -------- | ------ | ------ |
| Adatkatalógus | igen  | [Adatkatalógus többi](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016 – 03 – 30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Adatok gyári | igen | [Adatok gyári többi](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Adatok tó Analytics | igen |   |   [2015-10-01 – előzetes verzió](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Tó adattárhoz | igen  | [A többi tó adattárhoz](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015-10-01 – előzetes verzió](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | igen | [HDInsights többi](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Gépi tanulási | igen | [Gépi tanulási többi](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016 – 05-01 – előzetes verzió](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Értékáram-elemzés | igen  | [Gőzzel Analytics többi](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| A Power BI | igen | [A Power BI beágyazott többi](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016-01-29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Időintelligencia

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------- | ------ | ------ |
| Kognitív szolgáltatások | igen |  | [2016 – 02-01 – előzetes verzió](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Internetes dolgot

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------- | ------ | ------ |
| Esemény központi | igen  | [Esemény központi többi](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | igen | [IoT központi többi](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016 – 02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Értesítés hubok | igen | [Értesítés központi többi](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Média és CDN

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------- | ------ | ------ |
| CDN | igen | [CDN-TÖBBI](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Multimédia-szolgáltatás | igen | [Media Services többi](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [2015-10-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Hibrid-integráció

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------- | ------ | ------ |
| BizTalk szolgáltatások | igen |          | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Helyreállítási szolgáltatás | igen | [A webhely többi helyreállítás](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Szolgáltatás Bus | igen | [Szolgáltatás Bus többi](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Identitás és a hozzáférés kezelése 

Azure Active Directory az erőforrás-kezelő ahhoz, hogy az előfizetés szerepköralapú hozzáférés-vezérlés működik. Szerepköralapú hozzáférés-vezérlés és az Active Directory használatával kapcsolatos további tudnivalókért olvassa el az [Azure szerepköralapú hozzáférés-vezérlés](./active-directory/role-based-access-control-configure.md)című témakört.

## <a name="developer-services"></a>Fejlesztői szolgáltatások 

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------- | ------ | ------ |
| Alkalmazás Hírcsatornájában | igen  | [A háttérismeretek többi](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| A Bing Maps | igen  |          |        |  |
| DevTest Labs | igen |   | [2016 – 05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Visual Studio-fiók | igen   |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Kezelés és biztonság

| Szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------- | ------ | ------ |
| Automatizálási | igen | [Automatizálási többi](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10 – 31.](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Fő tárolóból elemre | igen | [Kulcs a többi tárolóból elemre](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Fő tárolóból elemre](resource-manager-template-keyvault.md)<br />[Titkos kulcs tárolóból elemre](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Műveleti Hírcsatornájában | igen |          |        |  |
| A Feladatütemező | igen  | [A Feladatütemező többi](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [2016-03-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Biztonsági (előzetes verzió) | igen | [Biztonsági többi](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Erőforrás-kezelő

| A szolgáltatás | Erőforrás-kezelő engedélyezve | REST API-VAL | Séma | Quickstart útmutató sablonok |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Engedély | igen | [Zárolásainak kezelése](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Szerepköralapú hozzáférés-vezérlés](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Erőforrás-zárolás](resource-manager-template-lock.md)<br />[Szerepkör-hozzárendelés](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Erőforrások | igen | [Csatolt erőforrások](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Erőforrás-hivatkozások](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Erőforrás-szolgáltatók és -típusok

Erőforrások telepítésekor gyakran kell lekérése az erőforrás-szolgáltatók és típusú információt. Ez az információ REST API-t, Azure PowerShell vagy Azure CLI használatával meghallgathatja.

Dolgozunk egy erőforrás-szolgáltatóval, adott erőforrás-szolgáltató rendelkeznie kell regisztrált a fiókjához. Alapértelmezés szerint a sok erőforrást szolgáltató automatikusan regisztrált; azonban előfordulhat manuálisan regisztrálhatja az egyes erőforrás szolgáltatók. Az alábbi példák bemutatják, hogyan regisztrációs állapotuk egy erőforrás-szolgáltatóval, és regisztráljon az erőforrás-szolgáltató, ha szükséges.

### <a name="portal"></a>Portál

Egyszerűen megtekintheti a támogatott források szolgáltatót, hajtsa végre a következő lépéseket:

1. Jelölje ki **az engedélyeket**.

    ![az engedélyek](./media/resource-manager-supported-services/my-permissions.png)

2. Jelölje ki az **erőforrás-szolgáltató állapota**

    ![erőforrás-szolgáltató állapota](./media/resource-manager-supported-services/resource-provider-status.png)

3. Előfizetéshez tartozó látni az erőforrás-szolgáltatók teljes listáját.

    ![erőforrás-szolgáltatók lista](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>REST API-VAL

Az összes rendelkezésre álló az erőforrás szolgáltatók, beleértve a típusok, a helyek, az API-verziók és a regisztrációs állapotát, használja a [lista összes erőforrás-szolgáltató](https://msdn.microsoft.com/library/azure/dn790524.aspx) műveletet. Ha egy erőforrás-szolgáltató regisztrálnia kell jelenik meg [egy erőforrás-szolgáltatóval előfizetés regisztrálni](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>A PowerShell

A következő példa bemutatja, hogyan kell felvenni az összes rendelkezésre álló erőforrás-szolgáltatónál.

    Get-AzureRmResourceProvider -ListAvailable
    

A következő példa bemutatja a erőforrástípus beszerzése egy adott erőforrás-szolgáltató.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
A kimenet hasonlít:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Regisztrálni egy erőforrás-szolgáltatót, adja meg a névtér:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure CLI

A következő példa bemutatja, hogyan kell felvenni az összes rendelkezésre álló erőforrás-szolgáltatónál.

    azure provider list
    
A kimenet hasonlít:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

Egy adott erőforrás-szolgáltató adatait egy fájlt, az alábbi paranccsal mentheti.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Regisztrálni egy erőforrás-szolgáltatót, adja meg a névtér:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>A támogatott régiók

Erőforrások telepítésekor általában kell adjon meg egy erőforrás régiót. Erőforrás-kezelő minden régióban támogatott, de nem támogatja az erőforrások rendszerbe előfordulhat, hogy minden régióban. Ezeken kívül lehetnek olyan előfizetését, megakadályozzák a használja, amely támogatja az erőforrás bizonyos területeire korlátozásait. Ezek a korlátozások előfordulhat, hogy kapcsolódik az otthoni ország problémák adó, vagy az eredmény házirend elhelyezett használata csak egyes régiókra előfizetés rendszergazdájától. 

Az összes támogatott régiók Azure szolgáltatások listáját olvassa el a [régió szerint szolgáltatások](https://azure.microsoft.com/regions/#services); című témakört. Ez a lista azonban a régiók, amely nem támogatja az előfizetés tartalmazhat. Meghatározhatja, hogy egy adott erőforrás típusa, amely támogatja az előfizetése keresztül a portálon, a REST API-t, a PowerShell vagy az Azure CLI régióit.

### <a name="portal"></a>Portál

Megjelenik a támogatott régiók egy erőforrástípus keresztül az alábbi lépéseket:

1. Jelölje ki a **További szolgáltatások** > **Erőforrás Explorer**.

    ![erőforrás explorer](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Nyissa meg a **szolgáltatók** csomópontot.

    ![Jelölje be a szolgáltató](./media/resource-manager-supported-services/select-providers.png)

3. Válassza ki az erőforrás-szolgáltatót, és tekintse meg a támogatott régiók és az API-verziók.

    ![nézet szolgáltató](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>REST API-VAL

Megtudhatja, hogy melyik régiók érhetők el az előfizetés az adott erőforrástípus, használja az a [lista összes erőforrás-szolgáltató](https://msdn.microsoft.com/library/azure/dn790524.aspx) műveletet. 

### <a name="powershell"></a>A PowerShell

A következő példa bemutatja, hogyan hozhatja ki a támogatott régiók webhelyekhez.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
A kimenet hasonlít:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure CLI

Az alábbi példa eredménye minden egyes erőforrástípus támogatott helyét.

    azure location list

A hely között, például [jq](https://stedolan.github.io/jq/)JSON segédprogram is szűrheti.

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Amely adja eredményül:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Támogatott API-verziók

Amikor rendszerbe állítják egy sablont, meg kell adnia egy API-verzió kezelje hozhat létre az egyes erőforrásokhoz. Az API-verzió megfelel az erőforrás-szolgáltató által kiadott műveletek REST API-verziót. Egy erőforrás-szolgáltató lehetővé teszi, hogy az új funkciók, mint a REST API-t egy új verziója által kiadott. Ezért a API, adja meg, ha a sablon verziójának érinti, mely tulajdonságokat adhatja meg a sablonban. Az általános szeretne sablonok létrehozásakor, jelölje ki a legutóbbi API-verziót. A meglévő sablonok eldöntheti, hogy továbbra is a API régebbi verzióját használja, vagy a sablon kihasználhatja az új funkciók a legújabb verzióra frissíteni.

### <a name="portal"></a>Portál

A támogatott API-verziók ugyanúgy megadta a támogatott régiók (korábban látható) határozza meg.

### <a name="rest-api"></a>REST API-VAL

Megtudhatja, hogy mely API-verziók fióktípus esetén elérhetők erőforrás, használja az a [lista összes erőforrás-szolgáltató](https://msdn.microsoft.com/library/azure/dn790524.aspx) műveletet. 

### <a name="powershell"></a>A PowerShell

A következő példa bemutatja a rendelkezésre álló API-verziók beszerzése egy adott erőforrás típusa.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
A kimenet hasonlít:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure CLI

A következő parancs a fájl egy erőforrás-szolgáltató az adatokat (beleértve a rendelkezésre álló verziók API-val) mentheti.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Nyissa meg a fájlt, és keresse meg a **apiVersions** elem

## <a name="next-steps"></a>Következő lépések

- Erőforrás-kezelő sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő létrehozáshoz használható sablonok](resource-group-authoring-templates.md).
- Erőforrások telepítésével kapcsolatos további tudnivalókért lásd: [Deploy egy alkalmazást, az erőforrás-kezelő Azure sablonnal](resource-group-template-deploy.md).
