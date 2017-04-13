<properties
    pageTitle="Szerepköralapú hozzáférés-szerepalapú az Azure CLI kezelése |} Microsoft Azure"
    description="Megtudhatja, hogy miként kezelheti a szerepköralapú hozzáférés szerepalapú Azure parancssori felülettel szerepkörök hozzárendelése az előfizetést, és az alkalmazás hatókörök és listázása szerepkörök és szerepkör műveletek által."
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

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Szerepköralapú hozzáférés-vezérlés Azure parancssori felülettel kezelése

> [AZURE.SELECTOR]
- [A PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API-VAL](role-based-access-control-manage-access-rest.md)

Szerepköralapú hozzáférés szerepalapú az Azure-portálon és Azure erőforrás-kezelő API segítségével az előfizetést, és az erőforrások külön szinten lévő való hozzáférés kezelése. Ezzel a szolgáltatással néhány szerepköröket rendel őket egy adott hatókört a access, az Active Directory-felhasználók, csoportok vagy szolgáltatás rendszerbiztonsági biztosíthat.

Az Azure parancssori kezelőfelületről RBAC kezelése használata előtt az alábbiakat kell rendelkeznie:

- Azure CLI verzió 0.8.8 vagy újabb verziójában. A legújabb verzió telepítéséhez és társíthatja az Azure előfizetés, lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md).
- Azure erőforrás-kezelő Azure CLI. Lépjen [az Azure CLI az erőforrás-kezelő való használatáról](../xplat-cli-azure-resource-manager.md) további információt.

## <a name="list-roles"></a>Lista-szerepkörök

### <a name="list-all-available-roles"></a>Rendelkezésre álló összes szerepkörének felsorolása
Az összes rendelkezésre álló szerepkörök listában használja:

        azure role list

A következő példa bemutatja az *összes rendelkezésre álló szerepkörének*felsorolása.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure - lista azure szerepkör - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Listaműveletek: a szerepkör
A szerepkör által végzett műveletek listában használja:

    azure role show "<role name>"

A következő példa bemutatja a *közös munka* és a *Virtuális gép munkatársi* szerepkörök által végzett műveletek.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure - azure szerepkör megjelenítése - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Az access lista
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Hatékony egy erőforrás csoportot a listában szerepkör-hozzárendelés
Erőforráscsoport megtalálható szerepkör-hozzárendelés listában használja:

    azure role assignment list --resource-group <resource group name>

