<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="rasquill" />

## <a name="setting-up-powershell"></a>Állítsa be a PowerShell

Azure PowerShell használata előtt kövesse az alábbi lépéseket.

### <a name="verify-powershell-versions"></a>A PowerShell verziószámának megállapítása

A Windows PowerShell használata előtt rendelkeznie kell a Windows PowerShell, 3.0-s verziója vagy 4.0. A Windows PowerShell verziója megkereséséhez ezt a parancsot a Windows PowerShell parancssorba.

    $PSVersionTable

Megjelenik az alábbihoz hasonló.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Győződjön meg arról, hogy **PSVersion** értéke 3.0-s vagy 4.0. Kompatibilis verzió telepítéséhez olvassa el a [Windows Management Framework 3.0-s](http://www.microsoft.com/download/details.aspx?id=34595) vagy a [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)című témakört.

Is van, hogy Azure PowerShell verzió 0.8.0 vagy újabb verziójában. Ellenőrizheti, hogy az Azure PowerShell ezzel a paranccsal a Azure PowerShell-parancssorában telepített verziója.

    Get-Module azure | format-table version

Megjelenik az alábbihoz hasonló.

    Version
    -------
    0.8.16.1

Az útmutatók és a legújabb verzióra mutató hivatkozást megtudhatja, [hogy miként telepítése és beállítása Azure PowerShell](powershell-install-configure.md).


### <a name="set-your-azure-account-and-subscription"></a>Az Azure-fiók és az előfizetés beállítása

Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

Nyissa meg az Azure PowerShell a parancssor parancsot, és jelentkezzen be az Azure be ezzel a paranccsal.

    Add-AzureAccount

Ha több Azure előfizetéssel rendelkezik, ezzel a paranccsal a Azure előfizetések jeleníthet meg.

    Get-AzureSubscription

A következő típusú adatokat fog kapni:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Beállíthatja, hogy az aktuális Azure előfizetés ezeket a parancsokat a Azure PowerShell-parancssorában futtatásával. Mindent, ami az idézőjelekkel együtt cseréje a < és > karaktert, a helyes nevet.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Azure előfizetések és -fiókok kapcsolatos további tudnivalókért lásd [Útmutató: az előfizetés csatlakoztatása](powershell-install-configure.md#Connect).
