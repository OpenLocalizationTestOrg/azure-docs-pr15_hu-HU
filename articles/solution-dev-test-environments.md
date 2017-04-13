<properties
   pageTitle="Fejlesztés és a próba-környezetben |} Microsoft Azure"
   description="Megtudhatja, hogy miként Azure erőforrás-kezelő sablonok segítségével gyorsan egységes létrehozása és törlése fejlesztési és tesztelési környezetben."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="01/22/2016"
   ms.author="tomfitz"/>

# <a name="development-and-test-environments-in-microsoft-azure"></a>Fejlesztés és tesztelése a Microsoft Azure környezetben

Egyéni alkalmazások általában több fejlesztés és termelési telepítés előtt tesztelés környezetben van telepítve. A helyszíni környezetekkel létrehozásukkor számítógépes erőforrások beszerzett vagy az egyes alkalmazások környezetben kiosztva. A környezetben gyakran tartalmaznak, több fizikai vagy virtuális gépeken futó az egyes konfigurációk manuálisan telepített vagy az összetett automatizálási parancsfájlokat. Telepítések gyakran órára és a várttól eltérően működik konfigurációk támogatása környezetekben.

## <a name="scenario"></a>Eset ##

Akkor kiépítése fejlesztési és a Microsoft Azure-ban próba-környezetben, amikor csak fizet az erőforrások használja.  Ebből a cikkből megtudhatja, hogy milyen gyorsan és egységes létrehozása, kezelése, és törölheti fejlesztés és tesztelje a környezetek az erőforrás-kezelő Azure-sablonok és paraméter fájlok, ahogy alább látható.

![Eset](./media/solution-dev-test-environments/scenario.png)

Három fejlesztés és tesztelés környezetekben a fent látható.  Minden legyen webalkalmazás és SQL adatbázis erőforrásokat, amelyek a sablonfájlt megadott.  Az alkalmazás és az adatbázis minden környezetben azoknak a különböző, és egyedi paraméter fájlok környezetben a megadott.  

Ha nem ismeri a Azure erőforrás-kezelő fogalmak, javasolt, hogy a cikket elolvasva előtt olvassa el az [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md) cikket.

Érdemes először lépjen a jelen cikk lépéseit felsorolt olvassa fel a hivatkozott cikkek gyorsan szükséges az egyes élmény Azure erőforrás-kezelő sablonok használata nélkül. Magára a műveletek elvégzéséhez egyszer, után is a legtöbb merültek fel az első kérdésére választ kaphat időben keresztül kísérletezés további lépéseket, és a hivatkozott cikkek elolvasásával.

## <a name="plan-azure-resource-use"></a>Azure erőforrások használatának megtervezése
Ha az alkalmazás már magas szintű látványterv, állíthatók be:

- Az alkalmazás melyik Azure erőforrások fogja tartalmazni. Előfordulhat, hogy az alkalmazás összeállítása, és üzembe meg az Azure Web App az SQL Azure-adatbázisból.  Előfordulhat, hogy az alkalmazás virtuális gépeken futó PHP és MySQL vagy IIS és az SQL Server vagy más összetevő generál. Az [Azure alkalmazás szolgáltatás, a Felhőszolgáltatások, és a virtuális gépeken futó összehasonlító]( app-service-web/choose-web-site-cloud-service-vm.md) cikk segít eldönteni, hogy a mely Azure erőforrásokat, érdemes lehet csatlakozást az alkalmazás.
- Milyen szolgáltatás szintű követelmények például az elérhetőség, biztonságot és skálát, amely megfelel-e az alkalmazást.

## <a name="download-an-existing-template"></a>Meglévő sablon letöltése
Egy erőforrás-kezelő Azure-sablon határozza meg, hogy az alkalmazás segítségével Azure erőforrásokat. Több sablont már létezik, hogy közvetlenül az Azure-portálon üzembe vagy letöltheti, módosítása, és mentse a forrás rendszerben az alkalmazás kódját.  Kövesse az alábbi lépéseket követve töltse le a meglévő sablon.

