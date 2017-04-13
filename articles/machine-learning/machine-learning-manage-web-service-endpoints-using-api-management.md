<properties
    pageTitle="Megtudhatja, hogy miként API-kezelés segítségével AzureML webszolgáltatások kezelése |} Microsoft Azure"
    description="Segédvonalhoz ábrázoló API-kezelés segítségével AzureML webszolgáltatások kezelése."
    keywords="gépi tanulási, api-kezelés"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Megtudhatja, hogy miként API-kezelés segítségével AzureML webes szolgáltatások kezelése

##<a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, hogy hogyan gyorsan használatának első lépései API-kezelés kezelheti a AzureML webes szolgáltatások.

##<a name="what-is-azure-api-management"></a>Mi az Azure API szolgáltatás?

Azure API-kezelés egy Azure szolgáltatás, amellyel a REST API-végpontok kezelése felhasználói hozzáférés szabályozása használatát és irányítópult figyelése megadásával. Kattintson [ide](https://azure.microsoft.com/services/api-management/) Azure API-kezelés részletesen. Kattintson [az alábbi](../api-management/api-management-get-started.md) útmutatást Azure API-kezelés – első lépések útmutató. Ez más útmutató, ez az útmutató alapul, amely bemutatja a további témakörök, például értesítési beállítások, a réteg árak, a válasz kezelését, a felhasználói hitelesítés, termékek, fejlesztőeszközök előfizetések és használati dashboarding létrehozása.

##<a name="what-is-azureml"></a>Mi az AzureML?

AzureML gépi tanulási, amely lehetővé teszi, hogy könnyen összeállítása, üzembe helyezéséhez és fejlett analitikai megoldások megosztása az Azure szolgáltatásainak. Kattintson [ide](https://azure.microsoft.com/services/machine-learning/) AzureML részletesen.

##<a name="prerequisites"></a>Előfeltételek

Ez az útmutató elvégzéséhez az alábbiakra van szüksége:

* Az Azure-fiók. Ha nem az Azure-fiók, kattintson [ide](https://azure.microsoft.com/pricing/free-trial/) ingyenes próbaverzió fiók létrehozása olvashat.
* Egy AzureML fiók. Ha nincs AzureML fiókkal, kattintson [ide](https://studio.azureml.net/) ingyenes próbaverzió fiók létrehozása olvashat.
* A munkaterület, szolgáltatás, és a webes szolgáltatásként rendszerbe AzureML kísérlet api_key. Kattintson a [Itt](machine-learning-create-experiment.md) olvashat AzureML kísérlet létrehozásához. Kattintson [ide](machine-learning-publish-a-machine-learning-web-service.md) AzureML kísérlet, webes szolgáltatás üzembe olvashat. Másik megoldás mintája rendelkezik létrehozása és egy egyszerű AzureML kísérlet és webes szolgáltatás üzembe útmutatót.

##<a name="create-an-api-management-instance"></a>Hozza létre az API-kezelés

Az alábbiakban az API-kezelés segítségével kezelheti a AzureML webszolgáltatás lépéseit. Először hozza létre a szolgáltatás. Jelentkezzen be a [Klasszikus portálra](https://manage.windowsazure.com/) , és kattintson az **Új** > **Alkalmazás szolgáltatások** > **API-kezelés** > **létrehozása**.

![példány létrehozása](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Adja meg, hogy egyedi **URL-CÍMÉT**. Ez az útmutató használja **demoazureml** – meg kell mást válassza. Válassza ki a kívánt **előfizetést** , és **régió** a szolgáltatás példány. A beállítások elvégzése után kattintson a Tovább gombra.

![Hozzon létre-szolgáltatás-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Adja meg az érték a **Szervezet neve**. Ez az útmutató használja **demoazureml** – meg kell mást válassza. A **rendszergazda e-mail címe** mezőben írja be az e-mail címét. Ezt az e-mail címét az API a Projektirányítási rendszerek az értesítések szolgál.

![Hozzon létre szolgáltatás – 2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Kattintson a jelölőnégyzet bejelölésével hozza létre a szolgáltatás. *Hozható létre egy új szolgáltatás legfeljebb 30 percig tart*.

##<a name="create-the-api"></a>Az API létrehozása

Amikor létrejött a szolgáltatás példánya, következő lépésként az API létrehozásához. Egy API-t, ahol a műveleteket, amelyek az ügyfélalkalmazás hívható csoportja. API-műveletek proxy meglévő webes szolgáltatásokhoz. Ez az útmutató API-hoz, hogy a meglévő AzureML Erőforrásrekordok és BES webszolgáltatásokhoz proxy hoz létre.

API-k létrehozása és beállítva a publisher az Azure klasszikus portálon keresztül érhető el API portálon. A publisher portál elérése végett jelölje ki a szolgáltatás-példányt.

![Jelölje ki-szolgáltatás-példány](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Kattintson a **kezelés** , az Azure klasszikus portálon a API-kezelési szolgáltatáshoz.

![szolgáltatás kezelése](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

**API-khoz** kattintson a bal oldali menüjében **API-kezelés** , és kattintson a **API hozzáadása**gombra.

![API-kezelés – menü](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Írja be a **AzureML bemutató API** a **webes API-nevét**. Írja be a **https://ussouthcentral.services.azureml.net** , **webes szolgáltatás URL-CÍMÉT**. Írja be **a bemutató azureml** szerepel a **webes API URL-címe utótag**. Jelölje be a **Webes API URL-címe** az Office-téma **HTTPS** . Jelölje ki a **Starter** **termékként**. Amikor befejezte, kattintson a **Mentés** az API létrehozása.

![Adja hozzá-új-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>A műveletek hozzáadása

Kattintson a **Hozzáadás művelet** műveletek hozzáadása a API-val.

![művelet hozzáadása](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Jelenik meg az **Új művelet** ablakban, és az **aláírása** lap alapértelmezés szerint lesz kijelölve.

##<a name="add-rrs-operation"></a>Erőforrásrekordok művelet hozzáadása

Először létre kell hoznia egy műveletet a AzureML Erőforrásrekordok szolgáltatás. Jelölje ki a **BEJEGYZÉST** a **HTTP-művelet**megegyezik. Típus **/workspaces/ {munkaterület} /services/ {szolgáltatás} / execute?api verziójú {apiversion} = & Részletek = {részletek}** **sablon URL-CÍMÉT**. Írja be a **Erőforrásrekordok hajtsa végre** a **megjelenítendő nevet**.

![erőforrásrekordok-művelet-aláírás hozzáadása](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Kattintson a **Válasz** > **hozzáadása** a balra, és válassza a **200 OK**. Ez a művelet mentése a **Mentés** gombra.

![Adja hozzá-erőforrásrekordok-művelet-válasz](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>BES műveletek hozzáadása

Képernyőképek nem szerepelnek a BES műveletek, nagyon hasonlóak, csak azok a rekordok művelet hozzáadása.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Egy köteg végrehajtási feladat elküldése (de nem indítása)

Kattintson a **művelet hozzáadása** a AzureML BES művelet hozzáadása az API-t. Jelölje be a **HTTP-utasítás** **BEJEGYZÉST** . Típus **/workspaces/ {munkaterület} /services/ {szolgáltatás} / jobs?api verziójú = {apiversion}** a **sablon URL-CÍMÉT**. Írja be a **BES elküldése** a **megjelenítendő nevet**. Kattintson a **Válasz** > **hozzáadása** a balra, és válassza a **200 OK**. Ez a művelet mentése a **Mentés** gombra.

###<a name="start-a-batch-execution-job"></a>Feladat Köteg végrehajtás indítása

Kattintson a **művelet hozzáadása** a AzureML BES művelet hozzáadása az API-t. Jelölje be a **HTTP-utasítás** **BEJEGYZÉST** . Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid} / start?api verziójú = {apiversion}** a **sablon URL-CÍMÉT**. Írja be a **BES indítsa el** a **megjelenítendő nevet**. Kattintson a **Válasz** > **hozzáadása** a balra, és válassza a **200 OK**. Ez a művelet mentése a **Mentés** gombra.

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>Az állapot vagy egy köteg végrehajtási feladat eredménye

Kattintson a **művelet hozzáadása** a AzureML BES művelet hozzáadása az API-t. Jelölje ki az **első** a **HTTP-utasítás**. Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid} ?api verziójú = {apiversion}** a **sablon URL-CÍMÉT**. Írja be a **megjelenítendő név** **BES állapotát** . Kattintson a **Válasz** > **hozzáadása** a balra, és válassza a **200 OK**. Ez a művelet mentése a **Mentés** gombra.

###<a name="delete-a-batch-execution-job"></a>A köteg adatvégrehajtás-feladat törlése

Kattintson a **művelet hozzáadása** a AzureML BES művelet hozzáadása az API-t. Jelölje be a **törlése** a **HTTP-utasítás**. Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid} ?api verziójú = {apiversion}** a **sablon URL-CÍMÉT**. Írja be a **BES törlése** a **megjelenítendő nevet**. Kattintson a **Válasz** > **hozzáadása** a balra, és válassza a **200 OK**. Ez a művelet mentése a **Mentés** gombra.

##<a name="call-an-operation-from-the-developer-portal"></a>A Developer Portal segítségével hívhat fel egy művelet

Műveletek közvetlenül a Developer Portal segítségével, amely magában foglalja a megtekintése, és a műveleteket egy API teszteléséhez kényelmesen hívható. Ebben a lépésben útmutató a a Erőforrásrekordok metódus **Végrehajtása** a **AzureML bemutató API**felvett hívja fel. **Developer Portal segítségével** kattintson a menü felső sarkában látható a klasszikus portálon.

![Fejlesztőeszközök-portál](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

**API** a felső parancsot, és kattintson a **AzureML bemutató API** az elérhető műveletek megtekintéséhez.

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Jelölje ki a **Erőforrásrekordok hajtsa végre** a művelethez. Kattintson a **próbálja ki**.

![az informatikai kipróbálása](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Kérés paraméterekkel írja be a **munkaterület**, **szolgáltatás**, a **apiversion** **2.0-s** , **Igaz,** a **Részletek**. A **munkaterület** és a **szolgáltatás** megtalálhatja a AzureML web service dashboard (lásd: **a webszolgáltatás próba** mintája).

Kérés fejlécek, kattintson az **Élőfej hozzáadása** , és írja be a **Tartalomtípus -** és az **alkalmazás/json**, majd kattintson az **Élőfej hozzáadása** , és írja be az **Engedélyezés** és **Bearer <YOUR AZUREML SERVICE API-KEY> **. A **API-ja kulcs** megtalálhatja a AzureML web service irányítópulton (lásd: **a webszolgáltatás próba** mintája).

Típus **{"Ráfordítások": {"input1": {"ColumnNames": "Értékek" ["Col2"]: [["az jó nap"]]}}, "GlobalParameters": {}}** összehívás törzsében.

![azureml-bemutató-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Kattintson a **Küldés**gombra.

![Küldés](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Művelet hivatkoznak, miután a a developer Portal segítségével a **Kért URL-CÍMÉT** a háttéradatbázist szolgáltatás, **válaszállapota**, a **Válasz fejlécek**és **Válasz tartalmat**jeleníti meg.

![Válasz-állapota](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Mintája - létrehozása és tesztelése a egy egyszerű AzureML webszolgáltatás

###<a name="creating-the-experiment"></a>A kísérlet létrehozása

Alatti lépések egy egyszerű AzureML kísérlet létrehozásához, és üzembe helyezés egy webszolgáltatásból. A web service veszi, a bemeneti egy tetszőleges szöveget tartalmazó oszlopot, és visszaadja egy funkciói ábrázolva az egészérték részt veszi figyelembe. Példa:

Szöveg | Kivonatolt szöveg
--- | ---
Ez a jó nap | 1 1 2 2 0 2 0 1

Első lépésként lehetőség, a böngészőben nyissa meg: [https://studio.azureml.net/](https://studio.azureml.net/) , és adja meg a hitelesítő adatait, és jelentkezzen be. Ezután hozzon létre egy új üres kísérlet.

![a sablonok kísérlet keresése](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Nevezze át **SimpleFeatureHashingExperiment**. Bontsa ki a **Mentett adatkészleteket** , és **a címjegyzék véleményezések az Amazon** húzza az alakzatot a kísérlet.

![egyszerű-szolgáltatás – a kivonatolás-kísérlet](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Bontsa ki a **Átalakítási** és **kezelését** , és az **Oszlopok kiválasztása az adatkészlet** húzza az alakzatot a kísérlet. Csatlakozás **az Amazon könyv véleményezések** **Jelölje ki az adatkészlet oszlopait**.

![Jelölje ki-oszlopok](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Kattintson az **Oszlopok kiválasztása az adatkészlet** és **oszlopkijelölő indítása** gombra, és jelölje ki a **Col2**. Kattintson a módosítások alkalmazásához be van jelölve.

![Jelölje ki-oszlopok](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Bontsa ki a **Szöveg Analytics** , majd a kísérlet **Szolgáltatás kivonatolás** húzza. **Jelölje ki az adatkészlet oszlopait** csatlakozni **szolgáltatás kivonatolás**.

![Csatlakozás a project-oszlopok](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Írja be a **Hashing bitsize** **3** . Ez a 8 (23) hoz létre oszlopokat.

![a kivonatolás bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Ezen a ponton érdemes tesztelje a kísérlet a **Futtatás** parancsra.

![futtatása](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Hozzon létre egy webszolgáltatásból

Most hozzon létre egy webszolgáltatásból. Bontsa ki a **Webes szolgáltatás** , és a **bemeneti** húzza az alakzatot a kísérlet. Csatlakozás **beviteli** **szolgáltatás kivonatolás**. **Kimeneti** is húzhatja a kísérlet. Csatlakozás **kimeneti** **szolgáltatás kivonatolás**.

![kimeneti-a-szolgáltatás – a kivonatolás](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Kattintson a **Közzététel webszolgáltatásból**.

![közzététel-webszolgáltatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Kattintson az **Igen gombra** a kísérlet közzététele.

![Igen-közzététel](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>A webszolgáltatás tesztelése

Az AzureML webszolgáltatás RSS (kérés/válasz szolgáltatás) és BES (köteg végrehajtás szolgáltatás) végpontok áll. Az RSS szinkron végrehajtás szolgál. BES aszinkron feladat végrehajtását szolgál. Ha tesztelni szeretné a webszolgáltatás az alábbi példa Python forrását, előfordulhat, töltse le és telepítse az Azure SDK Python (lásd: [Python telepítése](../python-how-to-install.md)).

Is szüksége lesz a **munkaterület**, **szolgáltatási**és **api_key** a kísérlet az alábbi példa forrást. A munkaterület és szolgáltatás található, a kísérlet a web service irányítópulton a **Kérés/válasz** vagy a **Köteg végrehajtás** elemre kattintva.

![Keresés-munkaterület-és-szolgáltatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

A web service irányítópulton a kísérlet gombra kattintva megtalálhatja a **api_key** .

![Keresés billentyűs api](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Teszt Erőforrásrekordok végpont

#####<a name="test-button"></a>Képteszt gomb

Tesztelje a Erőforrásrekordok végpont legegyszerűbben, a web service irányítópulton **tesztelése** gombra kattintva.

![teszt](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Írja be az **Ez jó nap** **col2**számára. Kattintson a bejelölést.

![adatok megadása](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Láthatja az alábbihoz hasonló

![minta – kimeneti](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Minta kódot.

Tesztelje a Erőforrásrekordok úgy, az ügyfél kódból. Ha az irányítópulton, és görgetéssel keresse meg az alsó **kérés/válasz** gombra kattint, látni fogja példakódot C#, Python és R. Is látni fogja az alábbi Erőforrásrekordok kérés, többek között a kért URI, élőfejek és az üzenet.

Ez az útmutató Python példa működő jeleníti meg. Szüksége lesz a **munkaterület**, **szolgáltatási**és **api_key** a kísérlet módosítására.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Teszt BES végpont
**Köteg végrehajtása** az irányítópult és görgetéssel keresse meg az alsó gombra. Példakódot látni fogja a C#, Python és R. A feladat elküldése, feladat indítása, az állapot vagy eredmények a projekt vagy feladat törlése BES kérések szintaxisa a következő is látni fogja.

Ez az útmutató Python példa működő jeleníti meg. Módosítsa a **munkaterület**, **szolgáltatási**és **api_key** a kísérlet szüksége. Ezenkívül módosítania kell a **tároló fióknév** **tároló fiókkulcs**és **tároló tároló nevét**. Végezetül szüksége lesz a **bemeneti fájl** helyét és a **kimeneti fájl**helyét.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
