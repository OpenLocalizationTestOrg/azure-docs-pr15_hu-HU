<properties
    pageTitle="Access módosítási előzmények jelentés létrehozása |} Microsoft Azure"
    description="Jelentés készítése, az Access alkalmazásban az Azure-előfizetésekhez szerepköralapú hozzáférés-vezérlés az elmúlt 90 napon felett a minden módosítás sorolja fel."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="create-an-access-change-history-report"></a>Access módosítási előzmények jelentés létrehozása

Bármikor valaki ad meg, illetve belül az előfizetések, hozzáférés visszavonása a módosítások első bejelentkezve Azure eseményeket. Access módosítási előzmények jelentések összes módosítás megtekintéséhez az elmúlt 90 napig hozhat létre.

## <a name="create-a-report-with-azure-powershell"></a>Azure PowerShell-jelentés létrehozása
A PowerShell access módosítási előzmények jelentést hozhat létre a `Get-AzureRMAuthorizationChangeLog` parancsot. Többet szeretne tudni az ezzel a parancsmaggal a [PowerShell gyűjtemény](https://www.powershellgallery.com/packages/AzureRM.Storage/1.0.6/Content/ResourceManagerStartup.ps1)érhetők el.

Amikor felhívja ezt a parancsot, mely a hozzárendelések, többek között a következőkhöz feltüntetni kívánt tulajdonsága adhatja meg:

| A tulajdonság | Leírás |
| -------- | ----------- |
| **Művelet** | Hogy hozzáférési jogosultsággal vagy visszavont |
| **Hívóazonosító** | A tulajdonos felelős access módosítása |
| **Dátum** | A dátum és idő access megváltozott. |
| **Könyvtárnév** | Az Azure Active directory |
| **PrincipalName** | A felhasználó, csoport vagy alkalmazás nevét |
| **PrincipalType** | A hozzárendelés felhasználói, csoport vagy alkalmazás volt-e |
| **RoleId** | A nyújtott és visszavonva szerepkör a globálisan egyedi azonosítója |
| **RoleName** | A nyújtott és visszavonva szerepkör |
| **ScopeName** | Az előfizetést, az erőforráscsoport vagy az erőforrás neve |
| **ScopeType** | A hozzárendelés, az előfizetést, az erőforráscsoport vagy az erőforrás hatókör volt-e |
| **SubscriptionId** | Az Azure előfizetés a globálisan egyedi azonosítója |
| **SubscriptionName** | Az Azure előfizetés neve |

A példában a parancs az előfizetést, az utóbbi hét napban az összes access módosításának sorolja fel:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![A PowerShell Get-AzureRMAuthorizationChangeLog - képernyőképe](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Jelentés létrehozása az Azure CLI
Access módosítási előzmények jelentés létrehozása az Azure parancssori kezelőfelületről, használja a `azure role assignment changelog list` parancsot.

## <a name="export-to-a-spreadsheet"></a>Táblázat exportálása
Menti a jelentést, és az adatok módosítására, a hozzáférés módosításokat egy CSV-fájlba exportálhatja. Ezután megtekintheti a jelentés véleményezésre a számolótáblában.

![Változásnaplója tekinteni a számolótábla - képernyőképe](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="see-also"></a>Lásd még:
- Első lépések a [Azure Role-Based hozzáférés-vezérlés](role-based-access-control-configure.md)
- [Egyéni szerepkörök Azure RBAC](role-based-access-control-custom-roles.md) használata
