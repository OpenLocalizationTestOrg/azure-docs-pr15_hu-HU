<properties
    pageTitle="Azure gépi tanulási webszolgáltatások: Telepítési és felhasználási |} Microsoft Azure"
    description="Források telepítése és használata a webes szolgáltatások más."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure gépi tanulási webszolgáltatások: Telepítési és felhasználási

Azure gépi tanulási segítségével üzembe gépi tanulási munkafolyamatok és az adatmodellek webszolgáltatásokhoz. Ezek a szolgáltatások webes használható az előrejelzések valós idejű vagy köteg módban végezze el az interneten keresztül hívja fel a gépi tanulási modellek alkalmazásból. Mivel a webes szolgáltatások RESTful, felhívhatja őket a különböző programozási nyelven és platformok, például a .NET és Java-ből és alkalmazásokhoz, például az Excel.

A következő szakaszokban forgatókönyvek, a kód és a dokumentáció való használatának megkezdéséhez.

## <a name="deploy-a-web-service"></a>Webszolgáltatás terjesztése

### <a name="with-azure-machine-learning-studio"></a>Azure gépi tanulási Studio

Gépi tanulási Studio és a Microsoft Azure gépi tanulási Web Services portál segítséget üzembe helyezéséhez és kezeléséhez webszolgáltatás kódírás nélkül.

Az alábbi hivatkozásokra új webes szolgáltatás üzembe kapcsolatos általános tudnivalókért:

* Áttekintést kaphat arról, hogy miként, amelyet a Azure erőforrás-kezelő alapuló új webes szolgáltatás üzembe lásd: a [Deploy egy új webszolgáltatásból](machine-learning-webservice-deploy-a-web-service.md).
* Útmutató webszolgáltatás telepítéséről olvassa el a [Deploy az Azure gépi tanulási webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md)című témakört.
* Arról, hogy miként hozhat létre és üzembe webszolgáltatás teljes útmutató című [áttekintése a következő lépés 1: gépi tanulási munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md).
* Konkrét példákat, webes szolgáltatás telepítése című témakörökben talál:

    * [5 áttekintése a következő lépés: Az Azure gépi tanulási webszolgáltatás terjesztése](machine-learning-walkthrough-5-publish-web-service.md)
    * [Több területre webszolgáltatás telepítéséről](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Web services erőforrás szolgáltatónál API-khoz (Azure erőforrás-kezelő API-k)

Az Azure gépi tanulási erőforrás szolgáltató webszolgáltatásokhoz lehetővé teszi, hogy telepítési és webes szolgáltatások kezelése a REST API-hívások. További részletekért olvassa el a [Gépi tanulási Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) hivatkozást MSDN webhelyen.

### <a name="with-powershell-cmdlets"></a>A PowerShell-parancsmagok

Webszolgáltatásokhoz Azure gépi tanulási erőforrás szolgáltató lehetővé teszi, hogy telepítési és webes szolgáltatások kezelése a PowerShell-parancsmagokkal.

A parancsmagok használatához kell első alkalommal jelentkezik belül a PowerShell környezetet a Azure fiókjába a [Hozzáadás-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag használatával. Ha tudja pontosan, hogyan hívja fel a erőforrás-kezelő alapul PowerShell-parancsait, olvassa el a [Azure PowerShell használatá a Azure-kezelő eszközzel](../powershell-azure-resource-manager.md#login-to-your-azure-account)című témakört.

A cserélendő kísérlet exportálásához használja [ezt a minta kódot](https://github.com/ritwik20/AzureML-WebServices). Miután létrehozta az .exe fájl át a kódot, írhat:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Az alkalmazást futtató létrehoz egy webes szolgáltatás JSON sablont. A sablon használatával webszolgáltatás, fel kell vennie a következő adatokat:

* Fióknév tárhely és a billentyűk

    Amely letölthető a tárterület-fiók felhasználónevét és a kulcs az [Azure portál](https://portal.azure.com/) vagy az [Azure klasszikus portálon](http://manage.windowsazure.com/).
* Lekötési terv azonosítója

    Az [Azure gépi tanulási Web Services](https://services.azureml.net) portál bejelentkezés, és kattintson egy előfizetés nevétől elérheti a terv azonosítója.

Vegye fel őket a JSON-sablon a *Tulajdonságok* csomópont a *MachineLearningWorkspace* csomópont ugyanazon a szinten gyermekek.

Lássunk egy példát:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Az alábbi cikkek és a minta kód további részleteket lásd:

* [Azure gépi tanulási parancsmagok]( https://msdn.microsoft.com/library/azure/mt767952.aspx) hivatkozás az MSDN webhelyen
* Minta [Útmutató –](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) a GitHub

## <a name="consume-the-web-services"></a>A webes szolgáltatások felhasználása

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Az Azure gépi tanulási webes szolgáltatások felhasználói felület (vizsgálat)

A webszolgáltatás az Azure gépi tanulási Web Services portál tesztelheti. Ide tartoznak a teszteli a kérelem-válasz szolgáltatás (Erőforrásrekordok), és hogyan köteg végrehajtás szolgáltatás (BES).

* [Új webes szolgáltatás telepítése](machine-learning-webservice-deploy-a-web-service.md)
* [Az Azure gépi tanulási webszolgáltatás terjesztése](machine-learning-publish-a-machine-learning-web-service.md)
* [5 áttekintése a következő lépés: Az Azure gépi tanulási webszolgáltatás terjesztése](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Az Excel alkalmazásból

Az Excel-sablon, amely a webszolgáltatás fogyasztása töltheti le:

* [Az Excel web Azure gépi tanulási szolgáltatásainak használata más](machine-learning-consuming-from-excel.md)
* [Excel-bővítmény az Azure gépi tanulási webszolgáltatások](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>A többi-alapú ügyfélprogramból

Azure gépi tanulási webszolgáltatásokhoz RESTful API-khoz is. Ezek a különböző platformok, például .NET, Python, K, Java stb API-khoz mobilalkalmazásokban A webszolgáltatás a [Microsoft Azure gépi tanulási Web Services portál](https://services.azureml.net) **felhasználandó** lapjának minta kódot, amely segítséget nyújtanak az első lépések tartalmaz. További információért megtudhatja, [hogy miként felhasználása az Azure gépi tanulási webszolgáltatás, amely egy kísérlet tanulási számítógépről lett telepítve](machine-learning-consume-web-services.md).

