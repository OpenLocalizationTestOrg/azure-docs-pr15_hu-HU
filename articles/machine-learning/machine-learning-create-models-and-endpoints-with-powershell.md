<properties
pageTitle="Több modellek létrehozása egy kísérlet |} Microsoft Azure"
description="Hozzon létre gépi tanulási modellek és a webes szolgáltatás végpontok az azonos algoritmus, de a másik oktatás adatkészleteket a PowerShell használatával."
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Sok gépi tanulási modellek és készítése webes szolgáltatás végpontok egy kísérlet PowerShell használatával

Íme a gyakori gépi tanulási probléma: szeretne létrehozni, amely oktatás ugyanazt a munkafolyamatot, és használja ugyanazt a algoritmust, de rendelkezik különböző oktatás adatkészleteket bemeneteként sok modell. Ez a cikk bemutatja, hogyan Azure gépi tanulási Studio skála a művelet csak egyetlen kísérlet használatával.

Ha például tegyük fel, hogy a globális kerékpárhoz bérleti franchise üzleti tulajdonjogának. Szeretne összeállítása a regressziós modell előre a bérleti igény szerint a visszamenőleges adatok alapján. 1000 bérleti helyek világszerte van, és akkor gyűjtött adatok egy adathalmaz minden egyes helyre, például dátum, idő, időjárás fontos szolgáltatást tartalmaz, és a jellemző minden olyan helyen, forgalmat.

A modell többször verziójával egyesített összes adatkészletek át az összes hely sikerült képzése. De a helyek mindegyikének egy egyedi környezet, mert a jobb megoldást jelenthet a regressziós modell külön-külön minden olyan helyen, használja az adatkészlet betanítása lenne. Úgy, hogy minden képzett modell sikerült a különböző tároló méretét, mennyiségi, földrajzi, statisztikai sokaság, kerékpárhoz környezetbarát forgalom környezetben, figyelembe *stb*.

Lehet, hogy a legjobb megközelítés, de nem szeretné 1000 oktatás kísérletek létrehozása az Azure gépi tanulási egyenként, amely egy egyedi helyet. Amellett, hogy egy ellenőrizni tevékenységet, érdemes emellett úgy tűnik, mivel minden egyes kísérlet szeretné, hogy ugyanazokat az oktatás adatkészlet kivételével összetevők meglehetősen hatékony.

Szerencsére azt is elvégezheti az Ez az [Azure gépi tanulási átképzés API](machine-learning-retrain-models-programmatically.md) segítségével, és a tevékenység [Azure gépi tanulási](machine-learning-powershell-module.md)PowerShell automatizálása.

> [AZURE.NOTE] Ahhoz, hogy a minta gyorsabban, azt fogja csökkentse a helyek való 1000 10 számát. De 1000 helyek elvek és a eljárások vonatkoznak. Az egyetlen különbség, ha láthatja el ismeretekkel a 1000 adatkészleteket szeretne valószínűleg is szeretne, az alábbi PowerShell-parancsfájlokat párhuzamosan futó számításba. Ennek módja a jelen cikk túlra, de talál példákat PowerShell több szálon futó műveletvégzés az interneten.  

## <a name="set-up-the-training-experiment"></a>A tanfolyam kísérlet beállítása

