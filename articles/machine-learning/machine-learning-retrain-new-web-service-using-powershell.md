<properties
    pageTitle="Gépi tanulási kezelése a PowerShell-parancsmagok használata új webszolgáltatás Újraépítés |} Microsoft Azure"
    description="Megtudhatja, hogy miként programozás útján modell Újraépítés és frissíteni szeretné használni az újonnan képzett modell Azure gépi tanulási gépi tanulási kezelése a PowerShell-parancsmagok használata a webes szolgáltatást."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Gépi tanulási kezelése a PowerShell-parancsmagok használata új webszolgáltatás Újraépítés

Új webes szolgáltatás, Újraépítés, amikor frissíti a cserélendő web service definíció az új képzett modell hivatkozni szeretne.  

## <a name="prerequisites"></a>Előfeltételek

Akkor be kell állítania egy oktatás kísérlet és a prediktív kísérlet [Újraépítés gépi tanulási modellek programozás útján](machine-learning-retrain-models-programmatically.md)látható módon. 

>[AZURE.IMPORTANT] A cserélendő kísérlet kell telepíthető, mint az Azure erőforrás-kezelő (új) alapú gépi tanulási webszolgáltatás. 
 
További tájékoztatást az üzembe helyezés webszolgáltatásokhoz olvassa el a [Deploy az Azure gépi tanulási webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md)című témakört.

Ez a folyamat igényel, hogy telepítette az Azure gépi tanulási parancsmag. A gépi tanulási parancsmagok telepíti, az [Azure gépi tanulási parancsmagok](https://msdn.microsoft.com/library/azure/mt767952.aspx) hivatkozás a tartalmaz az MSDN webhelyen.

Az alábbi információk átmásolja a átképzési kimenet:

* BaseLocation
* RelativeLocation

A lépéseket, akkor tegye a következők:

1.  Jelentkezzen be az erőforrás-kezelő Azure-fiókjába.
2.  A webes szolgáltatás definíció beszerzése
3.  A Web Service Definition JSON exportálása
4.  Frissítse a ilearner blob a JSON a hivatkozás.
5.  A JSON importálása a webes definiált szolgáltatás
6.  Frissítse a webszolgáltatás új webes szolgáltatás meghatározása

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Jelentkezzen be az erőforrás-kezelő Azure-fiókjába

Kell első alkalommal jelentkezik a PowerShell környezetet, a [Hozzáadás-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmaggal belül a Azure fiókjába.

## <a name="get-the-web-service-definition"></a>A Web Service Definition beszerzése

Ezután beszerzése a webszolgáltatás hívja fel a [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag. A Web Service Definition a webszolgáltatás képzett mintája egy belső ábrázolása, ezért nem közvetlenül módosítható. Győződjön meg arról, hogy a Web Service Definition keres a cserélendő kísérlet és nem a oktatás kísérlet.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Annak megállapításához, az erőforrás-csoport nevét egy meglévő webszolgáltatás, futtassa a Get-AzureRmMlWebService parancsmagot a webes szolgáltatások jelennek meg az előfizetés paraméter nélkül. Keresse meg a webes szolgáltatás, és ott keresse meg a saját webhely szolgáltatás azonosítójával. Az erőforráscsoport neve a negyedik elem azonosítója, a csak a *resourceGroups* elem után. A következő példában az erőforrás-csoport nevét az alapértelmezett-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Azt is megteheti annak megállapításához, az erőforrás-csoport nevét egy meglévő webszolgáltatás, jelentkezzen be a Microsoft Azure gépi tanulási Web Services portál. Jelölje ki a webszolgáltatás. Az erőforrás-csoport nevét az URL-cím a webszolgáltatás az ötödik elem a csak a *resourceGroups* elem után. A következő példában az erőforrás-csoport nevét az alapértelmezett-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>A Web Service Definition JSON exportálása

A definíció, a képzett modellt az újonnan képzett módosításának modell, akkor először segítségével [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) parancsmag JSON formátumú fájlba exportálhatja.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Frissítse a ilearner blob a JSON a hivatkozás.

Az eszközök keresse meg a [képzett modell], frissítse a URI a ilearner blob- *uri* értéke a *locationInfo* csomópontot. A URI hozza létre a *BaseLocation* és a *RelativeLocation* , a hívás átképzés BES eredményéből.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>A JSON importálása a webes definiált szolgáltatás

A módosított JSON-fájl konvertálása webes definiált szolgáltatás frissítése a Predicative kísérlet használható az [Importálás-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) parancsmag kell használnia.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Frissítse a webszolgáltatás új webes szolgáltatás meghatározása

Végezetül segítségével [Frissítés-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) parancsmag frissíti a cserélendő kísérlet.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Összefoglalás

A gépi tanulási PowerShell-kezelő parancsmagok használ, például forgatókönyvek engedélyezése prediktív webszolgáltatás képzett mintája frissítése:

* Az új adatok átképzés periodikus modell.
* A cél, hogy őket a saját adatok használata modell Újraépítés eloszlást vevőknek modell.
