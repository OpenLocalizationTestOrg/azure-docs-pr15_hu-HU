<properties 
    pageTitle="Erőforrás-kezelő Azure PowerShell |} Microsoft Azure" 
    description="Azure PowerShell használatával több erőforrások erőforrás csoportként Azure bemutatása." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Azure PowerShell használatával a Azure-kezelő eszközzel

> [AZURE.SELECTOR]
- [Portál](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST API-VAL](resource-manager-rest-api.md)


Azure erőforrás-kezelő végrehajtja a modern megközelítés Azure erőforrások életciklus vezérlőelemre. Helyett létrehozásáról és kezeléséről az egyes erőforrások, akkor kezdje azzal, hogy teljes megoldás, például a blogban, a fotótár, egy SharePoint portal vagy wikilap Tervező. – Deklaráció ábrázolása a megoldást – sablon segítségével adjon meg egy erőforrás csoportot, amely tartalmaz minden ezzel az erőforrásokhoz támogatja a megoldást. Ezután üzembe helyezéséhez és kezelése egy logikai egységként erőforráscsoport. 

Ebből az oktatóanyagból megismerheti, hogyan Azure-PowerShell használata az Azure erőforrás-kezelő. Végigvezeti a folyamaton, a megoldást üzembe helyezné, és a megoldás használata. Azure PowerShell és erőforrás-kezelő sablon üzembe használja:

- Az SQL server - tárolni az adatbázisban
- SQL-adatbázis – az adatok tárolására
- Tűzfalszabályokat – az adatbázishoz való csatlakozáshoz a webes alkalmazás engedélyezése
- Alkalmazás szolgáltatás terv - szolgáltatásait és a web app költségeinek meghatározása
- Webhely - futtatásához a web App alkalmazásban
- Webes config - a kapcsolati karakterláncot az adatbázis tárolásához 
- Riasztási szabályok - teljesítmény és hibák ellenőrzése
- Alkalmazás háttérismeretek – a beállítások automatikus méretezése

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez szükséges:

- Azure-fiók
  + [Nyissa meg az ingyenes Azure-fiók](/pricing/free-trial/?WT.mc_id=A261C142F)van lehetősége: jóváírások kap próbálja ki az fizetett Azure szolgáltatások is használhatja, és a megszokott ezek után akár tarthatja a fiók, és használata ingyenes Azure szolgáltatások, például webhelyek. A hitelkártya soha nem megterheljük, kivéve, ha kifejezetten az beállítások módosítása és kérdezzen rá kell fizetnie.
  
  + [MSDN előfizetői előnyeinek aktiválása](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)közül választhat: az MSDN-előfizetés lépve credits havonta fizetett Azure szolgáltatások használható.
- Azure PowerShell 1.0. Ebben a verzióban, és útmutatást adunk a telepítés tudni megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](powershell-install-configure.md).

Ebben az oktatóanyagban lett tervezve PowerShell kezdőknek, de azt feltételezi, hogy megismeri a alapfogalmak, például modulokat, parancsmagok és a munkamenetek.

## <a name="get-help-for-cmdlets"></a>Segítség a parancsmagok

Részletes bármely parancsmag ebben az oktatóanyagban megjelenik a, használja a Get-Help parancsmag. 

    Get-Help <cmdlet-name> -Detailed

A Get-AzureRmResource parancsmag súgójának, írja be például a:

    Get-Help Get-AzureRmResource -Detailed

Lista beszerzése az parancsmagok az erőforrások és a Súgó összegzést modulban, írja be: 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

A kimenet jelenik meg a következő kivonat hasonló:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Teljes segítségért parancsmag, írja be a formázás parancsot:

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Jelentkezzen be az Azure-fiók

A megoldás dolgozik, mielőtt kell bejelentkezés a fiókjába.

Bejelentkezési az Azure-fiókjába, használja a **Hozzáadás-AzureRmAccount** parancsmag.

    Add-AzureRmAccount

