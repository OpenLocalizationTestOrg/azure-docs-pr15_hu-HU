<properties
    pageTitle="Szerepköralapú hozzáférés-szerepalapú Azure PowerShell kezelése |} Microsoft Azure"
    description="Azure PowerShell, beleértve a szerepköröket, szerepkörök kiosztása és törlése a szerepkör-hozzárendelések RBAC kezelésének módját."
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
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Azure PowerShell szerepköralapú hozzáférés-vezérlőt kezelése

> [AZURE.SELECTOR]
- [A PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API-VAL](role-based-access-control-manage-access-rest.md)


Szerepköralapú hozzáférés szerepalapú az Azure-portálon és Azure erőforrás-kezelés API segítségével hozzáférés kezelése a külön szinten-előfizetéséhez. Ezzel a szolgáltatással néhány szerepköröket rendel őket egy adott hatókört a access, az Active Directory-felhasználók, csoportok vagy szolgáltatás rendszerbiztonsági biztosíthat.

Mielőtt a PowerShell használatával RBAC kezelése, az alábbi kell rendelkeznie:

- Azure PowerShell verzió 0.8.8 vagy újabb verziójában. A legújabb verzió telepítéséhez és társíthatja az Azure előfizetés, megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).

- Azure erőforrás-kezelő parancsmagok. Telepítse az [Azure erőforrás-kezelő parancsmagok](https://msdn.microsoft.com/library/mt125356.aspx) PowerShell.

## <a name="list-roles"></a>Lista-szerepkörök

### <a name="list-all-available-roles"></a>Rendelkezésre álló összes szerepkörének felsorolása
Lista RBAC szerepköröket hozzárendelés érhetők el, és nézze meg a műveletek, amelyhez azok hozzáférést, használja a `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell-Get AzureRmRoleDefinition - képernyőképe](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Listaműveletek: a szerepkör
A műveletek az adott szerepkörhöz listában használja `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell-Get AzureRmRoleDefinition egy adott szerepkör - képernyőképe](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Hozzáféréssel rendelkezők megtekintése
Access-hozzárendelések RBAC lista, használhatja a `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Egy adott hatóköre a lista szerepkör-hozzárendelés
Egy megadott előfizetés, erőforráscsoport vagy egy erőforrás az access-hozzárendelések megtekintése Ha például az összes aktív hozzárendeléseinek megtekintéséhez erőforráscsoport, használja `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment erőforráscsoport - képernyőképe](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Egy felhasználó szerepköreit lista
Adott felhasználóhoz rendelt összes szerepkörének és a szerepkörökhöz rendelt a felhasználó csoportjának listáját, használjon `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment a felhasználó - képernyőképe](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Lista klasszikus szolgáltatás rendszergazdája és a coadmin szerepkör-hozzárendelés
A lista az access-hozzárendelésekhez klasszikus előfizetés rendszergazda és coadministrators, használata:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Engedélyek biztosítása
### <a name="search-for-object-ids"></a>Objektum azonosítók keresése
Rendeljen egy szerepkört, hogy Önnek kell azonosítania a objektum (felhasználói, csoport vagy alkalmazás) és a hatókör is.

Ha nem tudja, hogy az előfizetés azonosítója, az **előfizetések** lap az Azure portálon is talál. Megtudhatja, hogy miként lekérdezhetők az előfizetés azonosítója, lásd: a [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) MSDN webhelyen.

Az objektum azonosító egy Azure AD-csoport, használja:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Azure AD-szolgáltatás egyszerű vagy alkalmazás Objektumazonosító, használja:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Rendeljen egy szerepkört, az előfizetés hatókör az alkalmazás
Az előfizetés hatókörét, az alkalmazás hozzáférést, használhatja:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShell új AzureRmRoleAssignment - képernyőképe](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>A szerepkör hozzárendelése egy felhasználóhoz, az erőforrás csoport hatóköre a
Az erőforrás csoport hatóköre a felhasználó hozzáférést, használhatja:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell új AzureRmRoleAssignment - képernyőképe](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Rendeljen egy szerepkört, az erőforrás hatókör csoport
Ad hozzáférést, az erőforrás hatókör csoport használata:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell új AzureRmRoleAssignment - képernyőképe](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>A hozzáférés-eltávolítása
Ha el szeretné távolítani a felhasználók, csoportok és alkalmazások használata az access:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell-eltávolítása AzureRmRoleAssignment - képernyőképe](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Hozzon létre egy egyéni szerepkört
Hozzon létre egy egyéni szerepkört, használja a `New-AzureRmRoleDefinition` parancsot.

Amikor létrehoz egy egyéni szerepkört PowerShell használatával, akkor indítsa el a [beépített szerepkörök](role-based-access-built-in-roles.md)egyikét. A *Műveletek*, *notActions*és *tartományok* , amelyet a jellemzőket szerkesztése, és mentse a módosításokat egy új szerep.

Az alábbi példa a *Virtuális gép munkatársi* szerepkörök kezdődik, és használja, hozzon létre egy egyéni szerepkört *Virtuális géphez üzemeltető*nevű. Az új szerepkör *Microsoft.Compute* *Microsoft.Storage*és *Microsoft.Network* erőforrás-szolgáltatónál az összes olvasási műveletek hozzáférést biztosít hozzáférést szeretné kezdeni, indítsa újra, és figyelheti a virtuális gépeken futó. Az egyéni szerepkör két előfizetések használható.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Get AzureRmRoleDefinition - képernyőképe](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Egy egyéni szerepkör módosítása
Ha módosítani szeretné egy egyéni szerepkört, először használja a `Get-AzureRmRoleDefinition` parancsot a szerepkör-definíció lekéréséhez. Második végezze el a kívánt módosításokat a szerepkör-definíció. Végül, használja a `Set-AzureRmRoleDefinition` menteni a módosított szerepkör-definíció parancsot.

Az alábbi példa összeadja a `Microsoft.Insights/diagnosticSettings/*` művelet a *Virtuális géphez üzemeltető* egyéni szerepkörhöz.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Set AzureRmRoleDefinition - képernyőképe](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Az alábbi példa az Azure előfizetéssel ad a *Virtuális géphez üzemeltető* egyéni szerepkör a hozzárendelhető hatókörök.

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![RBAC PowerShell-Set AzureRmRoleDefinition - képernyőképe](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Egyéni szerepkört törlése

Ha törölni szeretne egy egyéni szerepkört, használja a `Remove-AzureRmRoleDefinition` parancsot.

Az alábbi példa a *Virtuális géphez üzemeltető* egyéni szerepkör eltávolítja.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell-eltávolítása AzureRmRoleDefinition - képernyőképe](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Egyéni lista-szerepkörök
A szerepkörök elérhető hozzárendelés a hatókör listában használja a `Get-AzureRmRoleDefinition` parancsot.

A következő példa hozzárendelés kijelölt-előfizetésben elérhető összes szerepkörének felsorolása

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell-Get AzureRmRoleDefinition - képernyőképe](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

A következő példában a *Virtuális géphez üzemeltető* egyéni szerepkör nem érhető el az előfizetés *Production4* mert adott előfizetés nem szerepel a szerepkör a **AssignableScopes** .

![RBAC PowerShell-Get AzureRmRoleDefinition - képernyőképe](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Lásd még:
- [Azure PowerShell használatával a Azure-kezelő eszközzel](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
