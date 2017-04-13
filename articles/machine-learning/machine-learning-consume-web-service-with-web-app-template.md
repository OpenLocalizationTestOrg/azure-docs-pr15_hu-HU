<properties
    pageTitle="A web app sablonnal gépi tanulási webszolgáltatás felhasználása |} Microsoft Azure"
    description="Microsoft Azure piactéren web app sablon használatával az Azure gépi tanulási prediktív webszolgáltatás felhasználni."
    keywords="webszolgáltatás operationalization, REST API-t, a gép oktatás"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Az Azure gépi tanulási webszolgáltatás web app sablonnal felhasználása

>[AZURE.NOTE] Ez a témakör ismerteti a klasszikus webszolgáltatás alkalmazandó technikákat alkalmaz. 

Egyszer által fejlesztett a cserélendő modell, és gépi tanulási Studio segítségével Azure webes szolgáltatásainak telepítése vagy például R vagy Python eszközök segítségével érheti el a operationalized modell REST API-t használja.

Számos módszert, amellyel felhasználni a REST API-val és a webszolgáltatás elérésére. Az alkalmazások írhat például a C#, R vagy Python jön létre meg, amikor telepítette (a web service irányítópulton gépi tanulási Studio API súgója lapon érhető el) webszolgáltatás minta kód használatával. Másik lehetőségként használhatja a Microsoft Excel mintamunkafüzet (a web service irányítópulton Studio is elérhető) jött létre.

