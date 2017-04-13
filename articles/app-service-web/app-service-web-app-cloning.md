<properties
    pageTitle="Web App klónozhatja PowerShell használatával"
    description="További információ a Web Apps alkalmazások új Web Apps alkalmazások használata a PowerShell klónozhatja."
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
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure alkalmazás szolgáltatás alkalmazás klónozhatja a PowerShell használatával#

Az verziójának megjelenése után a Microsoft Azure PowerShell 1.1.0 verzió új lehetőséget szeretne adni, a felhasználó másolása egy másik régióbeli vagy ugyanabban a régióban egy újonnan létrehozott alkalmazás meglévő webes alkalmazás új AzureRMWebApp lett hozzáadva. Ezzel engedélyezi az ügyfelek számára üzembe alkalmazások számos különböző területei között, gyorsan és egyszerűen.

Alkalmazás klónozhatja jelenleg csak a prémium réteg app milyen szolgáltatáscsomagok támogatott. Az új szolgáltatás a korlátozásai használja, mint a Web Apps alkalmazások biztonsági szolgáltatás, című témakörben talál, [készítsen biztonsági másolatot az Azure alkalmazás szolgáltatás webalkalmazást](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Megismerheti az erőforrás-kezelő Azure-alapú Azure PowerShell-parancsmagok a Web Apps alkalmazások kezelése jelölje be [erőforrás-kezelő Azure-alapú PowerShell-parancsok az Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Meglévő alkalmazás klónozhatja ##

Alkalmazási helyzetek: Egy meglévő webalkalmazást Dél központi US területen, a felhasználó szeretné klónozhatja egy új web App Észak központi US régióban tartalmát. Ez az erőforrás-kezelő Azure verzióját a PowerShell-parancsmag használatával hozhat létre egy új web app - SourceWebApp lehetőséggel végezhető el.

Az erőforrás-csoport nevét, amely tartalmazza a forrás web app ismeretében is használjuk az alábbi PowerShell-parancsot a forrás web app-információ (ebben az esetben a forrás-webappban neve):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Hozzon létre egy új alkalmazás szolgáltatás megtervezése, a azt paranccsal új-AzureRmAppServicePlan a következő példának megfelelően

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

A New-AzureRmWebApp paranccsal azt az új webalkalmazás létrehozása a Észak központi US régióban, és kötik azt az egy meglévő prémium réteg alkalmazás szolgáltatás megtervezése, továbbá azt ugyanazon a erőforráscsoport használni a forrás web app, illetve megadhatja az új erőforráscsoport, az alábbi bemutatja, hogy:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Egy meglévő web App alkalmazásban, beleértve a kapcsolódó telepítési klónozhatja résidők, a felhasználónak kell IncludeSourceWebAppSlots paraméter használatával, az alábbi PowerShell-parancsot bemutatja, hogy a New-AzureRmWebApp parancs paraméter:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Egy meglévő web App alkalmazásban az azonos régión belüli klónozhatja, a felhasználónak kell hozzon létre egy új erőforráscsoport és az új alkalmazás szolgáltatás ugyanabban a régióban tervezése és az alábbi PowerShell-parancs használatával klónozhatja a web App alkalmazásban

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben meglévő alkalmazás klónozhatja ##

Alkalmazási helyzetek: Egy meglévő web app a déli központi US régió, a felhasználónak szeretné klónozhatja egy új web App alkalmazásban való egy meglévő alkalmazás szolgáltatási környezetben (SKB) tartalma.

Az erőforrás-csoport nevét, amely tartalmazza a forrás web app ismeretében is használjuk az alábbi PowerShell-parancsot a forrás web app-információ (ebben az esetben a forrás-webappban neve):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

A ASE nevét, és az erőforrás-csoport nevét, amelyhez tartozik a ASE ismeretében a felhasználó a paranccsal új-AzureRmWebApp az új webalkalmazás létrehozása a meglévő ASE, az alábbi bemutatja, hogy:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

A hely paraméter régebbi ok miatt szükséges, de esetén az alkalmazás létrehozása egy ASE akkor figyelmen kívül hagyja. 

## <a name="cloning-an-existing-app-slot"></a>Egy meglévő alkalmazás tárolóhely klónozhatja ##

Alkalmazási helyzetek: A felhasználó szeretné egy meglévő Web App tárolóhely vagy egy új Web App alkalmazásban vagy egy új Web App tárolóhely klónozhatja. Az új webalkalmazás lehet ugyanabban a régióban, mint az eredeti Web App tárolóhely vagy egy másik régióbeli.

Az erőforrás-csoport nevét, amely tartalmazza a forrás web app ismeretében is használjuk az alábbi PowerShell-parancsot a forrás web app tárolóhely információ (ebben az esetben a forrás-webappslot neve) az adatforrás-webappban Web App alkalmazásban tartozik:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

A következő bemutatja az adatforrás webes alkalmazás új webalkalmazást átirattal létrehozása:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Adatforgalom Manager beállítása egy alkalmazás klónozhatja közben ##

Több területre web Apps alkalmazások létrehozásáról és Azure-forgalmat kezelő irányítja a forgalmat a web Apps alkalmazások beállítása, az n fontosságát annak biztosítására, hogy a vevők alkalmazások érhetők el erősen, amikor egy meglévő webalkalmazás klónozhatja, lehetősége van mindkét web Apps alkalmazások csatlakoztatása forgalom manager új profil vagy a meglévő - vegye figyelembe, hogy csak Azure erőforrás-kezelő a forgalom Manager verziója támogatott.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Egy alkalmazás klónozhatja forgalom Manager új profil létrehozása ###

Alkalmazási helyzetek: A felhasználó szeretné másolása egy másik területére, hogy egy webalkalmazás konfigurálása egy forgalom Azure erőforrás-kezelő-kezelő mindkét web Apps alkalmazások tartalmazó profilhoz közben. A következő bemutatja az adatforrás webes alkalmazás új webalkalmazást átirattal létrehozása új forgalom Manager profil beállítása:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Új hozzáadása klónozva Web App egy meglévő forgalom Manager profilhoz ###

Alkalmazási helyzetek: A felhasználó már van egy erőforrás-kezelő Azure forgalom manager profil ő amelyhez hozzá szeretné adni a két web Apps alkalmazások, a végpontok. Ehhez, azt először össze kell gyűjtenie a meglévő forgalom manager profil azonosítója, fel kell az előfizetés azonosítója, erőforrás-csoport nevét és a meglévő forgalom kezelő profil nevét.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

A forgalom kezelő azonosítóját, miután a következő mutatja be egy új web App alkalmazásban a forrás webalkalmazás átirattal létrehozása hozzáadása őket egy meglévő forgalom Manager profilhoz közben:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Aktuális korlátozások ##

Ez a funkció jelenleg előzetes verzióban, új funkciók hozzáadása időbeli dolgozunk, az alábbi lista az alkalmazás klónozhatja aktuális verziójának ismert korlátozások:

- Automatikus méretezés beállításai vannak nem klónozva
- Ütemezett biztonsági mentés beállításait a rendszer nem klónozva
- VNET beállítások vannak nem klónozva
- Alkalmazás háttérismeretek nem beállítása automatikusan megtörténik a célként megadott web App
- Egyszerű Auth beállítások vannak nem klónozva
- Kudu bővítmény nem vannak klónozva
- Tipp: szabályok vannak nem klónozva
- Adatbázis-tartalom nem vannak klónozva


### <a name="references"></a>Hivatkozások ###
- [Azure erőforrás-kezelő alapú PowerShell-parancsait Azure-webalkalmazások számára](app-service-web-app-azure-resource-manager-powershell.md)
- [Web App klónozhatja Azure portál használatával](app-service-web-app-cloning-portal.md)
- [Készítsen biztonsági másolatot az Azure alkalmazás szolgáltatás webalkalmazást](web-sites-backup.md)
- [Azure erőforrás-kezelő támogatási Azure forgalom Manager előzetes verzió](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Alkalmazás-szolgáltatási környezetben – bevezetés](app-service-app-service-environment-intro.md)
- [Azure PowerShell használatával a Azure-kezelő eszközzel](../powershell-azure-resource-manager.md)
