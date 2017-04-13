<properties
    pageTitle="Erőforrás-kezelő-alapú PowerShell Azure parancsok Azure webalkalmazás |} Microsoft Azure"
    description="Megtudhatja, hogy miként az Azure Web Apps alkalmazások kezeléséhez használja az új erőforrás-kezelő Azure-alapú PowerShell parancsokat."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Azure erőforrás-kezelő-alapú PowerShell használatá Azure Web Apps alkalmazások kezelése#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

A Microsoft Azure PowerShell 1.0.0 verzió új parancsok felvétele, amely engedély megadása a felhasználó a Web Apps alkalmazások kezelése a PowerShell erőforrás-kezelő Azure-alapú parancsaival.

Erőforrás-csoportok kezelése kapcsolatos további tudnivalókért lásd: [Azure PowerShell használatá Azure erőforrás-kezelővel](../powershell-azure-resource-manager.md). 

A teljes beállításlista paramétereket és a beállítások a PowerShell-parancsmagok kapcsolatos további tudnivalókért lásd: a [Teljes parancsmagjai – referencia kezelő Web App Azure-alapú PowerShell-parancsmagok](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Alkalmazás-szolgáltatás kezelése csomagokhoz ##

### <a name="create-an-app-service-plan"></a>Alkalmazás szolgáltatás terv létrehozása ###
Hozzon létre egy alkalmazás szolgáltatáscsomagja, használja a **New-AzureRmAppServicePlan** parancsmag.

Az alábbiakban a különböző paraméterek leírása:

-   **Név**: az alkalmazás szolgáltatáscsomagja neve.
-   **Hely**: terv hely szolgáltatás.
-   **ResourceGroupName**: erőforráscsoport, amely tartalmazza az újonnan létrehozott alkalmazás szolgáltatáscsomagja.
-   **Réteg**: a kívánt árak réteg (ingyenes az alapértelmezett érték, a megosztott, Basic, normál és Premium.)
-   **WorkerSize**: méretét a dolgozók (az alapértelmezett érték kisebb, ha a réteg paraméterrel megadott Basic, a normál vagy a prémium. Egyéb lehetőségek is közepes és nagy.)
-   **NumberofWorkers**: az alkalmazás dolgozók számának szolgáltatás terv (alapértelmezett érték 1). 

Példa: Ezzel a parancsmaggal:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben alkalmazás szolgáltatás terv létrehozása ###
Az alkalmazás-szolgáltatási környezetben-alkalmazás szolgáltatáscsomagja létrehozásához további paraméterekkel parancs **Új-AzureRmAppServicePlan** parancs ugyanaz segítségével megadhatja a ASE és ASE tartozó erőforrás csoport nevét.

Példa: Ezzel a parancsmaggal:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Ha többet szeretne tudni az alkalmazás-szolgáltatási környezetben, olvassa el az [alkalmazás szolgáltatási környezetben – bevezetés](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Meglévő App milyen szolgáltatáscsomagok lista ###

A meglévő app-csomagok listája, használja a **Get-AzureRmAppServicePlan** parancsmagot.

Az előfizetés csoportban az összes alkalmazás milyen szolgáltatáscsomagok listában használja: 

    Get-AzureRmAppServicePlan

Egy adott erőforrás csoportban az összes alkalmazás milyen szolgáltatáscsomagok listában használja:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Egy adott alkalmazás szolgáltatáscsomagja, használja:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Egy meglévő alkalmazás szolgáltatás megtervezése konfigurálása ###

A **Set-AzureRmAppServicePlan** parancsmag használatával egy meglévő alkalmazás-csomagra beállításainak módosításához. Módosíthatja a réteg, dolgozó méretét és a munkatársak számát 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Az alkalmazás szolgáltatáscsomagja méretezése ####

Ha át kívánja méretezni, hogy egy meglévő alkalmazás szolgáltatás tervet, használja:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Az alkalmazás szolgáltatás megtervezése dolgozó méretének módosítása ####

Egy meglévő alkalmazás szolgáltatás megtervezése dolgozók méretének módosításához használja:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>A réteg-szolgáltatási alkalmazás tervének módosítása ####

A réteg egy meglévő alkalmazás szolgáltatás terv módosításához használja:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Egy meglévő alkalmazás szolgáltatás megtervezése törlése ###

Ha törölni szeretne egy meglévő alkalmazás szolgáltatáscsomagja, az összes hozzárendelt webalkalmazások kell helyezett vagy a törölt először. Kattintson a parancsmaggal **Eltávolítása-AzureRmAppServicePlan** , törölheti az alkalmazás szolgáltatáscsomagja.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Alkalmazás szolgáltatás Web Apps alkalmazások kezelése ##

### <a name="create-a-web-app"></a>Egy webalkalmazás létrehozása ###

A **New-AzureRmWebApp** parancsmag használatával webalkalmazást létrehozásához.

Az alábbiakban a különböző paraméterek leírása:

- **Név**: a web app neve.
- **AppServicePlan**: a szolgáltatás terv tárolni a web app használt nevét.
- **ResourceGroupName**: az alkalmazás szolgáltatáscsomagja üzemeltető erőforráscsoport.
- **Hely**: az alkalmazás webalapú helyét.

Példa: Ezzel a parancsmaggal:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Hozzon létre egy Web App alkalmazás szolgáltatási környezetben ###

A web app-alkalmazás szolgáltatási környezetben (SKB) létrehozása. További paraméterekkel ugyanazt a **New-AzureRmWebApp** parancs segítségével megadhatja, ASE nevét és az erőforrás-csoport nevét, amelyhez a ASE tartozik.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Ha többet szeretne tudni az alkalmazás-szolgáltatási környezetben, olvassa el az [alkalmazás szolgáltatási környezetben – bevezetés](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Egy meglévő webalkalmazás törlése ###

Egy meglévő webalkalmazás törlése az **Eltávolítás-AzureRmWebApp** parancsmag is használhatja, meg kell adnia a web app nevét és az erőforrás-csoport nevét.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Létező lista Web Apps alkalmazások ###

A **Get-AzureRmWebApp** parancsmag használatával a meglévő web Apps alkalmazások listában.

A lista összes web Apps alkalmazások csoportban az előfizetést, használhatja:

    Get-AzureRmWebApp

A lista összes web Apps alkalmazások egy adott erőforrás csoportban, használhatja:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Egy adott web App alkalmazásban, használja:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Egy meglévő webalkalmazás konfigurálása ###

Egy meglévő webalkalmazás konfigurációk és a beállítások módosításához használja a **Set-AzureRmWebApp** parancsmagot. Paraméterek teljes listáját olvassa el a [parancsmag hivatkozás](https://msdn.microsoft.com/library/mt652487.aspx)

Példa (1): Ezzel a parancsmaggal használatával módosíthatja a kapcsolati karakterláncot

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Példa (2): hozzáadása vagy alkalmazás beállításainak módosítása

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Példa (3): állítsa a web app futtatása 64 bites módban

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Egy meglévő webalkalmazás állapotának módosítása ###

#### <a name="restart-a-web-app"></a>Indítsa újra a megfelelő web App alkalmazásban ####

Indítsa újra a megfelelő web App alkalmazásban, hogy meg kell adnia a web app nevét és az erőforrás csoportjában található.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Állítsa le a megfelelő web App alkalmazásban ####

Le szeretné állítani a megfelelő web App alkalmazásban, meg kell adnia a web app nevét és az erőforrás csoportjában található.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Indítsa el a megfelelő web App alkalmazásban ####

Szeretné kezdeni a megfelelő web App alkalmazásban, meg kell adnia a web app nevét és az erőforrás csoportjában található.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Web App közzétételi profilok kezelése ###

Minden web app-alkalmazások közzététele használható közzétételi profil, akkor több művelet a közzétételi profilok futtatható.

#### <a name="get-publishing-profile"></a>Profil első közzététele ####

A közzétételi profil egy webalkalmazás, használja:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Ez a parancs echók a közzétételi profilt a parancssor, valamint kimeneti a közzétételi profil szövegfájlhoz.

#### <a name="reset-publishing-profile"></a>Közzétételi profil alaphelyzetbe állítása ####

Hozzon létre egy új mindkét FTP és internetes közzétételi jelszavának üzembe helyezése a web app használata:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Web App tanúsítványok kezelése ###

Arról, hogy miként kezelheti a web app tanúsítványok című témakörben talál [az SSL-tanúsítványok kötés PowerShell használatával](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Következő lépések ###
- Azure PowerShell-erőforrás-kezelő támogatási kapcsolatos további tudnivalókért lásd: [Azure PowerShell használatá Azure erőforrás-kezelővel.](../powershell-azure-resource-manager.md)
- Alkalmazás szolgáltatás környezetekkel kapcsolatos további tudnivalókért lásd: [alkalmazás szolgáltatási környezetben – bevezetés.](app-service-app-service-environment-intro.md)
- Kezelése a PowerShell használatá alkalmazás szolgáltatás SSL-tanúsítványok kapcsolatos további tudnivalókért lásd: [PowerShell használata az SSL-tanúsítványok kötés.](app-service-web-app-powershell-ssl-binding.md)
- További információt a teljes listát az erőforrás-kezelő Azure-alapú PowerShell-parancsmagok az Azure-webalkalmazások, témakörben [Web Apps alkalmazások Azure erőforrás-kezelő PowerShell-parancsmagok Azure parancsmagjai – referencia.](https://msdn.microsoft.com/library/mt619237.aspx)
- - CLI alkalmazás szolgáltatás kezelése című témakörben talál [Using Azure Resource Manager-Based XPlat CLI az Azure Web App](app-service-web-app-azure-resource-manager-xplat-cli.md)
