<properties
    pageTitle="Egy meglévő prediktív webszolgáltatás Újraépítés |} Microsoft Azure"
    description="Megtudhatja, hogy miként Újraépítés a modellt, és frissítse a webszolgáltatás szeretné használni az újonnan képzett modell Azure gépi tanulási."
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
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Egy meglévő prediktív webszolgáltatás Újraépítés

Ez a témakör átképzési folyamata a következő esetben:

* Ha egy oktatás kísérlet és a prediktív kísérlet, hogy telepítette az operationalized webszolgáltatás.
* Új adatok van, amelyet a pontozási végrehajtásához használja a cserélendő webes szolgáltatás.

A meglévő webes szolgáltatás és a kísérletek kezdve, akkor kell kövesse az alábbi lépéseket:

1. Frissíti a modellt.
    1. Módosítsa a oktatás kísérlet lehetővé teszik webhely szolgáltatás ráfordítások és a kimeneti értékeket.
    2. A tanfolyam kísérlet üzembe átképzési webes szolgáltatásként.
    3. A tanfolyam kísérlet köteg végrehajtás szolgáltatás (BES) segítségével Újraépítés a modellt.
2. Azure gépi tanulási PowerShell-parancsmagok használata a cserélendő kísérlet frissítéséhez.
    1.  Jelentkezzen be az erőforrás-kezelő Azure-fiókjába.
    2.  Ismerkedés a webes szolgáltatás definíció.
    3.  A webes szolgáltatás definíció exportálása JSON.
    4.  Frissítse a ilearner blob a JSON a hivatkozás.
    5.  A JSON importálása a webes szolgáltatás definícióját.
    6.  A webszolgáltatás frissítse az új webhely szolgáltatás definíciót.

## <a name="deploy-the-training-experiment"></a>A tanfolyam kísérlet terjesztése

A tanfolyam kísérlet átképzési webes szolgáltatás üzembe helyezéséhez hozzá kell adnia web service ráfordítások és a kimeneti értékeket a modellt. Kapcsolódás egy *Webes szolgáltatás kimeneti* modult a kísérlet által * [Vonaton modell] [ train-model] * modul engedélyezése a oktatás kísérlet kapcsol új képzett modell, amely a cserélendő kísérlet is használhatja. Ha egy *Felmérése modell* modul, webes szolgáltatás kimeneti értékelése az eredmény érhető el ezt az eredményt is csatolható.

A tanfolyam kísérlet frissítése:

1. Csatlakozás egy *Webes szolgáltatás beviteli* modult a bevitt adatok (például egy *Könnyen áttekinthető hiányzó adatok* modul). A szokásos biztos szeretne lenni, hogy a bemeneti adatok ugyanúgy, mint az eredeti oktatóanyag adatok a feldolgozása.
2. Csatlakozás egy *Webes szolgáltatás kimeneti* modult a *Vonaton modell* modul kimenetét.
3. Ha van egy *Felmérése modell* modul és értékelési eredmények kimeneti, *Webes szolgáltatás kimeneti* modul csatlakozni a kimenet: a *Modell felmérése* modul.

Futtassa a kísérlet.

Ezután telepítenie kell a oktatás kísérlet webes szolgáltatásként, amelyet egy képzett modell és a modell értékelési eredmények.  

A képernyő alján a kísérlet vászon **Webszolgáltatás beállítása**gombra, és válassza az **[Új] webes szolgáltatás telepítése**. Az Azure gépi tanulási Web Services portál a **Webes szolgáltatás üzembe** lapon nyílik meg. Írjon be egy nevet a webes szolgáltatás, válasszon egy kifizetés tervet, és válassza a **központi telepítés**. A köteg végrehajtási mód csak a képzett modelljeinek létrehozásához használható.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Az új adatokat a modell Újraépítés BES használatával

Ebben a példában programmal mutatjuk be C# a átképzési alkalmazást létrehozni. A feladat végrehajtásához Python vagy R példakódot használhatja.

Ha fel szeretne hívni a átképzési API-hoz:

1. C# console-alkalmazás létrehozása a Visual Studióban (**Új** > **Project** > **Windows asztali** > **New**).
2.  Jelentkezzen be a gépi tanulási Web Services portál.
3.  Kattintson a webszolgáltatás, amely dolgozik.
2.  Kattintson a **felhasználni**.
3.  A képernyő alján a **felhasználás** lap **Minta kód** csoportban kattintson a **köteget**.
5.  A köteg végrehajtási minta C# kód másolása, és illessze be a Program.cs fájlt. Győződjön meg arról, hogy a névtér sértetlen marad.

