<properties
    pageTitle="A REST API-val irányító szerepköralapú hozzáférés-vezérlés"
    description="A REST API-val irányító szerepköralapú hozzáférés-vezérlés"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>A REST API-val irányító szerepköralapú hozzáférés-vezérlés

> [AZURE.SELECTOR]
- [A PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API-VAL](role-based-access-control-manage-access-rest.md)

Szerepköralapú hozzáférés vezérlő (RBAC) az Azure-portál és Azure erőforrás-kezelő API segítségével az előfizetést, és az erőforrások külön szinten lévő való hozzáférés kezelése. Ezzel a szolgáltatással néhány szerepköröket rendel őket egy adott hatókört a access, az Active Directory-felhasználók, csoportok vagy szolgáltatás rendszerbiztonsági biztosíthat.

## <a name="list-all-role-assignments"></a>A lista összes szerepkör-hozzárendelés

A megadott hatókör és subscopes szerepkör-hozzárendelés listája.

Lista szerepkör-hozzárendelések kell van hozzáférése `Microsoft.Authorization/roleAssignments/read` művelet egy keresési tartományt. A beépített szerepkörök ezt a műveletet hozzáférést kapnak. Szerepkör-hozzárendelések és Azure erőforrások kezelése access kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md)című témakört.

### <a name="request"></a>Kérés

Az **első** mód használata az alábbi URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Belül a URI végezze el a következő helyettesítő kérését testre:

1. *{Hatókör}* cserélje, amelynek a szerepkör-hozzárendelés lista kívánt hatókör. Az alábbi példák bemutatják, hogyan különböző szintek megadásához:

  - Előfizetés: /subscriptions/ {előfizetés azonosítója}  
  - Erőforráscsoport: {Előfizetés azonosítója} /subscriptions//resourceGroups/myresourcegroup1  
  - Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Api-verzió}* cserélje 2015-07-01.

3. *{Szűrő}* cserélje ki a feltételt, amelyet szeretne alkalmazni a szerepkör-hozzárendelés szűrésre:

  - Szerepkör-hozzárendelések lista csak a megadott keresési tartomány, nem beleértve a subscopes szerepkör-hozzárendelés:`atScope()`    
  - Szerepkör-hozzárendelések egy adott felhasználói, csoport vagy alkalmazás lista:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Szerepkör-hozzárendelések az adott felhasználó, köztük öröklődnek a csoportok listában |}`assignedTo('{objectId of user}')`

### <a name="response"></a>Válasz

Állapotkód: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Szerepkör-hozzárendelés adatainak megtekintése

Megkapja a egyetlen szerepkör-hozzárendelés szerepkör-hozzárendelés azonosító által megadott információkat.

Tudnivalók a szerepkör-hozzárendelés van hozzáférése `Microsoft.Authorization/roleAssignments/read` művelet. A beépített szerepkörök ezt a műveletet hozzáférést kapnak. Szerepkör-hozzárendelések és Azure erőforrások kezelése access kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md)című témakört.

### <a name="request"></a>Kérés

Az **első** mód használata az alábbi URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Belül a URI végezze el a következő helyettesítő kérését testre:

1. *{Hatókör}* cserélje, amelynek a szerepkör-hozzárendelés lista kívánt hatókör. Az alábbi példák bemutatják, hogyan különböző szintek megadásához:

  - Előfizetés: /subscriptions/ {előfizetés azonosítója}  
  - Erőforráscsoport: {Előfizetés azonosítója} /subscriptions//resourceGroups/myresourcegroup1  
  - Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Szerepkör-hozzárendelés-id}* cserélje le a szerepkör-hozzárendelés GUID azonosítója.

3. *{Api-verzió}* cserélje 2015-07-01.

### <a name="response"></a>Válasz

Állapotkód: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Szerepkör-hozzárendelés létrehozása

Hozzon létre egy szerepkör-hozzárendelés a megadott résztvevő megadása a megadott szerepkör megadott hatókörét.

