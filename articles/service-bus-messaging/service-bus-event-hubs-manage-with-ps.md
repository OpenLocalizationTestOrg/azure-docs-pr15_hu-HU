<properties
    pageTitle="Szolgáltatás Bus és esemény hubok erőforrások kezelése a PowerShell használatával |} Microsoft Azure"
    description="PowerShell használatával hozhat létre és kezelhet szolgáltatás Bus és esemény hubok erőforrások"
    services="service-bus,event-hubs"
    documentationCenter=".NET"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="use-powershell-to-manage-service-bus-and-event-hubs-resources"></a>Szolgáltatás Bus és esemény hubok erőforrások kezelése a PowerShell használatával

Microsoft Azure PowerShell a parancsfájlok futtatásának környezet ellenőrzése és automatizálhatja a telepítési és Azure szolgáltatás kezelésére használható. Ez a cikk ismerteti a kiépítése, majd a szolgáltatás Bus szervezetek, például névtér, a sorok és a helyi Azure PowerShell-konzolon esemény hubok kezelése a PowerShell használatával.

## <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, szüksége lesz az alábbiakat:

- Egy Azure-előfizetést. Azure szolgáltatás egy előfizetés-alapú platform. Előfizetés beszerzésével kapcsolatos további tudnivalókért lásd: a [Beállítások megvásárlásához][], [tag kínál][]és [ingyenes fiókot][].

- Egy számítógép Azure PowerShell. Című cikkben olvashat [Telepítse és állítsa be a Azure PowerShell][].

- Egy PowerShell-parancsfájlokat, NuGet csomagokat és a .NET-keretrendszer általános értelmezését.

## <a name="include-a-reference-to-the-net-assembly-for-service-bus"></a>A .NET-összeállítás-szolgáltatás Bus hivatkozást tartalmaz

Nincsenek PowerShell-parancsmagok korlátozott számú szolgáltatás Bus kezelésére szolgáló. Hozhatók létre, amely a meglévő parancsmagok keresztül nem láthatóak szervezetek, is használhatja a .NET-ügyfél-szolgáltatás Bus a PowerShell belül a [szolgáltatás Bus NuGet csomag]hivatkozó.

Mindenekelőtt győződjön meg arról, hogy a parancsprogram keresse meg azt a **Microsoft.ServiceBus.dll** összeállítás, amely a NuGet csomag telepítve van-e. Ahhoz, hogy rugalmas, a parancsfájl ezeket a lépéseket hajtja végre:

1. Határozza meg, hogy az elérési út meghívott volt.
2. Az elérési út rajta áthaladó, amíg nem talál egy nevű mappát `packages`. Ez a mappa NuGet csomagok telepítése során jön létre.
3. Rekurzív keresések a `packages` egy összeállítás **Microsoft.ServiceBus.dll**nevű mappát.
4. Az összeállítás hivatkozik, hogy a típusok érhetők el a későbbi használatra.

Az alábbiakban hogyan ezeket a lépéseket végrehajtása a PowerShell-parancsprogramot:

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}

```

## <a name="provision-a-service-bus-namespace"></a>A szolgáltatás Bus névtér kiépítése

Amikor szolgáltatás Bus névtér, vannak-e a .NET SDK helyett használhatja két parancsmagok: [Get-AzureSBNamespace][] és a [New-AzureSBNamespace][].

Ez a példa néhány helyi változót hoz létre a parancsfájl; `$Namespace` and `$Location`.

- `$Namespace`a neve, az a szolgáltatás Bus névtér, amelyekkel elvégezheti szeretne dolgozni.
- `$Location`Az Adatközpont, amely azonosítja fogja azt a névtér kiépítése.
- `$CurrentNamespace`a hivatkozás névtér, amely azt beolvasásához (vagy hozzon létre) tárolja.

Tényleges betűkkel `$Namespace` és `$Location` paraméterként kell.

Ez a parancsfájl részét az alábbi műveleteket végzi el:

1. Megkísérli beolvashatja a megadott nevű szolgáltatás Bus névteret.
2. Ha a névtér megtalálható, jelentések, mi található.
3. A névtér nem található, ha a névtér hoz létre, és majd olvassa be az újonnan létrehozott névtér.

    ``` powershell

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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
Más szolgáltatás Bus szervezetek kiépítése, hozzon létre egy példányát a `NamespaceManager` a SDK objektumot. A [Get-AzureSBAuthorizationRule][] parancsmag beolvasásához, amelyek segítségével adja meg a kapcsolati karakterlánc engedélyezési szabály is használhatja. Ez a példa mutató hivatkozást is tárol a `NamespaceManager` a példány a `$NamespaceManager` változó. A későbbi parancsfájl `$NamespaceManager` más szervezetek hozhatók létre.

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## <a name="provisioning-other-service-bus-entities"></a>Más szolgáltatás Bus szervezetek kiépítése

Más szervezetek sorban várakozó, témák és esemény agyváltók, például kiépítése a [.NET API-szolgáltatás Bus][]is használhatja. Részletes példák, beleértve a más szervezetek hivatkozunk, ez a cikk végén.

### <a name="create-an-event-hub"></a>Hozzon létre egy esemény hubhoz

Ez a parancsfájl részét négy több helyi változót hoz létre. Ezek a változók használatával hozható létre egy `EventHubDescription` objektumot. A parancsfájl az alábbi műveleteket végzi el:

1. Használja a `NamespaceManager` objektum, ellenőrizze, hogy ha az esemény-központban azonosított `$Path` létezik.
2. Ha még nem létezik, hozzon létre egy `EventHubDescription` , hogy adják a `NamespaceManager` osztály `CreateEventHubIfNotExists` módszer.
3. Annak megállapítása, hogy az esemény-központban érhető el, miután létrehozása fogyasztói csoport `ConsumerGroupDescription` és `NamespaceManager`.

    ``` powershell

    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
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

### <a name="create-a-queue"></a>Várólista létrehozása

A várakozási vagy témakör létrehozása, ahogy az előző szakaszban névtér ellenőrzést végezni.

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### <a name="create-a-topic"></a>Témakör létrehozása

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## <a name="next-steps"></a>Következő lépések

Ez a cikk ellátni, egyszerű munkafolyamat kialakítási szolgáltatás Bus szervezetek PowerShell használatával. Bár nem érhetők el PowerShell-parancsmagok korlátozott számú szolgáltatás Bus kezelésére szolgáló személyek, üzenetküldés a Microsoft.ServiceBus.dll összeállítás, gyakorlatilag bármilyen művelet hivatkozó használatával végezhet a .NET ügyfél tárak egy PowerShell-parancsprogramot is elvégezhető.

Nincsenek további részletes példák az alábbi blogbejegyzések:

- [Hogyan hozhat létre a szolgáltatás Bus sorban várakozó, témák és előfizetésekből egy PowerShell-parancsprogramot használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Hogyan hozhatók létre egy szolgáltatás Bus Namespace és egy esemény hubhoz egy PowerShell-parancsprogramot használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Néhány előre elkészített parancsfájlok letöltési is érhetők el:

- [A szolgáltatás Bus PowerShell-parancsfájlokat](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[vásárlás beállításai]: http://azure.microsoft.com/pricing/purchase-options/
[tag ajánlatok]: http://azure.microsoft.com/pricing/member-offers/
[ingyenes fiókkal.]: http://azure.microsoft.com/pricing/free-trial/
[Szolgáltatás Bus NuGet csomag]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Új AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[A szolgáltatás Bus .NET API]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx
[Telepítse és állítsa be a Azure PowerShell]: ../powershell-install-configure.md