A parancsmaggal, a bejelentkezési hitelesítő adatokat kér, az Azure-fiók. Naplózás után letöltés a Fiókbeállítások, hogy Azure PowerShell elérhetők legyenek. 

A Fiókbeállítások járjon le, hogy frissítenie kell azokat alkalmanként. A fiók beállításainak frissítheti, futtassa újra a **Hozzáadás-AzureRmAccount** . 

>[AZURE.NOTE] Az erőforrás-kezelő modulok hozzáadása-AzureRmAccount szükséges. A Publish Settings fájl nem elegendő.     

Ha egynél több előfizetése van, adja meg a **Set-AzureRmContext** parancsmagot a telepítéshez használni kívánt előfizetés azonosítója.

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Erőforráscsoport létrehozása

Erőforrások mielőtt az előfizetés, létre kell hoznia egy, amely tartalmazni fogja az erőforrások erőforráscsoport. 

A **New-AzureRmResourceGroup** parancsmag segítségével egy erőforrás csoportot hozhat létre.

A parancs a **név** paraméter adjon meg egy nevet az erőforráscsoport és a **hely** paraméter helyére megadni. Mi azt talált az előző szakasz alapján, fogjuk használni "USA-beli nyugati" helyét.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
A kimenet lesz hasonló:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

Az erőforráscsoport létrehozása sikeresen befejeződött.

## <a name="deploy-your-solution"></a>A megoldás telepítése

Ez a témakör nem megtudhatja, hogyan hozhatja létre a sablon, és a sablon felépítésének megvitatása. Ezeket az információkat a [szerzői Azure erőforrás-kezelő sablonok](resource-group-authoring-templates.md) és [Erőforrás-kezelő sablon áttekintése a következő](resource-manager-template-walkthrough.md)témakörben talál. Az előre definiált [rendelkezést tartalmazó SQL-adatbázis webalkalmazást](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) sablon [Azure quickstart útmutató](https://azure.microsoft.com/documentation/templates/)sablonok telepíti.

Az erőforráscsoport van, és a sablont, így most már készen áll az erőforráscsoport a sablon meghatározott infrastruktúra üzembe van. A **New-AzureRmResourceGroupDeployment** parancsmag rendelkező erőforrások rendszerbe. A sablon Itt adhatja meg, hány alapértelmezett értékeket, amelyek fogjuk használni, nem kell azokat a paramétereket adja meg az értékeket. Az egyszerű alábbi néz ki:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

Megadhatja, hogy az erőforrás csoport és a sablon helyét. Ha a sablon egy helyi fájl, **- TemplateFile** paraméter használatával, és adja meg a sablon elérési útját. Beállíthatja, hogy a **-mód** **Léptetési távolság** vagy a **teljes**paraméter. Alapértelmezés szerint erőforrás-kezelő növekményes frissítés végrehajtása; a telepítés során Emiatt nem fontos beállítása **-mód** Ha azt szeretné, hogy **Léptetési távolság**. Az alábbi telepítési módok közötti különbségek, című cikkben talál részletes [Deploy Azure erőforrás-kezelő sablonnal kérelmet](resource-group-template-deploy.md). 

###<a name="dynamic-template-parameters"></a>Dinamikus sablon paraméterei

Ha ismeri a PowerShell, tudni fogja, hogy a mínuszjel (-) beírásával, és kattintson a TAB billentyű a rendelkezésre álló paraméterek parancsmag válthat. Ez a funkció akkor is használható, amely csak a sablon paraméterrel. Amint a sablon nevét a szó beírásakor a parancsmag beolvassa a sablont, értelmezi azt, és a sablon paramétereket ad a parancs dinamikusan. Ezzel megkönnyíti nagyon adja meg a sablon paraméterértékeket.

Amikor megadja a parancs, a program kéri a hiányzó **administratorLoginPassword**kötelező paraméter. És a jelszó beírásakor látható-e a biztonságos karakterláncérték. A stratégia megszünteti a kockázat biztosítása egy jelszót egyszerű szöveges formátumban.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

A sablon a parancsot a sablon (például a sablon, amely megegyezik a **ResourceGroupName** paramétert a [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) parancsmag **ResourceGroupName** nevű paraméter együtt) üzembe a paraméterek megegyező nevet a paraméter tartalmaz, a rendszer kéri a utólagos javítás **FromTemplate** (például **ResourceGroupNameFromTemplate**) paraméter adjon meg egy értéket. Általánosságban elmondható ne a fejetlenséget által nem elnevezési paraméterek paraméterként használt üzembe helyezési műveleteket azonos nevű.

A parancs fut, és az erőforrások létrehozásakor a üzenetek adja eredményül. Végül az eredmény a központi telepítés.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

Mindössze néhány lépésben hogy létrehozott és összetett webhely szükséges erőforrások rendszerbe. 

### <a name="log-debug-information"></a>Naplóadatok hibakeresési

Sablon telepítésekor **Új-AzureRmResourceGroupDeployment**futtatásakor a **- DeploymentDebugLogLevel** paraméter megadásával jelentkezhet a kérelem és a válasz további információt. Ez az információ segítséget nyújthatnak telepítési hibáinak elhárítása. Az alapértelmezett értéke **nincs** ami azt jelenti, nem a kérelmet, vagy válasz tartalom be van jelentkezve. Megadhatja, hogy a tartalom naplózás a kérést, a válasz vagy mindkettőt.  Hibaelhárítás telepítések és hibakeresési információk naplózása kapcsolatos további tudnivalókért olvassa el a [Hibaelhárítás erőforrás csoport telepítési környezetének Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md)című témakört. Az alábbi példa a kérelem tartalom és a válasz tartalmat a telepítéshez naplózza.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] A DeploymentDebugLogLevel paraméter beállításakor gondosan fontolja meg a telepítés során áthaladó az információtípust. A kérések és válaszok naplózás információkért által esetleg bizalmas adatokat másolja át az üzembe helyezési műveleteket sikerült nézetéhez. 