De a leggyorsabban és legegyszerűbben úgy a webszolgáltatás elérése révén a webes alkalmazás a a [Microsoft Azure Web App piactéren](https://azure.microsoft.com/marketplace/web-applications/all/)elérhető sablonokat.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Az Azure gépi tanulási a Web App-sablonok

A web app elérhető sablonok a Microsoft Azure piactéren található hozhat létre egyéni webalkalmazás tudja a webszolgáltatás bemeneti adatok és a várt eredményeket. Az összes kell tennie a web app hozzáférési engedély a webes szolgáltatás és az adatok, és a sablont tartalmaz, a többi.

Két sablonok állnak rendelkezésre:

- [Azure Machine Learning kérés válasz szolgáltatás Web-Alkalmazássablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Azure Machine Learning köteg végrehajtás szolgáltatási Web App sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Minden sablon létrehoz egy minta ASP.NET-alkalmazást, az API URI és a billentyűk segítségével esetében a webszolgáltatás és üzembe helyezi a szolgáltatást, az Azure webhelyként. A kérés-válasz szolgáltatás (Erőforrásrekordok) sablon hoz létre, amely lehetővé teszi, hogy az adatok egyetlen sor küldhet a webszolgáltatás egyetlen eredmény webalkalmazást. A köteg végrehajtás szolgáltatás (BES) sablon hoz létre, amely lehetővé teszi, hogy sok adatsort kell több eredményt küldhet webalkalmazást.

Ezek a sablonok használata nélkül coding van szükség. Csak ki az API URI és billentyűt, és a sablont, hozza létre az alkalmazást.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>A kérés-válasz szolgáltatás (Erőforrásrekordok) sablon használata

Ha már telepítette a webes szolgáltatás, kövesse az alábbi lépésekkel Erőforrásrekordok web app sablon használatára, a következő ábrán látható módon.

![A folyamat Erőforrásrekordok webhely sablon használata][image1]

1. A gépi tanulási Studióban nyissa meg a **Szolgáltatások webes** fülre, és nyissa meg az a webszolgáltatás el szeretne érni. Másolja az **API-ja kulcs** alatt billentyűt, és mentse.

    ![API-ja kulcs][image3]

2. Nyissa meg a **KÉRÉS/válasz** API súgó lapot. A Súgó **kérése**csoportban lap tetején a **Kérelem URI** értéket másolja, és mentse. Ez az érték fog kinézni:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![Kérés URI][image4]

3. [Azure portál](https://portal.azure.com), **Jelentkezzen be**, kattintson a megnyitásához, keresse meg és választó **Azure Machine Learning kérés-válasz szolgáltatás Web App** **Új**, majd kattintson a **Létrehozás**gombra. 

    - Adjon meg egy egyedi nevet a web App alkalmazásban. A web app URL-címe lesz ez a név követ `.azurewebsites.net.` például`http://carprediction.azurewebsites.net.`

    - Válassza az Azure előfizetés és szolgáltatások, amelyek alapján a webes szolgáltatás fut.

    - Kattintson a **létrehozása**gombra.

    ![Webalkalmazás létrehozása][image5]

4. Befejeződése Azure a web app telepítése után kattintson az **URL-CÍMÉT** a web app beállításai lap Azure-ban, vagy írja be az URL-cím egy webböngészőben. Ha például`http://carprediction.azurewebsites.net.`

5. Amikor első alkalommal futtatja a web app azt fogja kérni az **API-bejegyzés URL-cím** és az **API billentyűt**.
Írja be a korábban mentett értékeket:
    - A **kérés URI** **API-bejegyzés URL-CÍMÉT** az API súgó lapról
    - **API -ja kulcs** az **API -ja kulcs**web service irányítópultról.

    Kattintson a **Küldés**gombra.

    ![Adja meg a bejegyzés URI és az API-ja kulcs][image6]

6. A web app jeleníti meg a **Web App konfigurációs** tartalmazó lapon az aktuális webhely-beállításokkal. Itt módosíthatja a web app által használt beállításait.

    > [AZURE.NOTE] Ezek a beállítások csak módosítása őket a webalkalmazás. Nem változik, akkor a webszolgáltatás az alapértelmezett beállítások. Például ha módosítja a **Leírás** Itt azt nem leírásának a módosítása jelenik meg a webes szolgáltatás irányítópult gépi tanulási Studio alkalmazásban.

    Amikor elkészült, kattintson a **módosítások mentéséhez**, és kattintson az **Ugrás a kezdőlapra**.

7. A Kezdőlap lapon adhatja meg a értékeket szeretne küldeni a webszolgáltatás, kattintson a **Küldés gombra**, és visszatér az eredményt.

Ha vissza szeretne térni az **beállítása** lapon, nyissa meg a `setting.aspx` a web app oldalát. Példa: `http://carprediction.azurewebsites.net/setting.aspx.` kérni fogja adja meg újra az API-ja kulcs – van szüksége, amely elérése a lapra, és frissítse a beállításokat.

Leállítása, indítsa újra, vagy törölje a web app, mint bármely más webalkalmazás az Azure portálon. Mindaddig, amíg fut tallózással keresse meg az otthoni URL-címe, és írja be az új értékeket.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>A köteg végrehajtás szolgáltatás (BES) sablon használata

A is használhatja a BES web app sablon ugyanúgy, mint a Erőforrásrekordok sablont, azzal a különbséggel, hogy a web App alkalmazásban létrehozott lehetővé teszi az adatok több sor elküldése és több kapni.

Az eredmények köteg végrehajtás webszolgáltatásból tárolja az Azure tároló tároló; a bemeneti értékek származhatnak Azure tároló vagy a helyi fájl.
Igen szüksége van egy Azure tároló tároló tartása a web app eredménye, és a bemeneti adatok felkészítése kell.

![A folyamat BES webhely sablon használata][image2]

1. Hajtsa végre ugyanezt az eljárást, kivéve a Erőforrásrekordok sablon, mint a BES webalkalmazás létrehozásához:
    - Juthat a **Kérelem URI** **KÖTEG végrehajtás** API súgó esetében a webszolgáltatás.
    - Nyissa meg az [Azure Machine Learning köteg végrehajtás szolgáltatási Web App sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) nyissa meg a Microsoft Azure piactéren található BES sablon, és kattintson a **Webalkalmazás létrehozása**elemre.

2. Adja meg, amelyre az eredmények tárolt, írja be a cél tároló adatokat web app kezdőlapján. Amennyiben a web app szerezhet be a bemeneti értékek, egy helyi fájl vagy egy Azure tároló tároló is megadhatja.
Kattintson a **Küldés**gombra.

    ![Tárterület-információ][image7]

A web app jeleníti meg a feladat állapota weblapra.
Amikor befejezte a feladatot fog hol található az eredményeket az Azure blob-tárolóhoz fordítani. Akkor is, hogy az eredmények letöltése helyi fájlként.

## <a name="for-more-information"></a>További információ

További tudnivalók...

- egy gépi tanulási kísérlet létrehozni a gépi tanulási Studio, lásd: [az első kísérlet Azure gépi tanulási Studio létrehozása](machine-learning-create-experiment.md)

- a gépi tanulási telepítéséről, egy webszolgáltatásból kipróbálni című [Deploy az Azure gépi tanulási webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md)

- a webszolgáltatás elérésének egyéb módjai megtudhatja, [hogy miként az Azure gépi tanulási webszolgáltatás felhasználása](machine-learning-consume-web-services.md)


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