1. Tallózással keresse meg a meglévő sablonok az [Azure quickstart útmutató sablonok](https://github.com/Azure/azure-quickstart-templates/) GitHub tárban tárolnak. A lista megjelenik egy "[201 web-alkalmazás – sql-adatbázis-](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)" mappát. Mivel sok egyéni alkalmazások tartalmaz egy webalkalmazás és SQL-adatbázissal, ezt a sablont használják a jelen cikk további részében példaként segít megérteni a sablonok használatáról. Ez a cikk elmagyarázni teljesen minden ezzel a sablonnal hoz létre, és konfigurálja túlra, de ha a szervezet tényleges környezetekben létrehozásához használandó, akkor érdemes ismerje a [rendelkezést tartalmazó SQL-adatbázis webalkalmazást](app-service-web/app-service-web-arm-with-sql-database-provision.md) témakör elolvasásával.
Megjegyzés: Ez a cikk a [201 web-alkalmazás – sql-adatbázis -](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) sablon December 2015-verzióhoz készült. A hivatkozások alatt, mutasson a sablon és paraméter fájlokat, hogy a sablon verzióját.
2. Kattintson a [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json) fájl tartalmának megtekintéséhez 201 web-alkalmazás – sql-adatbázis-mappában. Ez az erőforrás-kezelő Azure sablonfájl. 
3. A megjelenítési mód használata esetén kattintson a "[nyers](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json)" gombra. 
4. Az egérrel jelölje be a fájl tartalmát, és a "TestApp1-Template.json." nevű-fájlként mentheti őket a számítógépen 
5. Vizsgálja meg a sablon tartalmát, és figyelje meg a következőket:
 - **Források** részben: Ebben a szakaszban határozza meg, ez a sablon alapján készült Azure erőforrások típusait. Más erőforrás-típusok között ezzel a sablonnal [Azure Web App](app-service-web/app-service-web-overview.md) és az [Azure SQL-adatbázis](sql-database/sql-database-technical-overview.md) -erőforrások hoz létre. Ha szeretné futtatni, és kezelheti a webes és virtuális gépeken futó SQL-kiszolgálók, a "[iis-2vm-sql-1vm](https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm)" vagy "[lámpa-app](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)" sablonokat is használhatja, de ez a cikk utasításait a [201-webes-alkalmazás – sql-adatbázis](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) sablon alapján.
 - **Paraméterek** rész: Ebben a szakaszban határozza meg a paraméterek, amely az egyes erőforrásokhoz konfigurálható. A paraméterek a sablonban meghatározott részét "defaultValue" tulajdonság tartozik, míg mások nem. Azure erőforrások sablonnal telepítésekor az összes paramétert, amely nincs a sablonban megadott defaultValue tulajdonságok adjon meg értéket.  Ha nem adjon meg értéket a paraméterek defaultValue tulajdonságok, a sablonban defaultValue paraméterhez megadott érték használják.

Sablon határozza meg, hogy mely Azure erőforrások jönnek létre, és a paraméterek az egyes erőforrások konfigurálható. Sablonok és tervezéséről, saját maga által olvasása a [Gyakorlati tanácsok az Azure erőforrás-kezelő sablonok tervezése](best-practices-resource-manager-design-templates.md) a cikk további információt talál.

## <a name="download-and-customize-an-existing-parameter-file"></a>Töltse le és meglévő paraméter fájl testreszabása

Bár létrehozásakor célszerű az *azonos* Azure erőforrások létrehozott minden környezetben, valószínűleg érdemes az erőforrások *különböző* minden környezetben beállításait.  Ez az, hogy honnan származnak a paraméter fájlokat. Az alábbi lépéseket követve minden környezetben egyedi értékeket tartalmazó paraméter fájlokat hozhat létre.   

1. A 201 web-alkalmazás – sql-adatbázis-mappa a [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json) fájl tartalmának megtekintéséhez. Ez a paraméter fájlt a sablonfájlhoz az előző szakaszban a mentett. 
2. A megjelenítési mód használata esetén kattintson a "[nyers](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json)" gombra. 
3. Az egérrel jelölje be a fájl tartalmát, és mentheti őket az alábbi névvel rendelkező számítógépen három külön fájlok:
 - TestApp1 – paraméterek-Development.json
 - TestApp1 – paraméterek-Test.json
 - TestApp1 – paraméterek-előtti-Production.json

3. A fejlesztői környezet paraméter fájlt, a 3 helyett az értékeket, a jobb oldalon a **Paraméterek** alatt felsorolt *értékek* jobb oldalán a paraméterértékeket, a fájlban felsorolt bármilyen szöveget vagy a JSON-szerkesztő használata esetén szerkesztése: 
 - **siteName**: *TestApp1DevApp*
 - **hostingPlanName**: *TestApp1DevPlan*
 - **siteLocation**: *Központi Amerikai Egyesült Államok*
 - **kiszolgálónév**: *testapp1devsrv*
 - **serverLocation**: *Központi Amerikai Egyesült Államok*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *cserélje ki a jelszó*
 - **adatbázisnév**: *testapp1devdb*

4. A szöveg vagy a JSON-szerkesztőben szerkesztése a környezet paraméter tesztfájl lépésben létrehozott 3 cseréje a jobbra lévő paraméterértékeket az *értékeket* a fájlban felsorolt értékeket az alábbi **Paraméterek** jobbra látható:
 - **siteName**: *TestApp1TestApp*
 - **hostingPlanName**: *TestApp1TestPlan*
 - **siteLocation**: *Központi Amerikai Egyesült Államok*
 - **kiszolgálónév**: *testapp1testsrv*
 - **serverLocation**: *Központi Amerikai Egyesült Államok*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *cserélje ki a jelszó*
 - **adatbázisnév**: *testapp1testdb*

5. Szöveg vagy a JSON-szerkesztő használata esetén szerkesztése a 3 létrehozott előtti gyártási paraméter fájlt.  Mi az alábbi cserélje ki a fájl teljes tartalmát:

        {
          "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion" : "1.0.0.0",
          "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "replace with your password"
        },
        "databaseName" : {
          "value" : "testapp1preproddb"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PreProdPlan"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprodsrv"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProdApp"
        },
        "sku" : {
          "value" : "Standard"
        },
            "requestedServiceObjectiveName" : {
              "value" : "S1"
        }
          }
        }

