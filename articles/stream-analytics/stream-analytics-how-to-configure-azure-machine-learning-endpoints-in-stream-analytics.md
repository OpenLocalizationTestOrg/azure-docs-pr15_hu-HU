<properties 
    pageTitle="Azure gépi tanulási végpontok konfigurálása a Értékáram-elemzés |} Microsoft Azure" 
    description="Értékáram-elemzés a gép nyelvi a felhasználó által definiált függvények"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>A gép tanulási integrációs Értékáram-elemzés

Értékáram-elemzés támogatja a felhasználó által definiált függvényeket Azure gépi tanulási végpontokhoz feliratozni. Ez a funkció REST API-támogatása a [Értékáram-elemzés REST API-tár](https://msdn.microsoft.com/library/azure/dn835031.aspx)részletes. Ebben a cikkben ezt a lehetőséget, a megjelenítő Értékáram-elemzés sikeres végrehajtásához szükséges kiegészítő információk. Egy oktatóprogram is közzétett és érhető el [az alábbi](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Áttekintés: Azure gépi tanulási kifejezések

Microsoft Azure gépi tanulási rendelkezik egy olyan együttműködési, húzással történő áthelyezés eszközzel, hozhat létre, tesztelése és az adatokon prediktív analytics-megoldások telepítése. Ez az eszköz a *Azure gépi tanulási Studio*neve. A studio-lapok használata a gépi tanulási források és könnyen összeállítása, tesztelése és az találta kattintson a tervezés szolgál. Ezek az erőforrások és a definíciók alatt találhatók.

- **Munkaterület**: A *munkaterület* olyan tároló, amely minden más gépi tanulási források az együttes kezelése és a vezérlő tárolóban.
- **Kísérlet**: adatok tudósok csatlakozást adatkészleteket, és gépi tanulási modell képzése *kísérletek* hozza létre.
- **Végpont**: *Végpontok* azok az Azure gépi tanulási objektum használt funkciók bemeneteként, a megadott gépi tanulási modell alkalmazása és pontszáma eredményt ad vissza.
- **Webszolgáltatás pontozás**: *webszolgáltatás pontozási* gyűjteménye végpontok fent említett.

Minden végpontra foglalja magában: API-khoz köteg végrehajtási és szinkron végrehajtása Értékáram-elemzés szinkronizált végrehajtás használ. Az adott szolgáltatás AzureML Studio [Kérés/válasz szolgáltatás](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) neve.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Gépi tanulási Értékáram-elemzés feladatokhoz szükséges források

Értékáram-elemzés céljából feladat feldolgozása, egy kérés/válasz végpontot, egy [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)és swagger definícióját szükségesek az összes sikeres végrehajtás. Értékáram-elemzés, amely szerkezetek swagger végpont URL-címét, a felület és visszaadása egy alapértelmezett UDF megadása a felhasználó számára további zárólap tartalmaz.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Egy adatfolyam analitikai és gépi tanulási UDF keresztül REST API konfigurálása

A REST API-khoz is beállíthatja az Azure Machine Language függvényeket a feladat. A lépések a következők:

1. Értékáram-elemzés feladat létrehozása
2. A beviteli meghatározása
3. Egy kimenet meghatározása
4. Felhasználó által definiált függvény (UDF) létrehozása
5. Értékáram-elemzés átalakítás, amely felhívja a UDF írása
6. A projekt indítása

## <a name="creating-a-udf-with-basic-properties"></a>Egy UDF létrehozása egyszerű tulajdonságok

Példaként a példa egy skaláris UDF Azure gépi tanulási zárólap köti *newudf* nevű hoz létre. Megjegyzendő, hogy a választott szolgáltatás és az *apiKey* tartozó API súgó lapon található *végpont* (URI-szolgáltatás) a fő-szolgáltatások lapon találhatók.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Példa összehívás törzsébe:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Az alapértelmezett UDF hívás RetrieveDefaultDefinition végpont

A skeleton UDF létrehozása után az UDF teljes definícióját van szükség. A RetreiveDefaultDefinition végpont segítséget az alapértelmezett definíciója Azure gépi tanulási zárólap kötött skaláris függvény. Az alábbi tartalom szükséges, hogy az alapértelmezett UDF definíciója Azure gépi tanulási zárólap kötött skaláris függvény első. Azt nem adja meg a tényleges végpont már kapott helyezése kérelem során. Értékáram-elemzés az végpontot, ha explicit módon megadva a kérelem leírt hívások. Egyéb esetben a hivatkozott eredetileg egy használ. Itt a UDF veszi egyetlen karakterlánc paraméter (mondatok), és adja eredményül, egyetlen kimeneti típus karakterlánc, ami azt jelenti, hogy mondat "sentiment" címke.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Példa összehívás törzsébe:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Minta kimeneti a keresés valamit, amit szeretne alatt.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>A kérdésre adott választ tartalmazó javítás UDF 

Most már az UDF kell javítani az előző válasz, az alább látható módon.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

A kérelem törzs (RetrieveDefaultDefinition kimenetét):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Értékáram-elemzés átalakítása hívja fel az UDF megvalósítása

Most már lekérdezhetők az UDF (Itt nevű scoreTweet) minden bemeneti esemény, és az esemény választ írási olyan eredménye.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