Szerepkör-hozzárendelés létrehozásához rendelkeznie kell a hozzáférést `Microsoft.Authorization/roleAssignments/write` művelet. A beépített szerepkörök csak a *tulajdonos* és a *Felhasználói hozzáférés rendszergazda* hozzáférést kapnak ezt a műveletet. Szerepkör-hozzárendelések és Azure erőforrások kezelése access kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md)című témakört.

### <a name="request"></a>Kérés

Az alábbi URI **ELHELYEZNI** módszer használhatja:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Belül a URI végezze el az alábbi helyettesítés kérését testre:

1. *{Hatókör}* cserélje le a hatókör, amelynél le szeretne létrehozni a szerepkör-hozzárendelés. Szerepkör-hozzárendelés a szülő hatókör létrehozásakor az összes alárendelt hatókör azonos szerepkör-hozzárendelés örökli. Az alábbi példák bemutatják, hogyan különböző szintek megadásához:

  - Előfizetés: /subscriptions/ {előfizetés azonosítója}  
  - Erőforráscsoport: {Előfizetés azonosítója} /subscriptions//resourceGroups/myresourcegroup1   
  - Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Szerepkör-hozzárendelés-id}* cserélje le egy új globálisan egyedi azonosítója, amely akkor válik az új szerepkör-hozzárendelés GUID azonosítója.

3. *{Api-verzió}* cserélje 2015-07-01.

A kérés szervezet adja meg az értékeket a következő formátumban:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Elem neve     | Szükséges | Típus   | Leírás |
|------------------|----------|--------|-------------|
| roleDefinitionId | igen      | Karakterlánc | A szerepkör azonosítója. Az azonosító formátuma:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | igen      | Karakterlánc | Objektumazonosító az Azure Active Directory tőke (felhasználói, csoport vagy szolgáltatás egyszerű), amely a szerepkör hozzá van rendelve. |

### <a name="response"></a>Válasz

Állapotkód: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Szerepkör-hozzárendelés törlése

A megadott keresési tartomány: a szerepkör-hozzárendelés törlése.

Szerepkör-hozzárendelés törléséhez van hozzáférése a `Microsoft.Authorization/roleAssignments/delete` művelet. A beépített szerepkörök csak a *tulajdonos* és a *Felhasználói hozzáférés rendszergazda* hozzáférést kapnak ezt a műveletet. Szerepkör-hozzárendelések és Azure erőforrások kezelése access kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md)című témakört.

### <a name="request"></a>Kérés

A **Törlés** mód használata az alábbi URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Belül a URI végezze el a következő helyettesítő kérését testreszabása:

1. *{Hatókör}* cserélje le a hatókör, amelynél le szeretne létrehozni a szerepkör-hozzárendelés. Az alábbi példák bemutatják, hogyan különböző szintek megadásához:

  - Előfizetés: /subscriptions/ {előfizetés azonosítója}  
  - Erőforráscsoport: {Előfizetés azonosítója} /subscriptions//resourceGroups/myresourcegroup1  
  - Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Szerepkör-hozzárendelés-id}* cserélje le a szerepkör-hozzárendelés azonosító globálisan egyedi azonosítója.

3. *{Api-verzió}* cserélje 2015-07-01.

### <a name="response"></a>Válasz

Állapotkód: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Összes szerepkörének felsorolása

Elérhető a megadott keresési tartomány a hozzárendeléshez tartozó összes szerepkörének felsorolása

Lista szerepkörökhöz rendelkeznie kell a hozzáférést `Microsoft.Authorization/roleDefinitions/read` művelet egy keresési tartományt. A beépített szerepkörök ezt a műveletet hozzáférést kapnak. Szerepkör-hozzárendelések és Azure erőforrások kezelése access kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md)című témakört.

### <a name="request"></a>Kérés

Az **első** mód használata az alábbi URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Belül a URI végezze el a következő helyettesítő kérését testreszabása:

1. *{Hatókör}* cserélje, amelynek a szerepkörök lista kívánt hatókör. Az alábbi példák bemutatják, hogyan különböző szintek megadásához:

  - Előfizetés: /subscriptions/ {előfizetés azonosítója}  
  - Erőforráscsoport: {Előfizetés azonosítója} /subscriptions//resourceGroups/myresourcegroup1  
  - Erőforrás /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Api-verzió}* cserélje 2015-07-01.

