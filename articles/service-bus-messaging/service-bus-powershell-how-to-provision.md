<properties
    pageTitle="Kezelése a PowerShell szolgáltatás Bus |} Microsoft Azure"
    description="Szolgáltatás Bus kezelése a PowerShell-parancsfájlokat"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>A PowerShell szolgáltatás Bus kezelése

## <a name="overview"></a>– Áttekintés

Microsoft Azure PowerShell a parancsfájlok futtatásának környezet ellenőrzése és a telepítési és Azure-ban a feladatok kezelésének automatizálásához használható. Ez a cikk ismerteti a kiépítése, majd a szolgáltatás Bus szervezetek, például névtér, a sorok és a helyi Azure PowerShell-konzolon esemény hubok kezelése a PowerShell használatával.

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt rendelkeznie kell az alábbi előfeltételek:

- Egy Azure-előfizetést. Azure szolgáltatás egy előfizetés-alapú platform. Előfizetés beszerzésével kapcsolatos további tudnivalókért lásd: a [Vételi beállítások][], [Tag kínál][], vagy az [Ingyenes próbaverziót][].

- Egy számítógép Azure PowerShell. Című cikkben olvashat [Telepítse és állítsa be a Azure PowerShell][].

- Egy PowerShell-parancsfájlokat, NuGet csomagokat és a .NET-keretrendszer általános értelmezését.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Többek között a .NET-összeállítás-szolgáltatás Bus mutató hivatkozás

Nincsenek PowerShell-parancsmagok korlátozott számú szolgáltatás Bus kezelésére szolgáló. Építse entitás, amely nem láthatóak a meglévő parancsmagok keresztül, a [szolgáltatás Bus NuGet csomag][]szolgáltatás Bus a .NET-ügyfél is használhatja.

Mindenekelőtt győződjön meg arról, hogy a parancsprogram keresse meg azt a **Microsoft.ServiceBus.dll** összeállítás, amely a NuGet csomag telepítve van-e. Ahhoz, hogy rugalmas, a parancsfájl ezeket a lépéseket hajtja végre:

1. Határozza meg, hogy az elérési utat, amelynél elindították.
2. Az elérési út rajta áthaladó, amíg nem talál egy nevű mappát `packages`. Ez a mappa NuGet csomagok telepítése során jön létre.
3. Rekurzív keresések a `packages` egy összeállítás **Microsoft.ServiceBus.dll**nevű mappát.
4. Az összeállítás hivatkozik, hogy a típusok érhetők el a későbbi használatra.

Az alábbiakban hogyan ezeket a lépéseket végrehajtása a PowerShell-parancsprogramot:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>A szolgáltatás Bus névtér kiépítése

Két PowerShell-parancsmagok támogatja a szolgáltatás Bus névtér műveleteket. A .NET SDK API-khoz helyett [Get-AzureSBNamespace][] és a [New-AzureSBNamespace][]is használhatja.

Ez a példa néhány helyi változót hoz létre a parancsfájl; `$Namespace` and `$Location`.

- `$Namespace`az a név, a szolgáltatás Bus névtér, amelyekkel elvégezheti szeretne dolgozni.
- `$Location`Az Adatközpont, amelyben a parancsfájl rendelkezések a névtér azonosítja.
- `$CurrentNamespace`a hivatkozás névtér, hogy a parancsprogram olvassa be (vagy hoz létre) tárolja.

Tényleges betűkkel `$Namespace` és `$Location` paraméterként kell.

Ez a parancsfájl részét az alábbi műveleteket végzi el:

1. Megkísérli beolvashatja a megadott nevű szolgáltatás Bus névteret.
2. Ha a névtér megtalálható, jelentések, mi található.
3. A névtér nem található, ha a névtér hoz létre, és majd olvassa be az újonnan létrehozott névtér.

    ```
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
    
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
    
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Más szolgáltatás Bus szervezetek kiépítése, készítése a [NamespaceManager][] osztály egy példányát a SDK csomagjában talál.
A [Get-AzureSBAuthorizationRule][] parancsmag beolvasásához, amelyek segítségével adja meg a kapcsolati karakterlánc engedélyezési szabály is használhatja. Azt fog tárolni a hivatkozást a `NamespaceManager` a példány a `$NamespaceManager` változó. Fogjuk használni `$NamespaceManager` újabb a parancsfájl kiépítése más szervezetek.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Más szolgáltatás Bus szervezetek kiépítése

Annak érdekében, hogy kiépítése más szervezetek, például a sorok, témák és esemény agyváltók, használja a [.NET API-szolgáltatás Bus][]. Ez a cikk csak koncentrál esemény hubok, de más szervezetek lépései hasonlítanak. Ezeken kívül a részletes példák, beleértve a más szervezetek, ez a cikk végén hivatkozunk.

Ez a parancsfájl részét négy több helyi változót hoz létre. Ezek a változók használatával hozható létre egy `EventHubDescription` objektumot. A parancsfájl hajtja végre az alábbi műveleteket:

1. Használja a `NamespaceManager` objektum, ellenőrizze, hogy ha az esemény-központban azonosított `$Path` létezik.
2. Ha még nem létezik, hozzon létre egy `EventHubDescription` , hogy adják a `NamespaceManager` osztály `CreateEventHubIfNotExists` módszer.
3. Annak megállapítása, hogy az esemény-központban érhető el, miután létrehozása fogyasztói csoport `ConsumerGroupDescription` és `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
        
    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>A névtér áttelepítése másik Azure-előfizetésre

A következő parancssorozat névteret Azure előfizetésből a másikra lép. Ez a művelet végrehajtása, a névtér már aktívnak kell lennie, és a felhasználó a PowerShell-parancsot futtató rendszergazdai jogosultságokkal kell rendelkeznie a forrás- és célwebhelyek előfizetések.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Következő lépések

Ez a cikk egyszerű szerkezetet előírt kiépítési szolgáltatás Bus szervezetek PowerShell használatával. Valamit, hogy végezheti el az ügyfél .NET tárak használata, is megteheti az egy PowerShell-parancsprogramot.

Nincsenek további részletes példák a következő blogbejegyzések:

- [Hogyan hozhat létre a szolgáltatás Bus sorban várakozó, témák és előfizetésekből egy PowerShell-parancsprogramot használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Hogyan hozhatók létre egy szolgáltatás Bus Namespace és egy esemény hubhoz egy PowerShell-parancsprogramot használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Néhány előre elkészített parancsfájlok letöltési is érhetők el:
- [A szolgáltatás Bus PowerShell-parancsfájlokat](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Vásárlás beállításai]: http://azure.microsoft.com/pricing/purchase-options/
[Tag ajánlatok]: http://azure.microsoft.com/pricing/member-offers/
[Ingyenes próbaverzió]: http://azure.microsoft.com/pricing/free-trial/
[Telepítse és állítsa be a Azure PowerShell]: ../powershell-install-configure.md
[Szolgáltatás Bus NuGet csomag]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Új AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[A szolgáltatás Bus .NET API]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