Egy példa [oktatás kísérletezés](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) , hogy már létrehozott a [Cortana üzletiintelligencia-gyűjtemény](http://gallery.cortanaintelligence.com)használandó megyünk. Nyissa meg a kísérlet az [Azure gépi tanulási Studio](https://studio.azureml.net) munkaterület.

>[AZURE.NOTE] Ez a példa együtt követni, előfordulhat, hogy használni kívánt ingyenes munkaterület, hanem szabvány munkaterület. Azt létre egyik végpontját ügyfelek – 10 végpontok - összesen, és mivel ingyenes munkaterület 3 végpontok korlátozódik, amely esetén kell szabványos munkaterület. Ha csak egy ingyenes munkaterület, egyszerűen módosítsa az alábbi csak 3 helyek engedélyezése a parancsfájlok.

A kísérlet- **Adatok importálása** modul használja a oktatás adatkészlet *customer001.csv* importálása Azure tárterület-fiókkal. Tegyük fel, hogy oktatás adatkészleteket gyűjtött kerékpárhoz bérleti mindenhol és *rentalloc10.csv* *rentalloc001.csv* kezdve fájlnévvel blob-tároló adott helyen tárolja őket.

![kép](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Figyelje meg, hogy egy **Webes szolgáltatás kimeneti** modult a **Vonaton modell** modul lett hozzáadva.
Ha telepíti a kísérlet webes szolgáltatásként, a végpont társított kimeneti .ilearner fájl formátuma visszaadja a képzett modell.

Tartsa szem előtt, hogy azt beállítása egy webszolgáltatási paraméter az URL-címet, amely az **Adatok importálása** modul használja. Lehetővé teszi a paraméter használatával adja meg az egyes oktatás adatforrást láthatja el ismeretekkel a minden olyan helyen, a modell us.
Az alábbi egyéb módon azt van megtehette, például adatok beolvasása SQL Azure-adatbázis egy webszolgáltatási paraméter egy SQL-lekérdezés használja, vagy egyszerűen egy **Webes szolgáltatás beviteli** modulról átadni a webszolgáltatás adatkészletet.

![kép](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Most vegyük futtassa az Ez az alapértelmezett érték *rental001.csv* használata az oktatás adatkészlet oktatás kísérlet. Ha megtekintheti a kimenet a **kiértékelés** modul (a kimenet gombra, és válassza a **Megjelenítés**), megtekintheti azt első *AUC* egy decent teljesítményének = 0,91. Ezen a ponton, hogy ki a oktatás kísérlet webszolgáltatás telepíthető.

## <a name="deploy-the-training-and-scoring-web-services"></a>A képzés és webszolgáltatások pontozási terjesztése

Az oktatási webes szolgáltatás üzembe helyezéséhez kattintson a kísérlet vászon alatti **Webszolgáltatás beállítása** gombra, és válassza a **Webes szolgáltatás üzembe**. Hívja fel a webszolgáltatás "" kerékpárhoz bérleti képzése".

Most már szükség a pontozási webszolgáltatás telepítése.
Ehhez, hogy a vászonra alatt kattintson a **Webes szolgáltatás beállítása** , és válassza ki a **Cserélendő webszolgáltatás**. Ezzel létrehoz egy pontozási kísérlet.
Azt, hogy legyen a munka webes szolgáltatásként, például a címke "számláló" oszlop eltávolítása, a bemeneti adatok a néhány kisebb vonatkozó kell, és korlátozása a kimenet csak a példány azonosító és a hozzá tartozó előre jelzett érték.

Használható megkímélheti magát, egyszerűen megnyithatja a [cserélendő kísérlet](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) a gyűjteményben, amely már lett elkészítve.

A webszolgáltatás telepítéséhez futtassa a cserélendő kísérlet, majd kattintson a alatt a vászonra a **Webes szolgáltatás üzembe** gomb. A pontozási webszolgáltatás "Kerékpárhoz bérleti pontozási" név ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>10 azonos web service végpontok PowerShell létrehozása

Ez a szolgáltatás alapértelmezett végpont megtalálható. De azt nem az alapértelmezett végpont kíváncsi, mivel az nem lehet frissíteni. Mi a teendő szükség 10 további végpontok, minden olyan helyen, az egyik létrehozásához. A PowerShell azt fogja ehhez.

Először is azt a PowerShell környezet beállítása:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Futtassa az alábbi PowerShell-parancsot:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Most már létrehozott 10 végpontok, és minden bennük a *customer001.csv*képzett ugyanazt a képzett modellt. Az adatkezelési portálon Azure megtekintheti őket.

![kép](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Frissítse a végpontok használatához külön oktatás adatkészleteket PowerShell használatával

A következő lépésként modellek egyedileg képzett minden ügyfél egyes adatok frissítése a végpontok. De először szükség konzerv e modellek **Kerékpárhoz bérleti oktatás** webszolgáltatásból. Térjen vissza a a **Kerékpárhoz bérleti oktatás** webszolgáltatás. Hívja fel a 10 kivonást 10 különböző oktatás adatkészleteket BES végpontját 10 különböző modellek előállításához szükséges. A **InovkeAmlWebServiceBESEndpoint** PowerShell-parancsmag használjuk ehhez.

Meg is kell adnia a hitelesítő adatokat a blob-tároló fiók be `$configContent`, azaz a mezőket a `AccountName`, `AccountKey` és `RelativeLocation`. A `AccountName` veheti fel a fióknevét esetén a **Klasszikus Azure Kezelőportálja** (*tárhely* lap). Tárterület-fiókjában gombra annak `AccountKey` a **Hívóbetűk kezelése** gomb lenyomásával a képernyő alján, és a következő *Hívóbetű elsődleges*másolásának találhatók. A `RelativeLocation` új modell tárolási helyére a tárhely viszonyított elérési van. Például az elérési út `hai/retrain/bike_rental/` nevű tároló pontok alatt a parancsfájl `hai`, és `/retrain/bike_rental/` almappái. Jelenleg nem hozhat létre a felhasználói felület portálon keresztül almappákat, de vannak, amelyek lehetővé teszik ehhez [Több Azure tároló kezelők](../storage/storage-explorers.md) . Azt ajánljuk, hogy a következőképpen tárolja az új képzett modelljei (.ilearner fájlok)-tárolóban lévő hozzon létre egy új tároló: a tárterület lapon kattintson a **Hozzáadás** gombra a képernyő alján, és nevezze el `retrain`. Összefoglalásként tehát elmondhatjuk, a szükséges módosításokat az alábbi parancsfájl vonatkoznak `AccountName`, `AccountKey` és `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] A BES végpont ehhez a művelethez csak támogatott módban. Erőforrásrekordok képzett modellek készítésére nem használhatók.

Tekinthetik meg a fenti helyett megépítése 10 különböző BES feladat konfigurációs json fájlokat, hogy dinamikusan létrehozni a config karakterláncot, és hírcsatorna **InvokeAmlWebServceBESEndpoint** parancsmag *jobConfigString* paraméterének, mivel nagyon nincs szükség a lemezen őrizni.

Ha mindent mellé kerül, egy idő után meg kell jelennie 10 .ilearner fájlok, *model001.ilearner* *model010.ilearner*, hogy a Azure tárterület-fiókjában. Most, hogy készen áll a 10 pontozási web service végpontok frissítse ezeket a **Javítás-AmlWebServiceEndpoint** PowerShell parancsmaggal modellek. Ne feledje, hogy újra, hogy azt csak javítás a az alapértelmezettől végpontokat programozás útján létrehozott korábbi verziójában.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Ez meglehetősen gyorsan fusson. Amikor befejezte a végrehajtás, azt fogja sikeresen létrehozott 10 prediktív web service végpontok, az adott adatkészlet a bérleti helyre, az összes olyan egyetlen képzési kísérlet egyedileg képzett képzett modell tartalmazó. Ennek ellenőrzéséhez, próbálja meg ezeket a **InvokeAmlWebServiceRRSEndpoint** parancsmaggal, a végpontokat hívásához nyújtó őket ugyanarra a bemeneti adatokat tartalmazó, és meg kell különböző előrejelzési eredmények megtekintéséhez, mivel a modellek vannak képzett oktatás különböző értékkészletet.

## <a name="full-powershell-script"></a>Teljes PowerShell-parancsprogramot

Az alábbiakban a teljes forráskód listája:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
