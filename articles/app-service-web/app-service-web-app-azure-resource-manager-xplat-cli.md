<properties
    pageTitle="Azure Azure erőforrás-kezelő-alapú platformok parancssori eszközök webes alkalmazás |} Microsoft Azure"
    description="Megtudhatja, hogyan lehet az új erőforrás-kezelő Azure-alapú platformok parancssori eszközök segítségével az Azure Web Apps alkalmazások kezelése."
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

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Azure erőforrás Manager-alapú XPlat CLI Azure Web App használata#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Microsoft Azure-platformok parancssori eszközök verzió 0.10.5 kiadását az új parancsok felvétele Ezek a parancsok engedély megadása a felhasználó a Web Apps alkalmazások kezelése a PowerShell erőforrás-kezelő Azure-alapú parancsaival.

Erőforrás-csoportok kezelése kapcsolatos további tudnivalókért ismerje meg [Az Azure CLI Azure erőforrások és erőforrás-csoportok kezelése](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Alkalmazás-szolgáltatás kezelése csomagokhoz ##

### <a name="create-an-app-service-plan"></a>Alkalmazás szolgáltatás terv létrehozása ###
Hozzon létre egy alkalmazás szolgáltatáscsomagja, használja az **azure appserviceplan létrehozása** parancsot.

Az alábbiakban a különböző paraméterek leírása:

-   **– erőforráscsoport**: erőforráscsoport, amely tartalmazza az újonnan létrehozott alkalmazás szolgáltatáscsomagja.
-   **--neve**: az alkalmazás szolgáltatáscsomagja neve.
-   **--hely**: alkalmazás SRV-csomagot.
-   **– réteg**: a kívánt árak termékváltozat (a következők: az F1 (ingyenes). A D1 (megosztott). B1 (egyszerű kicsi), a B2 (egyszerű közepes), és a B3 (Basic nagy). S1 (normál kicsi), S2 (normál közepes), és S3 (normál nagy). P1 (prémium kicsi), P2 (prémium közepes), és P3 (prémium nagy).)
-   **– példányok**: az alkalmazás dolgozók számának szolgáltatás terv (alapértelmezett érték 1).

Példa: Ezzel a parancsmaggal:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Meglévő App milyen szolgáltatáscsomagok lista ###

A meglévő app-csomagok listában paranccsal **azure appserviceplan listában** .

Egy adott erőforrás csoportban az összes alkalmazás milyen szolgáltatáscsomagok listában használja:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Úgy juthat az egy adott alkalmazás szolgáltatáscsomagja, paranccsal **azure appserviceplan megjelenítése** :

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Egy meglévő alkalmazás szolgáltatás megtervezése konfigurálása ###

Egy meglévő alkalmazás szolgáltatáscsomagja beállításainak módosításához a **azure appserviceplan config** paranccsal. Módosíthatja a termékváltozat és a munkatársak számát 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Az alkalmazás szolgáltatáscsomagja méretezése ####

Ha át kívánja méretezni, hogy egy meglévő alkalmazás szolgáltatás tervet, használja:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Az alkalmazás szolgáltatás tervet Termékváltozat módosítása ####

Egy meglévő alkalmazás szolgáltatás terv sku beállításához használja:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Egy meglévő alkalmazás szolgáltatás megtervezése törlése ###

Ha törölni szeretne egy meglévő alkalmazás szolgáltatáscsomagja, az összes hozzárendelt webalkalmazások kell helyezett vagy a törölt először. Akkor használja, törölheti az alkalmazás szolgáltatáscsomagja **azure webalkalmazás törlése** parancsot.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Alkalmazás szolgáltatás Web Apps alkalmazások kezelése ##

### <a name="create-a-web-app"></a>Egy webalkalmazás létrehozása ###

Hozzon létre webalkalmazást, használja a **azure webalkalmazás létrehozása** parancsot.

Az alábbiakban a különböző paraméterek leírása:

- **– név**: a web app neve.
- **– terv**: a szolgáltatás terv tárolni a web app használt nevét.
- **– erőforráscsoport**: az alkalmazás szolgáltatáscsomagja üzemeltető erőforráscsoport.
- **– hely**: az alkalmazás webalapú helyét.

Példa: Ezzel a parancsmaggal:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Egy meglévő webalkalmazás törlése ###

Egy meglévő webalkalmazás törlése paranccsal az **azure webappban törlése** , meg kell adnia a web app nevét és az erőforrás-csoport nevét.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Létező lista Web Apps alkalmazások ###

A meglévő web Apps alkalmazások listáját, a **azure webappban lista** paranccsal.

A lista összes web Apps alkalmazások egy adott erőforrás csoportban, használhatja:

    azure webapp list --resource-group ContosoAzureResourceGroup

Ha egy adott web App alkalmazásban, a **azure webappban megjelenítése** paranccsal.

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Egy meglévő webalkalmazás konfigurálása ###

Egy meglévő webalkalmazás konfigurációk és a beállítások módosításához használja a **azure webappban config beállítása** parancsot.

Példa (1): a webalkalmazást php verziója módosítása 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Példa (2): hozzáadása vagy alkalmazás beállításainak módosítása

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Szeretné, hogy mi egyéb konfigurációs lehet módosította, a paranccsal **azure webappban config set -h** .

### <a name="change-the-state-of-an-existing-web-app"></a>Egy meglévő webalkalmazás állapotának módosítása ###

#### <a name="restart-a-web-app"></a>Indítsa újra a megfelelő web App alkalmazásban ####

Indítsa újra a megfelelő web App alkalmazásban, hogy meg kell adnia a web app nevét és az erőforrás csoportjában található.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Állítsa le a megfelelő web App alkalmazásban ####

Le szeretné állítani a megfelelő web App alkalmazásban, meg kell adnia a web app nevét és az erőforrás csoportjában található.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Indítsa el a megfelelő web App alkalmazásban ####

Szeretné kezdeni a megfelelő web App alkalmazásban, meg kell adnia a web app nevét és az erőforrás csoportjában található.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Web App közzétételi profilok kezelése ###

Minden egyes web app-alkalmazások közzététele használható közzétételi profil tartalmaz.

#### <a name="get-publishing-profile"></a>Profil első közzététele ####

A közzétételi profil egy webalkalmazás, használja:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Ez a parancs a közzétételi profil felhasználónév és jelszó a parancssor echók.

### <a name="manage-web-app-hostnames"></a>Webalkalmazás állomásnevekké kezelése ###

Kezelése a webalkalmazás hostname (állomásnév) kötések, az **azure webappban config állomásnevekké** parancs használatával  

#### <a name="list-hostname-bindings"></a>Lista hostname (állomásnév) kötések ####

Az aktuális hostname (állomásnév) kötések egy webalkalmazás, használja:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Hostname (állomásnév) kötések hozzáadása ####

Hostname (állomásnév) kötések a web App hozhatja létre:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Hostname (állomásnév) kötések törlése ####

Hostname (állomásnév) kötések törléséhez használja:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Következő lépések ###
- Azure erőforrás-kezelő CLI támogatási kapcsolatos további tudnivalókért lásd: [az Azure CLI segítségével kezelheti az Azure erőforrásokat és erőforrás-csoportok.](../xplat-cli-azure-resource-manager.md)
- Kezelése a PowerShell használatá alkalmazás szolgáltatás kapcsolatos további tudnivalókért lásd: [Using Azure Resource Manager-Based PowerShell kezelése Azure-webalkalmazásokra.](app-service-web-app-azure-resource-manager-powershell.md)
