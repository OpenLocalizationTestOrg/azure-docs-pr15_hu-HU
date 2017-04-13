<properties
    pageTitle="Egyéni szerepkörök Azure RBAC |} Microsoft Azure"
    description="Megtudhatja, hogy miként pontosabb Identitáskezelés Azure Role-Based hozzáférés-vezérlés az egyéni szerepkörök definiálása az Azure-előfizetésben."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>Azure RBAC egyéni szerepkörök


Hozzon létre egy egyéni szerepkör Azure Role-Based Access vezérlő (RBAC) Ha a beépített szerepkörök egyik sem felel meg, hogy az adott access igényeinek. Egyéni szerepkörök [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure parancssori felület](role-based-access-control-manage-access-azure-cli.md) (CLI) és a [REST API](role-based-access-control-manage-access-rest.md)hozhat létre. Beépített szerepkörök, hasonlóan a felhasználók, csoportok és alkalmazásokat előfizetés, erőforráscsoport és erőforrás-tartományok egyéni szerepkörök rendelhetők. Egyéni szerepkörök Azure AD-ös bérlői vannak tárolva, és azt használni az Azure Active directory számára a subsciption összes előfizetéshez megoszthatók legyenek.

Az alábbi képen egy egyéni szerepkör figyelemmel kísérésére és a virtuális gépeken futó újraindítását:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Műveletek
A **Műveletek** tulajdonság, egy egyéni szerepkör határozza meg, hogy az Azure műveletek, amelyhez hozzáférést biztosít a szerepkör. Gyűjteménye, amely azonosítja az Azure erőforrás-szolgáltatók biztonságos műveletek művelet karakterláncok. A művelet karakterláncok helyettesítő karaktereket tartalmazó (\*), amelyek megegyeznek a művelet karakterlánc összes művelet hozzáférést. Például:

-   `*/read`További műveletek az összes típusú erőforrás összes Azure erőforrás-szolgáltató hozzáférést biztosít.
-   `Microsoft.Network/*/read`További műveletek az összes erőforrás típusú a Microsoft.Network erőforrás-szolgáltató az Azure hozzáférést biztosít.
-   `Microsoft.Compute/virtualMachines/*`a virtuális gépeken futó és az annak alárendelt összes művelet erőforrástípus hozzáférést biztosít.
-   `Microsoft.Web/sites/restart/Action`Indítsa újra a webhelyek hozzáférést biztosít.

Használat `Get-AzureRmProviderOperation` (a PowerShell) vagy `azure provider operations show` (az Azure CLI) az Azure erőforrás-szolgáltatók műveletek. Ezek a parancsok győződjön meg arról, hogy egy művelet karakterlánc érvényes, és bontsa ki a helyettesítő művelet karakterláncok is használhatja.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![A PowerShell képernyőkép - Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action |} OperationName művelet láb](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI képernyőképe - azure szolgáltató műveletek megjelenítése "Microsoft.Compute/virtualMachines/\*/művelet" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
**NotActions** tulajdonságot használja, ha engedélyezni szeretné műveletek megadása könnyebben, hiszen kizárhat korlátozott műveletek van meghatározva. Az access egy egyéni szerepkör által biztosított, hogy kivonjuk a **Műveletek** műveletek **NotActions** műveletek számítja ki.

> [AZURE.NOTE] Ha egy felhasználó egy szerepkört, amely nem tartalmazza a **NotActions**művelet, és van rendelve, amely ugyanannak a műveletnek hozzáférést biztosít a második szerepkörbe van hozzárendelve, a felhasználó lesz jogosult művelet elvégzéséhez. **NotActions** nem megtagadás szabály – egyszerűen kényelmesen engedélyezett műveletek készlet létrehozása, ha a meghatározott műveletekhez kell zárni.

## <a name="assignablescopes"></a>AssignableScopes
Az egyéni szerepkör **AssignableScopes** tulajdonsága Itt adhatja meg, amelyen belül az egyéni szerepkör érhető el a hozzárendelés hatókörök (előfizetések, az erőforrás csoportok vagy az erőforrások). Elérhetővé teheti az egyéni szerepkör-hozzárendelés csak a előfizetések vagy az erőforrás-csoportokat, amelyekhez szükséges, és nem a felhasználói felület – a többség az előfizetések vagy az erőforrás csoportok átnézendők.

Érvényes hozzárendelhető hatókörök példája:

-   "/ előfizetések/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ előfizetések/e91d47c4-76f3-4271-a796-21b4ecfe3624" - elérhetővé teszi a szerepkör-hozzárendelés két foglalt.
-   "/ előfizetések/c276fc76-9cd4-44c9-99a7-4fd71546436e" - elérhetővé tétele a szerepkör a hozzárendelés egyetlen-előfizetésben.
-  "/ előfizetések/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/hálózati" - elérhetővé tétele a szerepkör a hozzárendelés csak a hálózati erőforrás csoportban.

> [AZURE.NOTE] Használjon legalább egy előfizetés, erőforráscsoport vagy erőforrás-azonosító.

## <a name="custom-roles-access-control"></a>Egyéni szerepkörök hozzáférés-vezérlés
Az egyéni szerepkör **AssignableScopes** tulajdonsága vezérli ki megtekintése, módosítása és törlése a szerepkört is.

- Ki hozhat létre egy egyéni szerepkör?
    Tulajdonosok (és felhasználói hozzáférés rendszergazdák) előfizetések, erőforrás-csoportok és az erőforrások hozhat létre egyéni szerepkörök használatra e hatókörök.
    A felhasználó létrehozása a szerepkör kell hajtható végre `Microsoft.Authorization/roleDefinition/write` a szerepkör a **AssignableScopes** műveletet.

- Egyéni szerepkört módosítására jogosult?
    Tulajdonosok (és felhasználói hozzáférés rendszergazdák) előfizetések, az erőforrás-csoportok és az erőforrások módosíthatja az adott tartományok egyéni szerepkörök. A felhasználóknak kell tud végezni a `Microsoft.Authorization/roleDefinition/write` egyéni szerepkört a **AssignableScopes** műveletet.

- Kik tekinthetik meg egyéni szerepkörök?
    Azure RBAC az összes beépített szerepkörének érhetők el a hozzárendelés szerepköröket megtekintésének engedélyezése. A felhasználók, akik végre tud hajtani a `Microsoft.Authorization/roleDefinition/read` hatókör művelet érhetők el, hogy a hatókör hozzárendelés RBAC szerepkörökkel is megtekintheti.

## <a name="see-also"></a>Lásd még:
- [Szerepkör-alapú hozzáférés szabályozása](role-based-access-control-configure.md): Ismerkedés a RBAC az Azure-portálon.
- Megtudhatja, hogy miként hozzáférés kezelése:
    - [A PowerShell](role-based-access-control-manage-access-powershell.md)
    - [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
    - [REST API-VAL](role-based-access-control-manage-access-rest.md)
- [Beépített szerepkörök](role-based-access-built-in-roles.md): részleteket szeretne megtudni a szabványos RBAC a szerepköröket.