A fenti előtti termelési paraméterek fájlban a **termékváltozat** és **requestedServiceObjectiveName** paraméterek voltak *hozzáadott*, mivel ezek nem vette fel a fejlesztés és a próba paraméterek fájlokban. Ennek oka, hogy a sablon paraméterértékek megadott alapértelmezett értékek vannak, és a fejlesztés és próba környezetben, az alapértelmezett értékeket fogja használni, de a előtti gyártási környezet nem alapértelmezett értékének ezeket a paramétereket használnak.

Nem az alapértelmezett értékeket használják ezeket a paramétereket az előzetes éles üzemi környezetben oka az értékek, előfordulhat, hogy jobban szereti a gyártási környezetben, azok is vizsgálni paraméterértékek tesztelése.  Ezek a paraméterek összes az [Office Web App-csomagok](https://azure.microsoft.com/pricing/details/app-service/)Azure vagy **termékváltozat** Azure [SQL-adatbázis](https://azure.microsoft.com/pricing/details/sql-database/), és az alkalmazás által használt **requestedServiceObjectiveName** vonatkoznak.  Különböző SKU és objektív szolgáltatásnevek különböző költségek és a szolgáltatások van, és másik szolgáltatáscsaládba szintű mértékek támogatja.

Az alábbi táblázat felsorolja az alapértelmezett értékeket, a sablon és az értékeket a előtti termelési paraméterek fájlt az alapértelmezett értékek helyett használt megadott paraméterértékek.

| Paraméter | Alapérték | Paraméter fájl |
|---|---|---|
| **Raktári szám** | Ingyenes | Normál |
| **requestedServiceObjectiveName** | S0 | S1 |

## <a name="create-environments"></a>Környezet létrehozása
Azure összes erőforrás létre kell hozni az [Azure erőforráscsoport](azure-resource-manager/resource-group-overview.md)belül. Az erőforrás csoportok lehetővé teszi csoport Azure erőforrások, így azok együttesen kezelhető.  [Engedélyek](./active-directory/role-based-access-built-in-roles.md) rendelhetők erőforrás csoportokhoz, hogy a szervezeten belül meghatározott személyeket is létrehozása, módosítása, törlése vagy őket, és a bennük lévő erőforrások megtekintése.  Értesítések és az erőforrások erőforráscsoport a számlázási információkat megtekintheti az [Azure-portálon](https://portal.azure.com). Erőforrás-csoportok Azure [régió](https://azure.microsoft.com/regions/)hozhatók létre.  Ez a cikk a központi USA-beli tartományban lévő összes erőforrás jönnek létre. Indításakor létrehozása az aktuális környezetben, a régió, amely a leginkább megfelel fogja választja. 

Az alábbi módszerek bármelyikével környezetben erőforrás-csoportok létrehozása.  Minden módszer az azonos így éri el.

###<a name="azure-command-line-interface-cli"></a>Azure parancssor (CLI)

Győződjön meg arról, hogy a CLI van [telepítve](xplat-cli-install.md) a vagy a Windows operációs rendszer X, vagy Linux számítógépet, és amelyekhez Önnek már [csatlakozott](xplat-cli-connect.md) az [Azure Active Directory-fiókhoz](./active-directory/active-directory-how-subscriptions-associated-directory.md) (más néven egy munkahelyi vagy iskolai fiók) Azure-előfizetéséhez. A parancssorból CLI írja be a fejlesztői környezet az erőforrás-csoport létrehozása az alábbi parancsot.

    azure group create "TestApp1-Development" "Central US"

A parancs az alábbi ad vissza, ha sikerül:

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Az erőforrás-tesztkörnyezetben csoport létrehozásához írja be az alábbi parancsot:

    azure group create "TestApp1-Test" "Central US"

Az előzetes éles üzemi környezetben az erőforrás-csoport létrehozásához írja be az alábbi parancsot:

    azure group create "TestApp1-Pre-Production" "Central US"

###<a name="powershell"></a>A PowerShell

Győződjön meg arról, hogy Azure PowerShell 1.01 vagy magasabb telepítette egy Windows rendszerű és az előfizetés [telepítése és konfigurálása az Azure PowerShell](powershell-install-configure.md) cikkben ismertetett módon kapcsolt az [Azure Active Directory-fiókhoz](./active-directory/active-directory-how-subscriptions-associated-directory.md) (más néven egy munkahelyi vagy iskolai fiók). A PowerShell parancssorból írja be a fejlesztői környezet az erőforrás-csoport létrehozása az alábbi parancsot.

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

A parancs az alábbi ad vissza, ha sikerül:

    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

Az erőforrás-tesztkörnyezetben csoport létrehozásához írja be az alábbi parancsot:

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

Az előzetes éles üzemi környezetben az erőforrás-csoport létrehozásához írja be az alábbi parancsot:

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

###<a name="azure-portal"></a>Azure portál

1. Jelentkezzen be az [Azure portal](https://portal.azure.com) (más néven munkahelyi vagy iskolai) [Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) -fiókkal. Kattintson az új--> kezelés--> erőforráscsoport és megadása "TestApp1 fejlesztési" erőforrás csoport neve listában, jelölje ki azt az előfizetést, valamint "Központi US" erőforrás csoport helye listában a következő ábrán látható módon.
   ![Portál](./media/solution-dev-test-environments/rgcreate.png)
2. Kattintson az erőforrás csoport létrehozásához a Létrehozás gombra.
3. Kattintson a Tallózás gombra, görgessen lefelé a listában, az erőforrás csoportokra, majd kattintson az erőforrás csoportok alább látható módon.
   ![Portál](./media/solution-dev-test-environments/rgbrowse.png) 
4. Erőforrás csoportok parancsra kattintást követően akkor láthatja az erőforrás-csoportoknak lap az új erőforráscsoport a.
   ![Portál](./media/solution-dev-test-environments/rgview.png)
5. A TestApp1-próba létrehozása, és TestApp1-előtti-gyártási erőforráscsoport fenti TestApp1 fejlesztési erőforráscsoport létrehozott ugyanúgy.

##<a name="deploy-resources-to-environments"></a>Erőforrások üzembe környezetekbe

Az erőforrás-csoportokat a sablonfájlt a megoldáshoz és paraméter-fájlok használata az alábbi módszerek valamelyikével környezetben környezetben üzembe Azure erőforrásokat.  Az azonos így mindkét módszerhez éri el.

###<a name="azure-command-line-interface-cli"></a>Azure parancssor (CLI)

A parancssorból CLI írja be a parancsot az alábbi telepítéshez használni erőforrások erőforráscsoport hoz létre a fejlesztői környezet [elérési út] helyettesítve elérési útját mentett az előző lépéseket.

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

"A telepítés befejezéséhez Várakozás" üzenet jelenik meg néhány percig, miután a parancs ad eredményül a következő Ha sikerül:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1DevApp
    data:    hostingPlanName                String        TestApp1DevPlan
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1devsrv
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1devdb
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

Ha nem sikerül a parancsot, hibaüzenetek megoldani, és próbálkozzon újra.  Gyakori problémák, amelyek nem kötődnek Azure erőforrás elnevezési kényszerek paraméterértékeket használják. További hibaelhárítási tippek [Hibaelhárítás erőforrás csoport telepítések Azure-ban](./resource-manager-troubleshoot-deployments-cli.md) című témakörben találhatók.

A parancssorból CLI írja be a parancsot az alábbi telepítéshez használni erőforrások erőforráscsoport hoz létre a tesztkörnyezetben [elérési út] helyettesítve elérési útját mentett az előző lépéseket.

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

A parancssorból CLI írja be a parancsot az alábbi telepítéshez használni erőforrások erőforráscsoport hoz létre a előtti munkakörnyezetben [elérési út] helyettesítve elérési útját mentett az előző lépéseket.

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
###<a name="powershell"></a>A PowerShell

Azure PowerShell (1.01 vagy újabb verzió) parancssort írja be a parancsot az alábbi telepítéshez használni erőforrások erőforráscsoport hoz létre a fejlesztői környezet [elérési út] helyettesítve elérési útját mentett az előző lépéseket.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

Után megjelenik a parancssorablakba néhány percig, a parancs ad eredményül a következő Ha sikerül:

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1DevApp
                        hostingPlanName  String                     TestApp1DevPlan
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1devsrv
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1devdb
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  Ha nem sikerül a parancsot, hibaüzenetek megoldani, és próbálkozzon újra.  Gyakori problémák, amelyek nem kötődnek Azure erőforrás elnevezési kényszerek paraméterértékeket használják. További hibaelhárítási tippek [Hibaelhárítás erőforrás csoport telepítések Azure-ban](./resource-manager-troubleshoot-deployments-powershell.md) című témakörben találhatók.

  A PowerShell parancssorból írja be a parancsot az alábbi telepítéshez használni erőforrások erőforráscsoport hoz létre a tesztkörnyezetben [elérési út] helyettesítve elérési útját az előző lépést a mentett fájlok.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  A PowerShell parancssorból írja be a parancsot az alábbi telepítéshez használni erőforrások erőforráscsoport hoz létre a előtti munkakörnyezetben [elérési út] helyettesítve elérési útját az előző lépést a mentett fájlok.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

A sablon és a paraméterek fájlokat lehet verzióval és az alkalmazás kóddal tartani egy adatforrás rendszerben.  A fenti parancsfájlok és mentheti őket az kóddal parancsok is felszabadítani.

> [AZURE.NOTE] Azure a sablont közvetlenül [rendelkezést tartalmazó SQL-adatbázis webalkalmazást](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) témakörben "Üzembe az Azure" gombra kattintva telepítheti.  Előfordulhat, hogy megtalálta Ez hasznos, ha többet szeretne tudni a sablonok, de ezzel nem lehetővé teszi a szerkesztését, verzióját, és mentse a sablont, és a paraméter értéket az alkalmazás kóddal, így a további részletei ezzel a módszerrel nem vonatkozik a jelen cikkben.

## <a name="maintain-environments"></a>Naplók karbantartása: a környezetek
Egész fejlesztése a különböző környezetekben Azure erőforrások konfigurációs érvénytelenként módosítható szándékosan vagy véletlenül.  Az alkalmazás fejlesztési ciklus során léphetnek fel szükségtelen hibaelhárítás és a probléma megoldása.

1. A környezetek módosításához az [Azure portál](https://portal.azure.com)megnyitása. 
2. Jelentkezzen be ugyanazzal a fiókkal, mellyel a fenti lépéseket. 
3. A kép alatt, kattintson a Tallózás--> erőforrás csoportok (Előfordulhat, erőforrás-csoport megjelenítéséhez lefelé görgetve) szerint látható.
   ![Portál](./media/solution-dev-test-environments/rgbrowse.png)
4. Kattintás után a fenti képen az erőforrás-csoportok, az erőforrás-csoportoknak lap és a három erőforrás csoportok, az alábbi képen látható módon az előző lépésben létrehozott megjelenik. Kattintson a TestApp1 fejlesztési erőforráscsoport, és láthatja, hogy a lap, mely tartalmazza az erőforrásokat hozta létre a sablon egy előző lépésben elvégezte TestApp1 fejlesztési erőforrás környezetben.  A TestApp1DevApp Web App erőforrás törlése TestApp1DevApp kattintva a csoport TestApp1 fejlesztési erőforrás lap, és kattintson a TestApp1DevApp Web app lap a Törlés elemre kattintással.
   ![Portál](./media/solution-dev-test-environments/portal2.png)
5. Kattintson az "Igen" a portálon kéri, hogy Ön arról, hogy az erőforrás törölni szeretné, hogy amikor. A csoport TestApp1 fejlesztési erőforrás lap bezárása és újbóli megnyitása most jelennek meg azt a éppen csak törölt Web app nélkül.  Az erőforráscsoport tartalma most eltérő legyen. További kísérletezhet több erőforrások törlése több erőforrás-csoportokat, vagy akár az egyes erőforrások konfigurációs beállítások módosítása. Helyett az Azure portal segítségével erőforrás törlése egy erőforrás csoport, az [Eltávolítás-AzureResource](https://msdn.microsoft.com/library/azure/dn757676.aspx) PowerShell-parancsot sikerült használja, vagy a vagy a CLI "azure erőforrás törlése" parancs ugyanaz a feladat elvégzéséhez.
6. Úgy juthat az összes az erőforrások és a konfigurációs, amelyeknek el szeretné helyezni a állapotába az kell lennie az erőforrás-csoportokat, újból telepíteni az erőforrás csoportoknak a parancsaival azonos használt [környezetekben Deploy források](#deploy-resources-to-environments) szakaszában a környezetben, de cseréje "Deployment1" a "Deployment2."
7.  Ahogy a képen látható lépés: 4 TestApp1 fejlesztési lap összefoglaló szakaszának, látni fogja, hogy a Web app törölt az előző lépésben a portálon létezik, mint bármely más erőforrások: Előfordulhat, hogy nyilvánított törlése. Ha módosította az erőforrások konfigurációja, is ellenőrizheti, hogy azok van beállítva vissza értékeket a paraméter fájlok is. A környezetekben a Azure erőforrás-kezelő sablonok előnyei egyik, hogy könnyen újra telepítheti a környezetben, egy ismert állapotra bármikor.
8. Ha a szöveg csoportban az alábbi képen a "utolsó telepítési" parancsára kattint, megjelenik egy lap, amely mutatja az erőforráscsoport telepítési előzményeinek.  Mivel a második telepítési használt "Deployment1" nevét az első telepítési és "Deployment2", két bejegyzés fognak rendelkezésére állni.  Kattintson a telepítés gombra kattintva egy lap, amely mutatja az eredményeket minden telepítéshez jeleníti meg.
  ![Portál](./media/solution-dev-test-environments/portal3.png)



## <a name="delete-environments"></a>Környezetekben törlése
Miután végzett-környezet használatával, bizonyára tudni szeretné törölheti azt, így nem merülnek fel a használati költségek Azure erőforrások már nem használ.  Környezetekben törlése nem még egyszerűbb létrehozása őket.  Az előző lépéseket Azure-erőforrás különálló csoportjait létrehozott környezetben, és kattintson az erőforrások az erőforrás csoportokba rendszerbe volt. 

Törölje a környezetben használja, az alábbi eljárások valamelyikét.  Minden módszer az azonos így éri el.

### <a name="azure-cli"></a>Azure CLI

A CLI kérdésnél írja be a következőt:

    azure group delete "TestApp1-Development"

Amikor a rendszer kéri, adja meg az y, és nyomja meg az enter eltávolítása a fejlesztői környezet és az összes erőforrásait. Néhány perc múlva a parancs ad eredményül a következő:

    info:    group delete command OK

A CLI kérdésnél írja be a következőt a hátralévő környezetben törlése:

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
### <a name="powershell"></a>A PowerShell

Azure PowerShell (1.01 vagy újabb verzió) parancssort írja be az erőforráscsoport és minden tartalmának törlése az alábbi parancsot.    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

Amikor a rendszer kéri, ha biztos benne, hogy el szeretné távolítani az erőforráscsoport, adja meg az y, majd az enter billentyűt.

Írja be a következőt a hátralévő környezetben törlése:

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

### <a name="azure-portal"></a>Azure portál

1. Az Azure-portálon nyissa meg az erőforrás csoportok ahogy a fenti lépéseket. 
2. Jelölje ki a TestApp1 fejlesztési erőforrás csoportot, és kattintson a Törlés gombra a csoport TestApp1 fejlesztési erőforrás lap. Egy új lap jelenik meg. Írja be az erőforrás nevére, és kattintson a Törlés gombra.
![Portál](./media/solution-dev-test-environments/rgdelete.png)
3. És TestApp1-előtti-gyártási erőforráscsoport töröl a TestApp1 fejlesztési erőforráscsoport ugyanúgy TestApp1-próba törlése.

Függetlenül attól, a módszert használja az erőforrás és az összes azok található erőforrás megszűnik, és fogja már nem merülnek fel számlázási költségeket az erőforrásokhoz tartozó.  

Az Azure minimalizálásához erőforrás-kihasználtsági kiadások, alkalmazás fejlesztés [Azure automatizálási](automation/automation-intro.md) segítségével ütemezése során merülnek fel feladatok:

- Állítsa le a virtuális gépeken futó naponta végén, és újra kell indítania őket naponta elején.
- Törölje teljes környezetekben naponta végén és hozza létre őket naponta elején.
 
Most, hogy tapasztal, hogy milyen könnyen hozhat létre, megtartásához és törlése a fejlesztés és tesztelje a környezetben, ismerje meg Önről milyen egyszerűen did további kísérletezzen a fenti lépéseket, és a hivatkozásokat, a jelen cikkben szereplő olvasása.

## <a name="next-steps"></a>Következő lépések

- [Delegált felügyeletet](./active-directory/role-based-access-control-configure.md) más erőforrások rendelhet a Microsoft Azure Active Directory-csoportok és felhasználók adott szerepkörök, az azt jelenti, hogy csak egy részhalmazát Azure erőforrások műveletek végrehajtása rendelkező minden környezetben.
- [Címkék hozzárendelése](resource-group-using-tags.md) az egyes környezetekben, illetve az egyes erőforrások az erőforrás-csoportokat. Előfordulhat, hogy egy "Környezet" címke hozzá az erőforrás-csoportokat, és állítsa a környezet nevek felel meg az értékét. Címkék különösen akkor hasznos lehet, ha a számlázási vagy kezelése az erőforrások rendszerezése kell.
- Lync-értesítések és a számlázásra erőforrás csoport erőforrások az [Azure portálon](https://portal.azure.com).