3. *{Szűrő}* cserélje ki a feltétel alkalmazása a szűrésre szerepkörök használni kívánt:

  - A lista elérhető, a megadott keresési tartomány, és minden hozzárendelés gyermek hatókörén szerepkörök:`atScopeAndBelow()`
  - Keressen a pontos megjelenítendő nevet szerepkörbe: `roleName%20eq%20'{role-display-name}'`. Az URL-címként kódolandó képernyőn: a szerepkör a pontos megjelenítendő nevét. Ha például`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Válasz

Állapotkód: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Szerepkör adatainak megtekintése

Szerepkör-definíciós azonosító által megadott egyetlen szerepkör tájékoztatást kap. A megjelenítendő név segítségével egyetlen szerepkör kapcsolatos tudnivalók című témakörben kaphat [összes szerepkörének felsorolása](role-based-access-control-manage-access-rest.md#list-all-roles).

Kapcsolatos szerepkörbe tájékozódáshoz, van hozzáférése `Microsoft.Authorization/roleDefinitions/read` művelet. A beépített szerepkörök ezt a műveletet hozzáférést kapnak. Szerepkör-hozzárendelések és Azure erőforrások kezelése access kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md)című témakört.

### <a name="request"></a>Kérés

Az **első** mód használata az alábbi URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Belül a URI végezze el a következő helyettesítő kérését testre:

1. *{Hatókör}* cserélje, amelynek a szerepkör-hozzárendelés lista kívánt hatókör. Az alábbi példák bemutatják, hogyan különböző szintek megadásához:

  - Előfizetés: /subscriptions/ {előfizetés azonosítója}  
  - Erőforráscsoport: {Előfizetés azonosítója} /subscriptions//resourceGroups/myresourcegroup1  
  - Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Szerepkör-definíciós-azonosító}* cserélje le a szerepkör-definíció GUID azonosítója.

3. *{Api-verzió}* cserélje 2015-07-01.

### <a name="response"></a>Válasz

Állapotkód: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Hozzon létre egy egyéni szerepkört
Hozzon létre egy egyéni szerepkört.

Hozzon létre egy egyéni szerepkört, van hozzáférése `Microsoft.Authorization/roleDefinitions/write` összes művelet a `AssignableScopes`. A beépített szerepkörök csak a *tulajdonos* és a *Felhasználói hozzáférés rendszergazda* hozzáférést kapnak ezt a műveletet. Szerepkör-hozzárendelések és Azure erőforrások kezelése access kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md)című témakört.

### <a name="request"></a>Kérés

Az alábbi URI **ELHELYEZNI** módszer használhatja:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Belül a URI végezze el a következő helyettesítő kérését testre:

1. Cserélje ki az első *AssignableScope* az egyéni szerepkör *{hatókör}* . Az alábbi példák bemutatják, hogyan különböző szintek megadásához.

  - Előfizetés: /subscriptions/ {előfizetés azonosítója}  
  - Erőforráscsoport: {Előfizetés azonosítója} /subscriptions//resourceGroups/myresourcegroup1  
  - Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Szerepkör-definíciós-azonosító}* cserélje le egy új globálisan egyedi azonosítója, amely akkor válik az új egyéni szerepkör GUID azonosítója.

3. *{Api-verzió}* cserélje 2015-07-01.

A kérés szervezet adja meg az értékeket a következő formátumban:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elem neve | Szükséges | Típus | Leírás |
|--------------|----------|------|-------------|
| név         | igen | Karakterlánc   | Az egyéni szerepkör GUID azonosítója    |
| properties.roleName               | igen | Karakterlánc   | Megjelenítendő név az egyéni szerepkör. Maximális méret 128 karaktert.                        |
| Properties.Description            | nem  | Karakterlánc   | Az egyéni szerepkör leírását. Maximális méret 1024 karaktereket.                                               |
| Properties.Type                   | igen | Karakterlánc   | "CustomRole." beállítása                                         |
| Properties.permissions.Actions    | igen | Karakterlánc] | A műveletek, az egyéni szerepkör által biztosított művelet karakterláncokkal tömbje.             |
| properties.permissions.notActions | nem  | Karakterlánc] | A műveletek, ha nem szeretné, a műveletek, az egyéni szerepkör által biztosított művelet karakterláncokkal tömbje. |
| properties.assignableScopes       | igen | Karakterlánc] | Egy tömb hatókörök, ahol az egyéni szerepkört is használható.   |

### <a name="response"></a>Válasz

Állapotkód: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Egyéni szerepkört frissítése

Módosítsa az egyéni szerepkört.

Ha módosítani szeretné egy egyéni szerepkört, rendelkeznie kell a hozzáférést `Microsoft.Authorization/roleDefinitions/write` összes művelet a `AssignableScopes`. A beépített szerepkörök csak a *tulajdonos* és a *Felhasználói hozzáférés rendszergazda* hozzáférést kapnak ezt a műveletet. Szerepkör-hozzárendelések és Azure erőforrások kezelése access kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md)című témakört.

### <a name="request"></a>Kérés

Az alábbi URI **ELHELYEZNI** módszer használhatja:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Belül a URI végezze el a következő helyettesítő kérését testre:

1. Cserélje ki az első *AssignableScope* az egyéni szerepkör *{hatókör}* . Az alábbi példák bemutatják, hogyan különböző szintek megadásához:

  - Előfizetés: /subscriptions/ {előfizetés azonosítója}  
  - Erőforráscsoport: {Előfizetés azonosítója} /subscriptions//resourceGroups/myresourcegroup1  
  - Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Szerepkör-definíciós-azonosító}* cserélje ki az egyéni szerepkör GUID azonosítója.

3. *{Api-verzió}* cserélje 2015-07-01.

A kérés szervezet adja meg az értékeket a következő formátumban:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elem neve | Szükséges | Típus | Leírás |
|--------------|----------|------|-------------|
| név         | igen      | Karakterlánc | Az egyéni szerepkör GUID azonosítója |
| properties.roleName | igen | Karakterlánc | A frissített egyéni szerepkör megjelenítendő nevét. |
| Properties.Description | nem | Karakterlánc | A frissített egyéni szerepkör leírását. |
| Properties.Type | igen | Karakterlánc | "CustomRole." beállítása |
| Properties.permissions.Actions | igen | Karakterlánc] | A műveletek, amelyhez a frissített egyéni szerepkör hozzáférést biztosít a művelet karakterláncokkal tömbje. |
| properties.permissions.notActions | nem | Karakterlánc] | A műveletek, ha nem szeretné, a műveleteket, amelyek a frissített egyéni szerepköre művelet karakterláncokkal tömbje. |
| properties.assignableScopes | igen | Karakterlánc] | Egy tömb hatókörök, amelyben a frissített egyéni szerepkört is használható. |

### <a name="response"></a>Válasz

Állapotkód: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Egyéni szerepkört törlése

Egyéni szerepkört törlése.

Ha törölni szeretne egy egyéni szerepkört, rendelkeznie kell a hozzáférést `Microsoft.Authorization/roleDefinitions/delete` összes művelet a `AssignableScopes`. A beépített szerepkörök csak a *tulajdonos* és a *Felhasználói hozzáférés rendszergazda* hozzáférést kapnak ezt a műveletet. Szerepkör-hozzárendelések és Azure erőforrások kezelése access kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](role-based-access-control-configure.md)című témakört.

### <a name="request"></a>Kérés

A **Törlés** mód használata az alábbi URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Belül a URI végezze el a következő helyettesítő kérését testreszabása:

1. *{Hatókör}* cserélje le a hatókör, amelynél szerepkör-definíció törölni szeretne. Az alábbi példák bemutatják, hogyan különböző szintek megadásához:

  - Előfizetés: /subscriptions/ {előfizetés azonosítója}  
  - Erőforráscsoport: {Előfizetés azonosítója} /subscriptions//resourceGroups/myresourcegroup1  
  - Erőforrás: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Szerepkör-definíciós-azonosító}* cserélje ki az egyéni szerepkör globálisan egyedi azonosítója szerepkör definition azonosítója.

3. *{Api-verzió}* cserélje 2015-07-01.

### <a name="response"></a>Válasz

Állapotkód: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
