<properties
    pageTitle="Csatlakozás gépi tanulási webszolgáltatás |} Microsoft Azure"
    description="A C# vagy Python az Azure gépi tanulási webszolgáltatás-hitelesítés kulccsal csatlakozni."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016" 
    ms.author="garye" />


# <a name="connect-to-an-azure-machine-learning-web-service"></a>Az Azure gépi tanulási webszolgáltatás csatlakoztatása

Az Azure gépi tanulási fejlesztők számára egy webes szolgáltatás API-val, hogy az előrejelzések bemeneti adatok alapján, a valós idejű vagy köteg módban. Az előrejelzések készíthetnek és helyezhetnek üzembe a az Azure gépi tanulási webszolgáltatás Azure gépi tanulási Studio segítségével.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ha többet arról, hogy miként hozhat létre és gépi tanulási webszolgáltatás gépi tanulási Studio segítségével telepítése:

- A kísérlet létrehozása a gépi tanulási Studio az oktatóanyagot olvassa el a [az első kísérlet létrehozása](machine-learning-create-experiment.md)című témakört.
- A webszolgáltatás telepítéséről részletekért olvassa el [a Deploy gépi tanulási webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).
- Gépi tanulási kapcsolatos további tudnivalók az általános látogasson el a [Gép oktatóközpont dokumentációt](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure gépi tanulási webszolgáltatás ##

Az Azure gépi tanulási webszolgáltatással külső alkalmazás valós idejű modell pontozási gépi tanulási munkafolyamat kommunikál. A gépi tanulási webszolgáltatás hívása előrejelzési eredményt ad, külső alkalmazásba. Ahhoz, hogy a gépi tanulási webszolgáltatás hívása, adja meg az API kulcsot hoz létre, amikor rendszerbe állítják a szövegbevitel. A gépi tanulási webszolgáltatás többi, a népszerű architektúra választás programozási projektek webes alapul.

Azure gépi tanulási szolgáltatások két típusa van:

- Kérés-válasz szolgáltatás (Erőforrásrekordok) – egy kis késés, nagyon méretezhető szolgáltatása, amely egy csatolófelület, az állapot nélküli levő létrehozta és telepítette a gépi tanulási Studio.
- Köteg végrehajtás szolgáltatás (BES) –--aszinkron szolgáltatás, hogy egy köteg tartozik adatrekordok értékek.

Gépi tanulási webszolgáltatásokhoz kapcsolatos további tudnivalókért olvassa el a [Deploy gépi tanulási webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md)című témakört.

## <a name="get-an-azure-machine-learning-authorization-key"></a>Az Azure gépi tanulási engedélyezési kulcs beszerzése ##

Amikor rendszerbe állítják a kísérlet, a webszolgáltatás API kulcsot hoz létre. Számos helyről meghallgathatja a billentyűket.

## <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>A Microsoft Azure gépi tanulási webszolgáltatásokhoz portálról

Jelentkezzen be a [Microsoft Azure gépi tanulási Web Services](https://services.azureml.net) portál.

Az új gépi tanulási webszolgáltatás API kulcsa beolvasásához:

1. Az Azure gépi tanulási Web Services-portálon kattintson a **Web Services** a főmenü elemre.
2. Kattintson a webszolgáltatás, amelynek meg szeretné beolvasni a kulcsot.
3. A felső menüben kattintson a **felhasználás**.
4. Másolja a vágólapra, és mentse az **Elsődleges kulcs**.


A klasszikus gépi tanulási webszolgáltatás API billentyűjét beolvasásához:

1. Az Azure gépi tanulási Web Services-portálon kattintson a **Klasszikus webszolgáltatásokhoz** a főmenü elemre.
2. Kattintson a webes szolgáltatás, amellyel dolgozik.
3. Kattintson a kulcs lekérdezni kívánt végpontot.
3. A felső menüben kattintson a **felhasználás**.
4. Másolja a vágólapra, és mentse az **Elsődleges kulcs**.

## <a name="classic-web-service"></a>Klasszikus webszolgáltatás ##

 Egy kulcsot klasszikus webszolgáltatás lekérése gépi tanulási Studio vagy az Azure-portálra.

### <a name="machine-learning-studio"></a>Gépi tanulási Studio ###

1. Gépi tanulási Studio kattintson a bal oldali **WEBSZOLGÁLTATÁSOKHOZ** .
2. Kattintson egy webszolgáltatásból. Az **API-ja kulcs** van az **IRÁNYÍTÓPULT** lapon.

### <a name="azure-portal"></a>Azure portál ###

1. Kattintson a bal oldalon a **Gépi TANULÁSI** .
2. Kattintson a munkaterületre, ahol a webszolgáltatás is található.
3. Kattintson a **WEBES szolgáltatások**.
4. Kattintson egy webszolgáltatásból.
5. Kattintson a végpontok. A "API BILLENTYŰT" nem működik a jobb alsó sarokban.

## <a id="connect"></a>Gépi tanulási webszolgáltatás csatlakoztatása

Gépi tanulási webszolgáltatás bármilyen értékű programnyelv, amely támogatja a HTTP-összehívások és a visszajelzések használatával csatlakozhat. Példák a C#, Python és R weblapról gépi tanulási szolgáltatás súgó megtekintése.

**Gépi tanulási API segítségével** Gépi tanulási API súgó jön létre, amikor rendszerbe állítják egy webszolgáltatásból. Lásd: [Azure gépi tanulási útmutató – a webes szolgáltatás üzembe](machine-learning-walkthrough-5-publish-web-service.md).
Gépi tanulási API segítségével webszolgáltatás előrejelzése adatait tartalmazza.

2. Kattintson a webes szolgáltatás, amellyel dolgozik.
3. Kattintson a végpontra, amelynek meg szeretné tekinteni az API-súgó lapon.
3. A felső menüben kattintson a **felhasználás**.
3. **API súgó lapon** kattintson a kérelem-válasz vagy a köteg végrehajtás végpontok.

**Nézet gépi tanulási API segítségével egy új webhely szolgáltatás**

A Web Services portál tanulási Azure gépen:

1. **WEBSZOLGÁLTATÁSOK** a menü felső.
2. Kattintson a webszolgáltatás, amelynek meg szeretné beolvasni a kulcsot.

Kattintson a **felhasználás** az URL-címe beszerzése a kérelem-Reposonse és köteg végrehajtás szolgáltatások példakódot a C#, R és Python.

Kattintson a **Swagger API** alapú Swagger dokumentációjában, hogy a megadott URL-címe hívását API-khoz eléréséhez.

### <a name="c-sample"></a>C# minta ###

Gépi tanulási webszolgáltatás csatlakozhat egy **HttpClient** ScoreData áthaladó használja. ScoreData egy FeatureVector-n többdimenziósak vektoros numerikus szolgáltatás, amely a ScoreData tartalmazza. A gépi tanulási szolgáltatás hitelesíteni API kulccsal.

Gépi tanulási webszolgáltatás csatlakozik, a **Microsoft.AspNet.WebApi.Client** NuGet csomagot kell telepíteni.

**Visual Studio Microsoft.AspNet.WebApi.Client NuGet telepítése**

1. A letöltés adatkészlet UCI közzététele: felnőtt 2 osztály adatkészlet webszolgáltatásból.
2. Kattintson az **eszközök** > **NuGet csomag Manager** > **Csomag kezelője konzol**.
2. Válassza a **telepítés-csomag Microsoft.AspNet.WebApi.Client**.

**A kód mintát szeretné futtatni**

1. Közzététel "minta 1: Töltse le a adatkészlet UCI: 2 felnőtt osztály adatkészlet" kísérlet, a gépi tanulási minta gyűjtemény része.
2. Rendelje hozzá a kulccsal apiKey webszolgáltatásból. Lásd: a **Get-Azure gépi tanulási engedélyezési billentyűt** a fenti.
3. Rendelje hozzá a kérelem URI a serviceUri.


### <a name="python-sample"></a>Python minta ###

Gépi tanulási webszolgáltatás csatlakozik, használja a ScoreData áthaladó **urllib2** tárat. ScoreData egy FeatureVector-n többdimenziósak vektoros numerikus szolgáltatás, amely a ScoreData tartalmazza. A gépi tanulási szolgáltatás hitelesíteni API kulccsal.


**A kód mintát szeretné futtatni**

1. Üzembe "1 minta: Töltse le a adatkészlet UCI: 2 felnőtt osztály adatkészlet" kísérlet, a gépi tanulási minta gyűjtemény része.
2. Rendelje hozzá a kulccsal apiKey webszolgáltatásból. Című **az Azure gépi tanulási engedélyezési kulcs első** elején olvashatók.
3. Rendelje hozzá a kérelem URI a serviceUri.
