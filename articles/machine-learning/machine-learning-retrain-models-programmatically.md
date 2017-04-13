<properties
    pageTitle="Gépi tanulási modellek programozás útján Újraépítés |} Microsoft Azure"
    description="Megtudhatja, hogy miként programozás útján Újraépítés a modellt, és a webszolgáltatás szeretné használni az újonnan képzett modell Azure gépi tanulási frissítéséhez."
    services="machine-learning"
    documentationCenter=""
    authors="raymondlaghaeian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Gépi tanulási modellek programozás útján Újraépítés  

Az útmutató megtanulhatja, hogyan programozás útján Újraépítés az Azure gépi tanulási webszolgáltatás C# és a gépi tanulási köteg végrehajtás szolgáltatás használatával.

Miután a modell van retrained, a következő forgatókönyvek bemutatják, hogyan frissíti a modellt, a cserélendő webszolgáltatás:

- Ha telepítette az gépi tanulási Web Services portál klasszikus webszolgáltatás, olvassa el a [Klasszikus webszolgáltatás Újraépítés](machine-learning-retrain-a-classic-web-service.md)című témakört. 
- Ha egy új webes szolgáltatással telepítette, olvassa el a [Újraépítés webszolgáltatás a gépi tanulási kezelő parancsmagok használata](machine-learning-retrain-new-web-service-using-powershell.md)című témakört.

A átképzési folyamat áttekintése látható [Újraépítés gépi tanulási modell](machine-learning-retrain-machine-learning-model.md).

