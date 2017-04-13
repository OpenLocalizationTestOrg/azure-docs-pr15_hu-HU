## <a name="setting-up-powershell-for-resource-manager-templates"></a>Erőforrás-kezelő sablonok PowerShell beállítása

Mielőtt használatba vehetné Azure PowerShell az erőforrás-kezelő, szüksége lesz a Windows PowerShell jobbra és Azure PowerShell-verziók.

### <a name="verify-powershell-versions"></a>A PowerShell verziószámának megállapítása

Ellenőrizze, hogy rendelkezik-e 3.0-s vagy 4.0-s verziója a Windows PowerShell-e. A Windows PowerShell verziója megkereséséhez ezt a parancsot a Windows PowerShell parancssorba.

    $PSVersionTable

A következő típusú adatokat fog kapni:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Győződjön meg arról, hogy **PSVersion** értéke 3.0-s vagy 4.0. Ha nem látható [a Windows Management Framework 3.0-s](http://www.microsoft.com/download/details.aspx?id=34595) vagy a [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Az Azure-fiók és az előfizetés beállítása

Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

Nyissa meg az Azure PowerShell a parancssor parancsot, és jelentkezzen be az Azure be ezzel a paranccsal.

    Login-AzureRmAccount

Ha több Azure előfizetéssel rendelkezik, ezzel a paranccsal a Azure előfizetések jeleníthet meg.

    Get-AzureRmSubscription

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

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Azure előfizetések és -fiókok kapcsolatos további tudnivalókért lásd [Útmutató: az előfizetés csatlakoztatása](powershell-install-configure.md#Connect).