A következő példa bemutatja a szerepkör-hozzárendelés *pharma-értékesítés-projecforcast* csoportjában található.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure parancssori -, csoport-képernyőképe azure szerepkör-hozzárendelés listában](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Szerepkör-hozzárendelés lista egy felhasználóhoz
Az adott felhasználó szerepkör-hozzárendelések és a hozzárendelések egy felhasználói csoportok rendelt listában használja:

    azure role assignment list --signInName <user email>

A parancs módosításával csoportok öröklődnek szerepkör-hozzárendelés is megjelenik:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

A következő példa bemutatja a kapnak szerepkör-hozzárendelések az *sameert@aaddemo.com* felhasználói. Ide tartoznak, amelyek közvetlenül a felhasználóhoz rendelt és csoportokból örökölt szerepkörhöz.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure - hozzárendelés lista azure szerepkör felhasználó által - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Engedélyek biztosítása
Ha szeretne engedélyt, Miután azonosította, hogy a hozzárendelni kívánt szerepkört, használja:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Rendeljen egy szerepkört a előfizetés hatókör csoport
Rendeljen egy szerepkört, az előfizetés hatókör csoport, használhatja:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Az alábbi példa az *olvasó* szerepkör *Harmath Koch csoport* , az *előfizetés* hatókör rendeli hozzá.


![RBAC Azure parancssori - csoport-képernyőképe létrehozhat azure szerepkör-hozzárendelés](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Rendeljen egy szerepkört, az előfizetés hatókör az alkalmazás
Rendeljen egy szerepkört a előfizetés hatókör az alkalmazást, használhatja:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

A következő példa vonatkozó *munkatársi* szerepkörök *Azure AD* az alkalmazás a kijelölt előfizetéshez tartozó biztosít.

 ![RBAC Azure parancssori - alkalmazás létrehozása azure szerepkör-hozzárendelés](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>A szerepkör hozzárendelése egy felhasználóhoz, az erőforrás csoport hatóköre a
A szerepkör hozzárendelése egy felhasználóhoz, az erőforrás hatókörét, használhatja:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Az alábbi példa a *Virtuális gép munkatársi* szerepkörök, hogy hozzárendeli *samert@aaddemo.com* a *Pharma-értékesítés-ProjectForcast* erőforrás csoport hatóköre a felhasználó.

![RBAC Azure parancssori - felhasználó-képernyőképe létrehozhat azure szerepkör-hozzárendelés](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Rendeljen egy szerepkört, az erőforrás hatókör csoport
Rendeljen egy szerepkört, az erőforrás hatókör csoport, használhatja:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Az alábbi példa a *Virtuális gép munkatársi* szerepkörök az *Azure Active Directory* csoport *alhálózat*biztosít.

![RBAC Azure parancssori - csoport-képernyőképe létrehozhat azure szerepkör-hozzárendelés](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>A hozzáférés-eltávolítása
Szerepkör-hozzárendelés eltávolításához használja:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Az alábbi példa a *Virtuális gép közreműködői* szerepkör-hozzárendelés eltávolítja a *sammert@aaddemo.com* a *Pharma-értékesítés-ProjectForcast* erőforráscsoport felhasználóhoz.
A példa szerepkör-hozzárendelés majd eltávolítása egy csoportból az előfizetéshez tartozó.

![RBAC Azure - azure szerepkör-hozzárendelés törlés - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Hozzon létre egy egyéni szerepkört
Hozzon létre egy egyéni szerepkört, használhatja:

    azure role create --inputfile <file path>

Az alábbi példa létrehoz egy egyéni szerepkör *Virtuális géphez üzemeltető*neve. Az egyéni szerepkör *Microsoft.Compute* *Microsoft.Storage*és *Microsoft.Network* erőforrás-szolgáltatónál az összes olvasási műveletek hozzáférést biztosít hozzáférést szeretné kezdeni, indítsa újra, és figyelheti a virtuális gépeken futó. Az egyéni szerepkör két előfizetések használható. Ebben a példában a JSON-fájlt egy bemeneteként.

![A JSON - egyéni szerepkör-definíció - képernyőképe](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure parancssori - azure szerepkör létrehozása – kép](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Egy egyéni szerepkör módosítása

Ha módosítani szeretné egy egyéni szerepkört, először használja a `azure role show` szerepkör-definíció beolvasásához parancsot. Második végezze el a kívánt módosításokat a szerepkör-definíciós fájl. Végezetül használata `azure role set` menteni a módosított szerepkör-definíció.

    azure role set --inputfile <file path>

Az alábbi példa a *Microsoft.Insights/diagnosticSettings/* művelet hozzáadása a **tevékenységek**és az Azure előfizetéssel a **AssignableScopes** a virtuális géphez üzemeltető egyéni szerepkör.

![A JSON - egyéni szerepkör-definíció - kép módosítása](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![RBAC Azure - azure szerepkör set - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Egyéni szerepkört törlése

Ha törölni szeretne egy egyéni szerepkört, először használja a `azure role show` parancs a szerepkör **Azonosítóját** határozza meg. Ezután használja a `azure role delete` törlése a szerepkör a **azonosító**megadásával parancsot.

Az alábbi példa a *Virtuális géphez üzemeltető* egyéni szerepkör eltávolítja.

![RBAC Azure - azure szerepkör törlés - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Egyéni lista-szerepkörök

A szerepkörök elérhető hozzárendelés a hatókör listában használja a `azure role list` parancsot.

Az alábbi példa a hozzárendelés kijelölt-előfizetésben elérhető összes szerepkör sorolja fel.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure - lista azure szerepkör - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

A következő példában a *Virtuális géphez üzemeltető* egyéni szerepkör nem érhető el az előfizetés *Production4* mert adott előfizetés nem szerepel a szerepkör a **AssignableScopes** .

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure - azure szerepkör lista egyéni szerepkörök - parancssori képernyőképe](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>RBAC témakörök
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
