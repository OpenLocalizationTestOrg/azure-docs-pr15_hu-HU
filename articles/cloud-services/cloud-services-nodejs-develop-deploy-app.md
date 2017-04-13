<properties
    pageTitle="NODE.js használatának megkezdését segítő útmutatót |} Microsoft Azure"
    description="Megtudhatja, hogy miként kell egy Azure felhőalapú szolgáltatás üzembe, és hozzon létre egy egyszerű Node.js webalkalmazást."
    services="cloud-services"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Építse fel és telepítse az Azure Felhőszolgáltatásba egy Node.js alkalmazás

> [AZURE.SELECTOR]
- [NODE.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

Ebből az oktatóanyagból megtudhatja, hogy miként hozzon létre egy egyszerű Node.js alkalmazást futtató az Azure felhőszolgáltatásában. Cloud Services építőelemei a scalable felhő alkalmazások Azure-ban. Lehetővé teszik, elkülönítésének és független kezelését és előtér- és háttérösszetevőre, az alkalmazás skála meg.  Cloud Services robusztus dedikált virtuális gép a szolgáltatója biztos, hogy az egyes szerepkör szükséges.

További információt a Felhőszolgáltatások, és hogy azok hogyan viszonyulnak és a webhelyek Azure virtuális gépeken futó című témakörben talál [Azure webhelyek, a Felhőszolgáltatások és a virtuális gépeken futó összehasonlító].

>[AZURE.TIP] Egy egyszerű webhely létrehozásához keresi? Ha a forgatókönyv csak egy egyszerű webhelyhez előtér jár, fontolja meg inkább [könnyű webalkalmazást]. Egyszerűen frissíthet egy felhőalapú szolgáltatásba, a webalkalmazásban megnő, és módosítása az igényeknek megfelelően alakíthatja.

Ebben az oktatóanyagban követve gyűjt a webes szerepkörbe belül is egyszerű webalkalmazás. A számítási irányító használatával az alkalmazás helyileg tesztelése lesz, majd azokat a PowerShell parancssori eszközökkel.

Az alkalmazás egy egyszerű "Helló, világ" alkalmazás, amely:

![A Helló, világ weblap megjelenítése webböngészőben][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Előfeltételek

> [AZURE.NOTE] Ebben az oktatóanyagban Azure PowerShell, amely használatához Windows használja.

- Telepítse és állítsa be a [Azure Powershell].
- Töltse le és telepítse az [Azure SDK.NET 2.7]. Válassza a telepítés beállítása:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Azure Felhőszolgáltatásba projekt létrehozása

A következő feladatokat Azure Felhőszolgáltatásában projekt létrehozása, valamint alapvető Node.js állványon:

1. **A Windows PowerShell** Futtatás rendszergazdaként; a **Start menüből** vagy **Kezdőképernyőjén**keressen rá **A Windows PowerShell**.

2. [Csatlakozás a PowerShell] használata az előfizetéshez.

3. Írja be a PowerShell-parancsmagot létrehozása a projekt létrehozása:

        New-AzureServiceProject helloworld

    ![Az eredmény a New-AzureService helloworld parancs][The result of the New-AzureService helloworld command]

    A **New-AzureServiceProject** parancsmag hoz létre egy alapszerkezetét közzététele egy Node.js alkalmazást, ha egy felhőalapú szolgáltatásba. Azure való közzétételhez szükséges konfigurációs fájl tartalmaz. A parancsmaggal a munkakönyvtárat is módosítja a címtárhoz a szolgáltatás.

    A parancsmaggal hozza létre a következő fájlokat:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** és **ServiceDefinition.csdef**: Azure-specifikus fájlok szükséges közzétételi az alkalmazás. További tudnivalókért olvassa el a [létrehozásának Azure üzemeltetett szolgáltatásának áttekintése]című témakört.

    -   **deploymentSettings.json**: Azure PowerShell telepítésének-parancsmagok által használt helyi beállításokat tárol.

4.  Írja be az új webhely szerepkör hozzáadása a következő parancsot:

        Add-AzureNodeWebRole

    ![A Hozzáadás-AzureNodeWebRole parancs][The output of the Add-AzureNodeWebRole command]

    A **Hozzáadás-AzureNodeWebRole** parancsmag létrehoz egy egyszerű Node.js alkalmazást. A konfiguráció kiegészítenie az új szerepkör **.csfg** és **.csdef** fájlokat is módosítja.

    > [AZURE.NOTE] Ha nem adja meg a szerepkör nevét, az alapértelmezett nevet használják. Megadhat egy nevet az első paraméterként parancsmagot:`Add-AzureNodeWebRole MyRole`

A Node.js alkalmazás határozza meg a fájl **server.js**, a webes szerep (**WebRole1** alapértelmezés szerint) könyvtárában található. A következő kódot:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Ez a kód alapjában véve ugyanaz, mint a [nodejs.org] webhelyén, a "Helló, világ" minta azzal a különbséggel a portszámot kiosztotta a felhőalapú környezetben használja.

## <a name="deploy-the-application-to-azure"></a>Az Azure alkalmazás telepítése

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Töltse le az Azure közzétételi beállítások

Az Azure alkalmazás telepítéséhez, le kell töltenie a közzétételi beállításokat Azure-előfizetéséhez.

1.  Futtassa a következő Azure PowerShell-parancsmagot:

        Get-AzurePublishSettingsFile

    Ez használja a böngészőben nyissa meg azt a Közzététel beállításai letöltési lapját. Jelentkezzen be Microsoft-Account kérheti. Ha igen, a fiókot, az Azure-előfizetéséhez társított használja.

    A letöltött profil ment egy könnyen hozzáférhető fájl helyét.

2.  Futtassa a következő parancsmagot a közzétételi profil letöltött importálása:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Után a közzétételi beállítások importálása, érdemes lehet törölni a letöltött .publishSettings fájlt, mert tartalmaz, amely lehetővé tévő valakinek a fiók eléréséhez.

### <a name="publish-the-application"></a>Az alkalmazás közzététele

Szeretné közzétenni, futtassa az alábbi parancsokat:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **-ServiceName** megadja a telepítéshez nevét. Ez lehet egy egyedi nevet, egyéb esetben a közzétételi folyamat sikertelen lesz. A **Get-Date** parancs rajzszögek a dátum/idő karakterlánc, amely kell tennie a név egyedi.

- **-Hely** adja meg, hogy az alkalmazás fog tárolni az az Adatközpont. A **Get-AzureLocation** parancsmag használatával elérhető adatközpontokkal listájának megtekintéséhez.

- **-Indítása** böngészőablakban nyílik meg, és a szolgáltatott szolgáltatás navigál telepítésének befejeződése után.

Miután létrejött a közzétételre, a válasz, a következőhöz hasonló jelennek meg:

![A közzététel-AzureService parancs][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Eltarthat néhány percig, amíg az alkalmazás üzembe helyezéséhez és válik elérhetővé, ha először közzé.

A telepítésének befejeződése után egy böngészőablakban nyissa meg, és nyissa meg azt a felhőalapú szolgáltatást.

![A helló világ lap; megjelenített böngészőablakban az URL-cím azt jelzi, hogy a lapon lévő Azure üzemelteti.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Az alkalmazás letöltése fut Azure.

A **Közzététel-AzureServiceProject** parancsmag hajtja végre az alábbi lépéseket:

1.  Létrehoz egy csomag telepítéséhez. A csomag az alkalmazás mappában lévő összes fájlt tartalmazza.

2.  Új **tároló fiók** hoz létre, ha nem létezik. Az Azure tárterület-fiókot a telepítés során a alkalmazáscsomag tárolására szolgál. Biztonságosan törölheti a tárterület-fiókot, amikor befejeződik a telepítés.

3.  Hoz létre egy új, **felhőalapú szolgáltatás** , ha egy nem létezik. Egy **felhőalapú szolgáltatásba** , amelyben az alkalmazás üzemelteti, amikor az Azure telepítve van a tároló. További tudnivalókért olvassa el a [létrehozásának Azure üzemeltetett szolgáltatásának áttekintése]című témakört.

4.  Azure teszi közzé a telepítőcsomag.


## <a name="stopping-and-deleting-your-application"></a>Leállítása és az alkalmazás törlése

Miután üzembe az alkalmazásokat, érdemes letiltáshoz, így elkerülheti a többletköltségek. Azure számlák webes szerepkör-példányok felhasznált server idő óránkénti. Kiszolgáló ideje elfogyasztott mennyiség megjelenítésére, amikor az alkalmazás telepíti, még akkor is, ha a példányok nem fut, és a leállítva állapotban vannak.

1.  A Windows PowerShell ablakában megakadályozhatja, hogy a szolgáltatás telepítési, a következő parancsmagot a az előző részben létrehozott:

        Stop-AzureService

    A szolgáltatás leállítása eltarthat néhány percig, amíg. Ha a szolgáltatás, megjelenik egy üzenet jelzi, hogy egyáltalán nem.

    ![A Leállítás-AzureService parancs állapotának][The status of the Stop-AzureService command]

2.  A szolgáltatás törléséhez hívja a következő parancsmagot:

        Remove-AzureService

    Amikor a rendszer kéri, adja meg az **Y** kattintva törölheti a szolgáltatást.

    A szolgáltatás törlése eltarthat néhány percig, amíg. A szolgáltatás törlése után megjelenik egy üzenet jelzi, hogy törölve lett-e a szolgáltatást.

    ![Az Eltávolítás-AzureService parancs állapotának][The status of the Remove-AzureService command]

    > [AZURE.NOTE] A szolgáltatás nem törlése a tárterület-fiókot a szolgáltatást az eredetileg közzétételekor létrehozott, és akkor is használt tárolására számlát kapni. Ha semmi másra az tárolására, érdemes törölheti azt.

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [Node.js Developer Center].

<!-- URL List -->

[Azure webhelyek, a Felhőszolgáltatások és a virtuális gépeken futó összehasonlítása]: ../app-service-web/choose-web-site-cloud-service-vm.md
[a legegyszerűbb web app használatával]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershell]: ../powershell-install-configure.md
[A .NET rendszerhez 2.7 Azure SDK]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Csatlakozás PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Azure szolgáltatott szolgáltatás létrehozásának áttekintése]: https://azure.microsoft.com/documentation/services/cloud-services/
[NODE.js Developer Center]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