Adja hozzá a NuGet csomag Microsoft.AspNet.WebApi.Client, a megjegyzések meghatározott. A hivatkozás Microsoft.WindowsAzure.Storage.dll hozzáadásához, előfordulhat, hogy először telepítenie kell az [Azure tároló szolgáltatások ügyfél-tár](https://www.nuget.org/packages/WindowsAzure.Storage).

Az alábbi képernyőképen a **felhasználás** lapon az Azure gépi tanulási Web Services portál jeleníti meg.

![Lap felhasználása][1]

### <a name="update-the-apikey-declaration"></a>A apikey nyilatkozat módosítása

Keresse meg a **apikey** nyilatkozat:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

A **felhasználás** lap **alapvető felhasználási információ** szakaszban keresse meg az elsődleges kulcsot, és másolja a vágólapra a **apikey** nyilatkozat.

### <a name="update-the-azure-storage-information"></a>Az Azure tároló adatainak frissítése

A BES példakódot feltölt egy fájlt a helyi meghajtón (például "C:\temp\CensusIpnput.csv") az Azure-tárolóhoz, folyamatok, és az eredmények adatforrásokba Azure tárolóhoz.  

Az Azure tárolási adatainak frissítéséhez kell beolvashatja a fióknév tárhely, billentyűt, és az adatokat tároló tároló fiókjának az Azure klasszikus portálról, és kattintson a kísérlet az eredményül kapott munkafolyamat futtatása után a correspondi frissítés kell lennie az alábbihoz hasonló:

![Eredményül kapott munkafolyamat futtatása után][4]a kód ng értékeinek.

1. Jelentkezzen be az Azure klasszikus portálra.
1. A bal oldali oszlopban kattintson a **tárolás**.
1. Tárterület-fiókok listában jelölje ki egyet a retrained modell tárolásához.
1. A lap alján kattintson a **Hívóbetűk kezelése**gombra.
1. Másolja a vágólapra, és mentse a **Elsődlegeskulcs Access** és a párbeszédpanel bezárásához.
1. A lap tetején kattintson a **tárolók**parancsra.
1. Jelöljön ki egy meglévő tárolót, vagy hozzon létre egy újat, és mentse a nevét.

Keresse meg a *StorageAccountName* *StorageAccountKey*és *StorageContainerName* deklarációs, és frissítse a klasszikus portálról mentett értékeket.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Is gondoskodnia kell arról, hogy a bemeneti fájl érhető el a kódot a megadott helyre.

### <a name="specify-the-output-location"></a>A kimenet helyének megadása

A kimenet helyét a kérése tartalom ad meg, amikor a bővítmény *RelativeLocation* megadott fájl kell megadni `ilearner`. Az alábbi példában látható:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Az alábbi képen átképzési kimenet: ![kimeneti átképzés][6]

## <a name="evaluate-the-retraining-results"></a>Az átképzési eredmények értékelése

Az alkalmazás futtatásakor a eredménye magában foglalja az URL-CÍMEK és a megosztott aláírások jogkivonat értékelési eredmények eléréséhez szükséges.

A teljesítmény eredmények retrained modell kombinálása a *BaseLocation* *RelativeLocation*és *SasBlobToken* *output2* kimeneti eredményekből (mint az előző átképzési kimeneti képen látható), majd a teljes URL-cím beillesztése a böngésző címsorában látható.  

Tekintse meg az eredmények határozza meg, hogy az újonnan képzett modell hajt végre elég jól le szeretné cserélni a meglévőt.

Másolja a *BaseLocation* *RelativeLocation*és *SasBlobToken* a kimeneti közül.

## <a name="retrain-the-web-service"></a>A webszolgáltatás Újraépítés

Új webes szolgáltatás, Újraépítés, amikor frissíti a cserélendő web service definíció az új képzett modell hivatkozni szeretne. A webes szolgáltatás meghatározása a webszolgáltatás képzett mintája egy belső ábrázolása, és nem közvetlenül módosítható. Győződjön meg arról, hogy a webes szolgáltatás definíció keres a cserélendő kísérlet és nem a oktatás kísérlet.

## <a name="sign-in-to-azure-resource-manager"></a>Jelentkezzen be az Azure erőforrás-kezelő

Kell első alkalommal jelentkezik belül a PowerShell környezetet a Azure fiókjába a [Hozzáadás-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag használatával.

## <a name="get-the-web-service-definition-object"></a>A Web Service Definition objektum beolvasása

Ezután beszerzése a Web Service Definition objektum hívja fel a [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Annak megállapításához, az erőforrás-csoport nevét egy meglévő webszolgáltatás, futtassa a Get-AzureRmMlWebService parancsmagot a webes szolgáltatások jelennek meg az előfizetés paraméter nélkül. Keresse meg a webes szolgáltatás, és ott keresse meg a saját webhely szolgáltatás azonosítójával. Az erőforráscsoport neve a negyedik elem azonosítója, a csak a *resourceGroups* elem után. A következő példában az erőforrás-csoport nevét az alapértelmezett-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Azt is megteheti annak megállapításához, az erőforrás-csoport nevét egy meglévő webszolgáltatás, jelentkezzen be az Azure gépi tanulási Web Services portál. Jelölje ki a webszolgáltatás. Az erőforrás-csoport nevét az URL-cím a webszolgáltatás az ötödik elem a csak a *resourceGroups* elem után. A következő példában az erőforrás-csoport nevét az alapértelmezett-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Exportálás JSON Web Service Definition objektumként

Ha módosítani szeretné a képzett modellt az újonnan képzett modell definícióját, először a [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) parancsmag használatával exportálása a JSON-formátumú fájl

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>A ilearner blob hivatkozás frissítése

Az eszközök keresse meg a [képzett modell], frissítse a URI a ilearner blob- *uri* értéke a *locationInfo* csomópontot. A URI hozza létre a *BaseLocation* és a *RelativeLocation* , a hívás átképzés BES eredményéből.

     "asset3": {
        "name": "Retrain Sample [trained model]",
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

## <a name="import-the-json-into-a-web-service-definition-object"></a>A JSON importálása a webes szolgáltatás Definition objektum

A módosított JSON-fájl konvertálása vissza objektummá Web Service Definition frissítése a predicative kísérlet használható az [Importálás-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) parancsmag kell használnia.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>A webszolgáltatás frissítése

Végül a [Frissítés-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) parancsmag használatával frissítheti a cserélendő kísérlet.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
