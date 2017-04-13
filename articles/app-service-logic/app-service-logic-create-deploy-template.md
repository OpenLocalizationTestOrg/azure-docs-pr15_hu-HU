<properties
   pageTitle="Logika alkalmazás telepítési sablon létrehozása |} Microsoft Azure"
   description="Megtudhatja, hogy miként logika alkalmazás telepítési sablon létrehozása és használni szeretné a megjelenési kezelése"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Logika alkalmazás telepítési sablon létrehozása

Egy összefüggés-alkalmazás létrehozása után célszerű létrehozni a dokumentumot egy erőforrás-kezelő Azure sablonként. Ezzel a módszerrel egyszerűen telepítheti a logika alkalmazás bármely környezet vagy erőforráscsoport lehet szükség. Az erőforrás-kezelő sablonok bemutatása feltétlenül nézze meg a cikkek az [erőforrás-kezelő Azure-sablonok létrehozása](../resource-group-authoring-templates.md) és [telepíti az erőforrások Azure erőforrás-kezelő sablonok használatával](../resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Logika telepítési alkalmazássablon

Egy logikai alkalmazás három alapvető alkatrészek foglalja magában:

* **Logika alkalmazás erőforrás**. Ez az erőforrás összetevőjét, például a csomagot, helyét és a munkafolyamat-definíciót árak információkat tartalmazza.
* **Munkafolyamat-definíciót**. Ez a kód nézetben látható. A folyamat, és hogyan kell a motor hajtsa végre a lépéseket a definíció tartalmazza. Ez a `definition` tulajdonság a logika alkalmazás erőforrás.
* **Kapcsolatok**. Ezek a biztonságos tárolása az összekötő kapcsolatokat, például a kapcsolati karakterlánc és egy hozzáférési jogkivonat metaadatait külön erőforrásokat. Referencia a ezeket a összefüggés-alkalmazásban a `parameters` logika alkalmazás erőforrás szakaszban.

Minden meglévő logika alkalmazásai darabot megtekintheti például [Azure erőforrás Explorer](http://resources.azure.com)eszközzel.

Ahhoz, hogy a sablon egy logikai alkalmazását az erőforrás csoport telepítések használja, akkor kell definiálása az erőforrásokat, és szükség szerint paraméterezni. Például ha van egy fejlesztése, vizsgálat és üzemi környezetben üzembe helyezése, fog valószínűleg használni kívánt SQL-adatbázishoz különféle kapcsolati karakterláncot minden környezetben. Vagy, érdemes lehet a különböző előfizetések vagy az erőforrás csoportok belül telepíteni.  

## <a name="create-a-logic-app-deployment-template"></a>Logika alkalmazás telepítési sablon létrehozása

A szeretné, hogy egy érvényes logika alkalmazássablon telepítési legegyszerűbben a [Visual Studio Tools for logika alkalmazások](./app-service-logic-deploy-from-vs.md)használatát.  A Visual Studio eszközök több bármely előfizetést és a hely használható érvényes telepítési sablon készítése.

Néhány egyéb eszközöket is segítséget nyújt a logika alkalmazás telepítési sablonok létrehozásáról. Készíthet kézzel, ez azt jelenti, hogy az erőforrások már az itt tárgyalt paraméterek létrehozása szükség szerint használatával. Egy másik, hogy egy [logikai alkalmazás sablon készítő](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-modult használja. A Megnyitás-forrás modul először kiértékeli a logika alkalmazást, és minden kapcsolatot, hogy azt használja, és akkor a telepítéshez szükséges paraméterekkel sablon erőforrások hoz létre. Például alkalmazás a összefüggés-Azure Service Bus sorból üzenetet kap, és az adatokat felveszi Azure SQL-adatbázishoz, ha az eszköz megőrzi az összes az üzletifolyamat-tervező logika és paraméterezni az SQL és szolgáltatás Bus kapcsolati karakterláncot, hogy a üzembe állítható.

>[AZURE.NOTE] Kapcsolatok a logika App ugyanaz az erőforrás csoporton belül kell lennie.

### <a name="install-the-logic-app-template-powershell-module"></a>A logikai alkalmazás sablon PowerShell modul telepítése

Az a modul telepítése legegyszerűbben a [PowerShell gyűjtemény](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)keresztül parancsával `Install-Module -Name LogicAppTemplate`.  

Is telepítheti a PowerShell-modult kézzel:

1. Töltse le a [logika alkalmazás sablon készítő](https://github.com/jeffhollan/LogicAppTemplateCreator/releases)a legújabb verzióját.  
1. Bontsa ki a mappát, a PowerShell-modult mappában (általában `%UserProfile%\Documents\WindowsPowerShell\Modules`).

A modul használata az Access-szel bármilyen bérlői és az előfizetés jogkivonat, azt javasoljuk, hogy használni a [ARMClient](https://github.com/projectkudu/ARMClient) parancssori eszközt.  A [blog közzé](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ismerteti, hogy az ARMClient részletesebben.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Logika alkalmazás sablon készítése a PowerShell használatával

A PowerShell telepítése után egy sablont hozhat létre a következő parancs használatával:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`van az Azure előfizetés azonosítójával. Ebben a sorban először kap hozzáférési jogkivonat keresztül ARMClient, majd pipák keresztül, a PowerShell-parancsprogramot, hogy, és létrehozza a sablon egy JSON-fájlban.

## <a name="add-parameters-to-a-logic-app-template"></a>Paraméterek hozzáadása egy összefüggés-alkalmazássablon

Miután létrehozta a logika alkalmazássablon, továbbra is hozzáadásához vagy módosításához szükség lehet a paramétert. Például definíciójának tartalmaz egy erőforrás-azonosító Azure függvény vagy beágyazott munkafolyamat, amelyet használni szeretne egyetlen környezetben üzembe, is további erőforrások hozzáadása a sablonhoz és paraméterezni azonosítók, szükség szerint. Ugyanez igaz egyéni API-k és Swagger mutató hivatkozásokat telepítéshez használni az egyes erőforráscsoport várt végpontok.

## <a name="deploy-a-logic-app-template"></a>Egy logikai alkalmazássablon terjesztése

A sablon használatával eszközei, például a PowerShell, REST API-t, a Visual Studio megjelenés kezelése és az Azure portál sablonban telepítési tetszőleges számú telepítheti. Lásd: Ez a cikk további információt a [telepíti az erőforrások Azure erőforrás-kezelő sablonok használatával](../resource-group-template-deploy.md) kapcsolatos. Is azt javasoljuk, hogy a paraméter értékeket tároló [paraméter fájl](../resource-group-template-deploy.md#parameter-file) hoz létre.

### <a name="authorize-oauth-connections"></a>Engedélyezze a OAuth kapcsolatot

A telepítés után az logika alkalmazás működik végpont érvényes paraméterrel. Azonban OAuth kapcsolatok továbbra is kell engedélyezni kell, hogy egy érvényes jogkivonat készítése. Ez a logika a tervezőben megnyitása, és kattintson a kapcsolatok engedélyezése szerint teheti meg. Vagy ha automatizálni kívánt segítségével parancsfájl minden OAuth kapcsolat hozzájárul. Nincs példa parancsfájl GitHub a [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projekt keretében.

## <a name="visual-studio-release-management"></a>Visual Studio megjelenés kezelése

A leggyakoribb helyzet telepítéséhez és-környezet kezelése eszközével Visual Studio megjelenés kezelése, például az alkalmazás logika telepítési sablonnal. Visual Studio Team Services tartalmazza, hogy minden olyan fejlesztése felvehet, és engedje fel az folyamat [Üzembe Azure erőforráscsoport](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) tevékenység. [Egyszerű szolgáltatás](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) üzembe hitelesítéshez van szükség, és kattintson a Megjelenés definíció hozhat létre.

1. A Megjelenés-kezelés hozzon létre egy új definíciókat, jelölje be **üres** indítson egy üres definícióját.

    ![Hozzon létre egy új, üres meghatározása][1]   

1. Válassza ki a szükséges erőforrások. Ez valószínűleg lesz a összefüggés-alkalmazássablon létrehozott manuálisan vagy a Szerkesztés folyamat részeként.
1. Adja hozzá az **Azure erőforrás csoport telepítési** feladat.
1. Állítson be egy [egyszerű szolgáltatás](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/), és a sablon és a sablon paraméterek fájlokat hivatkozást.
1. A Megjelenés folyamat környezetben, automatikus próba vagy szükség szerint jóváhagyók lépéseinek kiépítésekor továbbra is.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
