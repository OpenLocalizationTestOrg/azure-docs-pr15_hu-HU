<properties 
    pageTitle="Üzembe és kezelheti a értesítés PowerShell használatával" 
    description="Hogyan hozhat létre és kezelése a PowerShell használata az automatizálási értesítés hubok" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Üzembe és kezelheti a értesítés PowerShell használatával

##<a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan létrehozása és kezelése Azure értesítés hubok PowerShell használatával. Ez a témakör a következő gyakori feladatok automatizálása látható.

+ Hozzon létre egy értesítés hubhoz
+ Hitelesítő adatainak beállítása

Ha meg kell hozzon létre egy új szolgáltatás bus névtér az értesítési hubok, olvassa el a [Szolgáltatás Bus kezelése a PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Értesítések hubok kezelése közvetlenül a részét képező Azure PowerShell-parancsmagokkal által nem támogatott. A legjobb megközelítés a PowerShell rendszer Microsoft.Azure.NotificationHubs.dll összeállítás hivatkozni szeretne. Az összeállítás a [Microsoft Azure értesítés hubok NuGet csomag](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)van meghatározva.


## <a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- Egy Azure-előfizetést. Azure szolgáltatás egy előfizetés-alapú platform. Előfizetés beszerzésével kapcsolatos további tudnivalókért lásd: a [Vételi beállítások], [Tag kínál], vagy az [Ingyenes próbaverziót].

- Egy számítógép Azure PowerShell. Című cikkben olvashat [Telepítse és állítsa be a Azure PowerShell].

- Egy PowerShell-parancsfájlokat, NuGet csomagokat és a .NET-keretrendszer általános értelmezését.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Többek között a .NET-összeállítás-szolgáltatás Bus mutató hivatkozás

Azure értesítés hubok kezelése még nem szerepel az Azure PowerShellben PowerShell-parancsmagok. Értesítés hubok kiépítése, a .NET-ügyfél, a [Microsoft Azure értesítés hubok NuGet csomag](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)is használhatja.

Először győződjön meg arról, hogy a parancsprogram keresse meg azt a **Microsoft.Azure.NotificationHubs.dll** összeállítás, amelyen telepítve van a Visual Studio projekt NuGet csomag. Ahhoz, hogy rugalmas, a parancsfájl ezeket a lépéseket hajtja végre:

1. Határozza meg, hogy az elérési utat, amelynél elindították.
2. Az elérési út rajta áthaladó, amíg nem talál egy nevű mappát `packages`. Ez a mappa Visual Studio projektekhez NuGet csomagok telepítése során jön létre.
3. Rekurzív keresések a `packages` egy összeállítás **Microsoft.Azure.NotificationHubs.dll**nevű mappát.
4. Az összeállítás hivatkozik, hogy a típusok érhetők el a későbbi használatra.

Az alábbiakban hogyan ezeket a lépéseket végrehajtása a PowerShell-parancsprogramot:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>A NamespaceManager osztály létrehozása

Értesítés hubok kiépítése, készítése a [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) osztály egy példányát a SDK csomagjában talál. 

A [Get-AzureSBAuthorizationRule] parancsmag Azure PowerShell részét képező beolvasásához, amelyek segítségével adja meg a kapcsolati karakterlánc engedélyezési szabály is használhatja. Azt fog tárolni a hivatkozást a `NamespaceManager` a példány a `$NamespaceManager` változó. Fogjuk használni `$NamespaceManager` egy értesítés hubhoz hozhatók létre.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Egy új értesítés központi webhely kiépítése 

Hozhatók létre egy új értesítés hubon keresztül csatlakozott, használja az [Értesítés hubok .NET API -val].

Ez a parancsfájl részén beállítása négy helyi változót. 

1. `$Namespace`: Ezeket a beállításokat a névtér nevét, amelyre értesítés hubon létrehozásához.
2. `$Path`: Állítsa ezt az elérési utat a nevét az új értesítés-központban.  Ha például "MyHub".    
3. `$WnsPackageSid`: Állítsa ezt az előkészítés biztonsági AZONOSÍTÓK meg a Windows-alkalmazás [Windows fejlesztői központ](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Állítsa ezt a titkos kulcs meg a Windows-alkalmazás [Windows fejlesztői központ](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Ezek a változók szolgálnak a névtér csatlakozzon, és hozzon létre egy új értesítés hubhoz konfigurálva a Windows-alkalmazás WNS hitelesítő adatokkal a Windows értesítési szolgáltatások (WNS) értesítések kezelése. A csomag beszerzése információkat biztonsági AZONOSÍTÓK és titkos kulcs megjelenítéséhez az [Értesítési hubok – első lépések](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) oktatóprogram. 

+ A parancsprogram kódtöredékének használja a `NamespaceManager` objektum ellenőrizze, hogy ha az értesítési-központban azonosított `$Path` létezik.

+ Ha még nem létezik, a parancsfájl hoz létre egy `NotificationHubDescription` WNS a hitelesítő adatait, és adják át, hogy a `NamespaceManager` osztály `CreateNotificationHub` módot.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>További források

- [A PowerShell szolgáltatás Bus kezelése](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Hogyan hozhat létre a szolgáltatás Bus sorban várakozó, témák és előfizetésekből egy PowerShell-parancsprogramot használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Hogyan hozhatók létre egy szolgáltatás Bus Namespace és egy esemény hubhoz egy PowerShell-parancsprogramot használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Néhány előre elkészített parancsfájlok letöltési is érhetők el:
- [A szolgáltatás Bus PowerShell-parancsfájlokat](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Vásárlás beállításai]: http://azure.microsoft.com/pricing/purchase-options/
[Tag ajánlatok]: http://azure.microsoft.com/pricing/member-offers/
[Ingyenes próbaverzió]: http://azure.microsoft.com/pricing/free-trial/
[Telepítse és állítsa be a Azure PowerShell]: ../powershell-install-configure.md
[Az értesítés hubok .NET API]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