## <a name="get-information-about-your-resource-groups"></a>Az erőforrás-csoportok adatainak megtekintése

Miután létrehozott egy erőforráscsoport, a parancsmagok az erőforrás-kezelő modulban is használhatja az erőforrás-csoportok kezelése.

- Erőforráscsoport az előfizetését, használja a **Get-AzureRmResourceGroup** parancsmagot:

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Amely adja eredményül az alábbi adatokat:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Ha nem adja meg a erőforrás-csoport nevét, a parancsmag adja vissza az összes erőforrás csoportokat az előfizetés.

- Az erőforrások erőforráscsoport a, használja a **Keresés-AzureRmResource** parancsmag és annak **ResourceGroupNameContains** paraméter. Paraméterek nélkül keresés-AzureRmResource összes erőforrás megkapja az Azure előfizetés.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Amely adja eredményül: erőforrások listája, formátumú, például:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- Címkék logikailag az erőforrások rendszerezheti az előfizetését, valamint a **Keresés-AzureRmResource** és a **Keresés-AzureRmResourceGroup** parancsmagokat erőforrások is használhatja.

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Erőforráscsoport hozzáadása

Erőforrás felvétele az erőforráscsoport, a **New-AzureRmResource** parancsmag is használhatja. Jó helyen jár így olyan erőforrást vesz fel lehet, hogy félrevezető jövőbeli, mert az új erőforrás nem létezik a sablon. Ha újra telepíti a régi sablont, egy teljes megoldás szeretné telepíteni. Ha gyakran üzembe helyezése kifejezésre keresve, könnyebben és megbízhatóbb az új erőforrás hozzáadása a sablonhoz, és újra telepítheti.

## <a name="move-a-resource"></a>Erőforrás áthelyezése

Új erőforráscsoport meglévő erőforrásokat viheti át. A példákban témakörökben [áthelyezése új erőforráscsoport vagy-előfizetésre](resource-group-move-resources.md).

## <a name="export-template"></a>Sablon exportálása

Meglévő erőforráscsoport (rendszerbe PowerShell vagy más módszerek a portálon például egy keresztül) az erőforrás-kezelő sablon az erőforráscsoport tekinthet meg. A sablon exportálása kínál a két előnyökkel jár:

