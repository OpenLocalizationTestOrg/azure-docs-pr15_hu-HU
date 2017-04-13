<properties 
    pageTitle="Azure Keresés kezelése a Powershell-parancsfájlokat |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás" 
    description="A PowerShell-parancsfájlokat az Azure keresési-szolgáltatás kezelése. Hozzon létre vagy frissítése az Azure keresési szolgáltatás és Azure keresési felügyeleti kulcsok kezelése" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>A PowerShell az Azure keresési szolgáltatás kezelése
> [AZURE.SELECTOR]
- [Portál](search-manage.md)
- [A PowerShell](search-manage-powershell.md)
- [REST API-VAL](search-get-started-management-api.md)

Ez a témakör ismerteti az Azure keresési szolgáltatás felügyeleti feladatok végrehajtása a PowerShell-parancsait. Fog végigvezetjük keresési szolgáltatás létrehozása, azt méretezése és kezelése az API-kulcsok.
Ezek a parancsok párhuzamos elérhető az [Azure Keresés kezelése REST API -t](http://msdn.microsoft.com/library/dn832684.aspx)az adatkezelési beállítások.

## <a name="prerequisites"></a>Előfeltételek
 
- Azure PowerShell-s vagy újabb 1,0 kell rendelkeznie. Című cikkben olvashat [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md).
- Az alábbiakban leírtak szerint PowerShell Azure-előfizetéséhez kell bejelentkeznie.

Első lépésként meg kell ezzel a paranccsal Azure bejelentkezési:

    Login-AzureRmAccount

A Microsoft Azure bejelentkezési párbeszédpanel az e-mail címet az Azure-fiók és a jelszó megadása

Azt is megteheti azt is megteheti [Bejelentkezés egyszerű szolgáltatás nem interaktív módon](../resource-group-authenticate-service-principal.md).

Ha több Azure előfizetéssel rendelkezik, kell beállítania az Azure előfizetés. Az aktuális előfizetések listáját, futtassa a parancsot.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Adja meg az előfizetést, futtassa a következő parancsot. A következő példában az előfizetés neve van `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Parancsok segít első lépések

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Következő lépések
    
Most, hogy a szolgáltatás hoz létre, a következő lépésekkel készíthet: [index](search-what-is-an-index.md), [index lekérdezés](search-query-overview.md)összeállítása és végül létrehozása és kezelése a saját keresőalkalmazás Azure keresési használó.

- [Azure keresési index létrehozása az Azure-portálon](search-create-index-portal.md)

- [A lekérdezés keresési Intézővel az Azure-portálon Azure keresési index](search-explorer.md)

- [Adatok betöltése más szolgáltatások indexkiszolgáló beállítása](search-indexer-overview.md)

- [Azure keresés használatáról a .NET rendszerhez](search-howto-dotnet-sdk.md)

- [A keresés Azure forgalom elemzése](search-traffic-analytics.md)