A meglévő új Azure-alapú erőforrás kezelő webszolgáltatás kezdődik, című témakörben olvashat [Újraépítés egy meglévő prediktív webszolgáltatásból](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Hozzon létre egy oktatás kísérlet
 
Ebben a példában a használandó "minta 5: bináris osztályozási vonaton, vizsgálat kiértékelés: felnőtt adatkészlet" a Microsoft Azure gépi tanulási minták. 
    
A kísérlet létrehozása:

1.  Jelentkezzen be a Microsoft Azure gép Studio tanulási. 
2.  Kattintson a jobb alsó sarkában az irányítópulton az **Új**gombra.
3.  A Microsoft Samples kattintson a minta 5.
4.  Nevezze át a kísérlet a kísérlet területén válassza a felső részén jelölje be a kísérlet neve "minta 5: bináris osztályozási vonaton, vizsgálat kiértékelés: felnőtt adatkészlet".
5.  Típus népszámlálás modell.
6.  A képernyő alján a kísérlet vászon, kattintson a **Futtatás**parancsra.
7.  Kattintson a **beállítás webszolgáltatás** , és válassza ki a **Retraining webszolgáltatásból**. 

    ![Kezdeti kísérlet.][2]

Diagram 2: Kezdeti kísérlet.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Hozzon létre egy prediktív kísérlet és közzététele, például egy webszolgáltatásból  

Ezután hozzon létre egy Predicative kísérlet.

1.  A kísérlet vászon alján kattintson a **Webes szolgáltatás beállítása** , és válassza ki a **Cserélendő webszolgáltatás**. Ez a modell menti képzett modellben, és hozzáadja a webes szolgáltatás bemeneti és kimeneti modulok. 
2.  Kattintson a **Futtatás**parancsra. 
3.  A kísérlet futtatása után kattintson a **Webes szolgáltatás üzembe [klasszikus]** vagy **Üzembe webszolgáltatás [Új]**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>A tanfolyam kísérlet üzembe oktatás webes szolgáltatásként

A képzett modell Újraépítés, telepítenie kell az Ön által létrehozott Retraining webes szolgáltatásként oktatás kísérletet. Ez a webszolgáltatás szüksége van egy *Webes szolgáltatás kimeneti* modulra csatlakozik a * [Vonaton modell] [ train-model] * modul engedélyezni szeretné, hogy új képzett modellek kiszámítására.

1. Szeretne visszatérni a oktatás kísérlet, kattintson a bal oldali ablaktáblában a kísérletek ikonra, majd kattintson a kísérlet népszámlálás modell nevű.  
2. A keresés kísérlet elemek keresése mezőbe írja be a webszolgáltatás. 
3. Húzzon egy *Webes szolgáltatás beviteli* modult a kísérlet vászon, és az eredményt csatlakozzon a *Hiányzó adatok karbantartása* modulra.  Ezzel biztosíthatja, hogy a átképzési adatfeldolgozás ugyanúgy, mint az eredeti oktatóanyag adatok.
4. Húzza a kísérlet vászon két *webszolgáltatás kimeneti* modulokat. A kimenet a *Vonaton modell* modul csatlakozni az egyik, a kimeneti a *Modell felmérése* modul között. A web service kimenet **Vonaton** modell ad nekünk az új képzett modell. A kimenet **Felmérése modell** csatolva, hogy a modul kimeneti, amely a teljesítmény eredményt adja vissza.
5. Kattintson a **Futtatás**parancsra. 

Ezután telepítenie kell a oktatás kísérlet webes szolgáltatásként, amelyet egy képzett modell és a modell értékelési eredmények. Ehhez a következő műveletsorozat függnek dolgozik, hogy a klasszikus webszolgáltatás vagy egy új webszolgáltatásból.  
  
**Klasszikus webszolgáltatás**

A kísérlet vászon alján kattintson a **Webes szolgáltatás beállítása** , és válassza a **Webes szolgáltatás üzembe [klasszikus]**. A webszolgáltatás **Irányítópult** jelenik meg az API-billentyűt, és az API-Súgó lap köteg végrehajtásához. Csak a köteg végrehajtási mód képzett modellek létrehozására használható.

**Új webes szolgáltatás**

A kísérlet vászon alján kattintson a **Webes szolgáltatás beállítása** , és válassza az **[Új] webes szolgáltatás üzembe**. A Web Service Azure gépi tanulási webszolgáltatásokhoz portál a központi telepítés szolgáltatás lap nyílik meg. Nevezze el a webes szolgáltatás, és válasszon egy kifizetés tervet, majd kattintson a **központi telepítés**. A köteg adatvégrehajtás-módszer használható képzett modellek létrehozása

Bármelyik lehetőséget választja kísérlet futtatása után az eredményül kapott munkafolyamat következőképpen kell kinéznie:

![Eredményül kapott munkafolyamat futtatása után.][4]

Diagram 3: Így munkafolyamat futtatása után.

## <a name="retrain-the-model-with-new-data-using-bes"></a>A modell Újraépítés BES használatával új adatokkal

Ebben a példában esetén C# a átképzési alkalmazást létrehozni. A Python vagy R példakódot is használható, ha a feladat végrehajtásához.

Ha fel szeretne hívni a átképzés API-hoz:

1. C# Console-alkalmazás létrehozása a Visual Studióban (új -> a Project -> Windows asztali -> New).
2.  Jelentkezzen be a gépi tanulási webszolgáltatás portálra.
3.  Ha klasszikus webszolgáltatás dolgozik, kattintson a **Klasszikus webszolgáltatásokhoz**.
    1.  Kattintson a webes szolgáltatás használata.
    2.  Kattintson az alapértelmezett végpontot.
    3.  Kattintson a **felhasználni**.
    4.  A képernyő alján a **felhasználás** lap **Minta kód** csoportban kattintson a **köteget**.
    5.  Folytassa 5 ezt az eljárást.
4.  Ha egy új webszolgáltatás dolgozik, kattintson a **Webes szolgáltatások**.
    1.  Kattintson a webes szolgáltatás használata.
    2.  Kattintson a **felhasználni**.
    3.  A képernyő alján a felhasználás lap **Minta kód** csoportban kattintson a **köteget**.
5.  A köteg végrehajtási minta C# kód másolása, és illessze be a Program.cs fájl gondoskodhat arról, hogy a névtér sértetlen marad.

Adja hozzá a Nuget csomag Microsoft.AspNet.WebApi.Client a megjegyzések meghatározott. A hivatkozás Microsoft.WindowsAzure.Storage.dll hozzáadásához, előfordulhat, hogy először telepítenie kell a Microsoft Azure tárolása az ügyfél tárat. További információ című témakör tartalmaz [Windows tárhelyet](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>A apikey nyilatkozat módosítása

Keresse meg a **apikey** nyilatkozat.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

A **felhasználás** lap **alapvető felhasználási információ** szakaszban keresse meg az elsődleges kulcsot, és másolja a vágólapra a **apikey** nyilatkozat.

### <a name="update-the-azure-storage-information"></a>Az Azure tároló adatainak frissítése

A BES példakódot feltöltések megjelenítése az Azure-tárolóhoz (például "C:\temp\CensusIpnput.csv") a helyi meghajtón tárolt fájlt, folyamatok, és az eredmények adatforrásokba Azure tárolóhoz.  

A feladat végrehajtásához a tárhely nevét, a billentyű és a tároló fiókadatok kell lekérése a tárterület-fiókot, a klasszikus Azure-portál és -a kódot a frissítés megfelelő értékeiből a. 

1. Jelentkezzen be a klasszikus Azure portálra.
1. A bal oldali oszlopban kattintson a **tárolás**.
1. Tárterület-fiókok listában jelölje ki egyet a retrained modell tárolásához.
1. A lap alján kattintson a **Hívóbetűk kezelése**gombra.
1. Másolja a vágólapra, és mentse a **Elsődlegeskulcs Access** és a párbeszédpanel bezárásához. 
1. A lap tetején kattintson a **tárolók**parancsra.
1. Jelöljön ki egy meglévő tárolót, vagy hozzon létre egy újat, és mentse a nevét.

Keresse meg a *StorageAccountName* *StorageAccountKey*és *StorageContainerName* nyilatkozatokat, és frissítse az Azure portálról mentett értékeket.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Is gondoskodnia kell a bemeneti fájl érhető el a helyen, a kód ad meg. 

### <a name="specify-the-output-location"></a>A kimenet helyének megadása

A kimenet helyét a kérése tartalomban megadásakor *RelativeLocation* megadott fájl kiterjesztését ilearner kell megadni. 

Az alábbi példában látható:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] A kimenet helyét azoknak a eltérhetnek az útmutató a sorrendben, amelyben a webes szolgáltatás kimeneti modulok felvett alapján szerint a lehetőségekből. Mivel a két kimeneti értékeket ez oktatás kísérletet állít be, az eredmények a tárolási helyére vonatkozó adatok őket a egyaránt tartalmazza.  

![Kimeneti átképzés][6]

Diagram 4: Kimeneti átképzés.

## <a name="evaluate-the-retraining-results"></a>Az átképzési eredmények értékelése
 
Az alkalmazás futtatásakor a a eredménye magában foglalja a értékelési eredmények eléréséhez szükséges URL-CÍMEK és a biztonsági jogkivonat.

Megjelenítheti a retrained modell teljesítmény eredményének egyesítése a *BaseLocation* *RelativeLocation*és *SasBlobToken* *output2* kimeneti eredményekből (mint az előző átképzési kimeneti képen látható), és illessze be a teljes URL-CÍMÉT a böngésző címsorában.  

Tekintse meg az eredmények határozza meg, hogy az újonnan képzett modell hajt végre elég jól le szeretné cserélni a meglévőt.

Másolja a vágólapra a *BaseLocation* *RelativeLocation*és *SasBlobToken* a kimeneti közül, ezeket a átképzési során fogja használni.

## <a name="next-steps"></a>Következő lépések

Ha a cserélendő webszolgáltatás **Webes szolgáltatás üzembe [klasszikus]**kattintva telepítette, olvassa el a [Klasszikus webszolgáltatás Újraépítés](machine-learning-retrain-a-classic-web-service.md)című témakört.

Ha telepítette a cserélendő webszolgáltatás **[Új] webes szolgáltatás üzembe**gombra kattintva, olvassa el a [Újraépítés webszolgáltatás a gépi tanulási kezelő parancsmagok használata](machine-learning-retrain-new-web-service-using-powershell.md)című témakört.

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/