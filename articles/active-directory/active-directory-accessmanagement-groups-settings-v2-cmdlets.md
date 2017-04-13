<properties
    pageTitle="Csoport kezelése az Azure Active Directory Azure Active Directory PowerShell előzetes parancsmagok |} Microsoft Azure"
    description="Ezen az oldalon példákat mutat be a PowerShell segít a csoportoknak az Azure Active Directory kezelése"
    keywords="Azure Active Directory, Azure Active Directory PowerShell, csoportok, csoportok kezelése"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory előzetes parancsmagjai csoportok kezelése

> [AZURE.SELECTOR]
- [Azure portál](active-directory-groups-create-azure-portal.md)
- [Azure klasszikus portál](active-directory-accessmanagement-manage-groups.md)
- [A PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

A következő dokumentumot szolgáltatja a példákat az Azure Active Directory (Azure Active Directory) a csoportok kezelése a PowerShell használatával.  Azt is ismerteti az Azure Active Directory PowerShell előzetes modul beállításához. Először [Töltse le az Azure Active Directory PowerShell-modult](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>A Powershellhez Azure Active Directory modul telepítése

A AzureAD PowerShell előzetes modul telepítése az alábbi parancsokat használhatja:

    PS C:\Windows\system32> install-module azureadpreview

Ha ellenőrizni szeretné, hogy a kép modul telepítése, használja az alábbi parancsot:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Most már használatba lehet a parancsmagok a modul. A parancsmagok a AzureAD előzetes modulban teljes körű leírását olvassa el a [hivatkozás online dokumentációt](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## <a name="connecting-to-the-directory"></a>Csatlakozás a címtárhoz
Mielőtt elkezdi Azure Active Directory PowerShell előzetes parancsmagok segítségével a csoportok kezelése, csatlakoznia kell a PowerShell-munkamenetet a címtárban kezelni szeretné. Ehhez használja a következő parancsot:

    PS C:\Windows\system32> Connect-AzureAD -Force

A parancsmaggal kérni fogja a címtárában eléréséhez használni kívánt hitelesítő adatait. Ebben a példában használja azt karen@drumkit.onmicrosoft.com a bemutató címtár eléréséhez. A parancsmaggal ad vissza egy megerősítést kérő kattintva jelenítse meg a munkamenet sikeres volt-e csatlakoztatva a címtárhoz:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Most már használatba lehet a AzureAD előzetes parancsmagok kezelheti a csoportokat a címtárában található.

## <a name="retrieving-groups"></a>Csoportok beolvasása
A címtárában meglévő csoportokhoz beolvasni a Get-AzureADGroups parancsmag is használhatja. Minden csoport a címtárban beolvasásához, használja a paraméter nélkül a parancsmag:

    PS C:\Windows\system32> get-azureadgroup

A parancsmaggal a csatlakoztatott címtárban ad vissza az összes csoport.

A - objektumazonosító paraméter segítségével beolvashatja egy bizonyos csoporthoz, amelynek a csoport objektumazonosító megadása:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

A parancsmaggal most ad eredményül a csoportot, amelynek objektumazonosító megfelel a megadott paraméter értéke:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Egy adott csoport segítségével megkeresheti a - szűrő paramétert. A paraméter egy ODATA-szűrési feltétel megnyitja, és minden csoport, amelyek megegyeznek a szűrőt, ahogy az alábbi példa eredménye:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Figyelje meg, hogy a AzureAD PowerShell-parancsmagok végrehajtása az OData-lekérdezés szabvány, további információt talál [az alábbi](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Csoportok létrehozása
Új csoport létrehozása a címtárában található, használja a New-AzureADGroup parancsmag. Ezzel a parancsmaggal "Marketing" nevű új biztonsági csoport létrehozása:

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Csoportok frissítése
A Set-AzureADGroup parancsmag segítségével egy meglévő csoportba frissítéséhez. Ebben a példában azt módosítani szeretné a DisplayName tulajdonság a csoport "Intune rendszergazdák." Először is azt esetén megkeresi a csoportot, a Get-AzureADGroup parancsmaggal, majd a DisplayName attribútum szűrni:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Ezután azt módosítani szeretné a Leírás tulajdonság az új értékre "Intune eszköz rendszergazdák":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Ha ismét találja azt a csoportot, jól látható, a Leírás tulajdonság frissül megfelelően az új érték:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Csoport törlése
Csoportok a címtárból törléséhez használja a eltávolítása-AzureADGroup parancsmag az alábbi képlettel történik:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Csoportok tagjainak kezelése
Ha új tagok hozzáadása csoporthoz van szüksége, használja a Hozzáadás-AzureADGroupMember parancsmag. Ez a parancs felvétele a Intune rendszergazdák csoport, akkor az előző példában használt tagja:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

A - objektumazonosító paraméter annak a csoportnak, amelyben tag szeretnénk objektumazonosító, és annak a felhasználónak a csoportba vegye fel tagként szeretnénk objektumazonosító - RefObjectId.

A meglévő csoport tagjainak, használja a Get-AzureADGroupMember parancsmag következő példának megfelelően:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

A korábban jelöltük a csoport tag eltávolításához használja a eltávolítása-AzureADGroupMember parancsmag, mint az itt látható:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Ha ellenőrizni szeretné a csoport membership(s), hogy egy felhasználó, használja a Select-AzureADGroupIdsUserIsMemberOf parancsmag. Ezzel a parancsmaggal megnyitja a paraméterként annak a felhasználónak, amelynek ellenőrizni a csoporttagság objektumazonosító, és a csoportok, amelynek ellenőrizni a tagságot listája. A csoportok listája kell lennie, feltéve "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" típusú komplex változó formájában, így azt először létre kell hoznia egy változó típusát:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Ezután elvégezheti a szükséges értékeket a csoportazonosítókat "Csoportazonosítókat" a komplex változó attribútumban ellenőrzése:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Ezután jelölje be a felhasználó objektumazonosító 72cd4bbd-2594-40a2-935c-016f3cfeeeea szemben lévő csoportok $g csoporttagságát szeretnénk, ha azt kell használni:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


A visszaadott érték, amelynek tagja a felhasználó csoportok listája. Jelölje be a névjegyek, csoportok és szolgáltatás rendszerbiztonsági tagság adott listáját a csoportok szakaszban válassza-AzureADGroupIdsContactIsMemberOf, kijelölése-AzureADGroupIdsGroupIsMemberOf vagy kiválasztása-AzureADGroupIdsServicePrincipalIsMemberOf szeretné ezt a módszert is alkalmazhat

## <a name="managing-owners-of-groups"></a>A csoportok tulajdonosok kezelése
Tulajdonosok csoportba hozhatja létre a Hozzáadás-AzureADGroupOwner parancsmagot:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

A - objektumazonosító paraméter a tulajdonos hozzáadása szeretnénk csoport objektumazonosító, és - RefObjectId annak a felhasználónak, vegye fel a csoport tulajdonosa szeretnénk objektumazonosító.

A csoporttulajdonosok beolvasásához, használja a Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

A parancsmaggal ad eredményül a megadott csoport tulajdonosok listája:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Ha szeretne tulajdonos eltávolítása egy csoportból, használja a eltávolítása-AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Következő lépések

Az [Azure Active Directory-parancsmagok](http://go.microsoft.com/fwlink/p/?LinkId=808260)található további Azure Active Directory PowerShell dokumentációt.

* [Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok](active-directory-manage-groups.md)

* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