1. Egyszerűen automatizálhatja a jövőbeli telepítéseknél a megoldást, mivel az összes infrastruktúra határozza meg a sablont.

2. Akkor is megismerje sablon szintaxis megjeleníti a a JavaScript objektum jelölés (JSON), amely a megoldás.

Az Powershellen keresztül is, amely az aktuális állapotát az erőforráscsoport sablon készítése, vagy a sablon egy adott telepítéshez használt beolvasásához.

A sablon erőforráscsoport exportálása akkor hasznos, ha egy erőforráscsoport módosításokat tettek, és annak aktuális állapotában JSON ábrázolása beolvasásához szükség. A létrehozott sablonra azonban csak minimális számú paramétereket és nincsenek változók tartalmaz. A sablonban szereplő értékek táblázatparancsok nagy része csomagolásukkor. A létrehozott sablont mielőtt kívánt előfordulhat, hogy paramétereket konvertálása több értékét, testre szabhatja a különböző környezetekben a telepítést.

A sablon egy adott telepítéshez exportálása akkor hasznos, ha a sablon megjelenítése a tényleges erőforrások üzembe használt kell. A sablon összes paramétereket és az eredeti telepítéshez meghatározott változók tartalmazni fogja. Jó helyen jár Ha a szervezet módosította az erőforráscsoport kívül a sablonban által definiált, ezzel a sablonnal nem jelöli az erőforráscsoport aktuális állapotát.

> [AZURE.NOTE] Az Exportálás sablonja szolgáltatás előzetes verzióban, és nem az összes erőforrástípus ismer exportálása egy sablont. Ha megpróbál exportálása egy sablont,, amely arról tájékoztat, hogy egyes erőforrások nem exportált hiba jelenhet meg. Ha szükséges, akkor kézzel megadható ezek az erőforrások a sablon letölti után.

###<a name="export-template-from-resource-group"></a>Erőforráscsoport sablon exportálása

A sablon egy erőforrás csoport megtekintéséhez futtassa **Export-AzureRmResourceGroup** .

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Töltse le a sablont telepítési

Egy adott telepítéshez használt sablon letöltéséhez futtassa **Mentés-AzureRmResourceGroupDeploymentTemplate** .

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Erőforrások vagy erőforrás csoport törlése

- Az erőforráscsoport töröl egy erőforrást, használja az **Eltávolítás-AzureRmResource** parancsmag. Ezzel a parancsmaggal törli az erőforráshoz, de nem törli az erőforráscsoport.

    Ez a parancs eltávolítása a TestRG1 erőforráscsoport a proba webhelyet.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Erőforráscsoport törléséhez használja a **Eltávolítása-AzureRmResourceGroup** parancsmag. Ezzel a parancsmaggal törli az erőforráscsoport és erőforrásait.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    A törlés megerősítését kéri.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Telepítési parancsfájlt

Ez a témakör az egyes parancsmag mutatott korábbi telepítési példái szükséges erőforrások Azure telepítéshez használni. A következő példa bemutatja az olyan telepítési parancsfájlt, amely az erőforráscsoport hoz létre, és az erőforrások üzembe helyezése.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Következő lépések

- Erőforrás-kezelő sablonok létrehozásával kapcsolatos további tudnivalókért olvassa el az [Azure erőforrás-kezelő sablonok létrehozása](./resource-group-authoring-templates.md)című témakört.
- Sablonok telepítésével kapcsolatos további tudnivalókért lásd: [Deploy Azure erőforrás-kezelő sablonnal kérelmet](./resource-group-template-deploy.md).
- A részletes példa az üzembe helyezése a projekt című témakörben [Deploy microservices előre Azure-ban](app-service-web/app-service-deploy-complex-application-predictably.md).
- Telepítés nem sikerült hibaelhárítási kapcsolatos további tudnivalókért olvassa el a [Hibaelhárítás erőforrás csoport telepítések Azure-ban](./resource-manager-troubleshoot-deployments-powershell.md)című témakört.

