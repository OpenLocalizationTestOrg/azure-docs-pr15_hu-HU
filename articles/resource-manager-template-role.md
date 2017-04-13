<properties
   pageTitle="Szerepkör-hozzárendelések az erőforrás-kezelő sablon |} Microsoft Azure"
   description="Az erőforrás-kezelő séma üzembe helyezése a szerepkör-hozzárendelés keresztül sablon látható."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="role-assignments-template-schema"></a>Szerepkör-hozzárendelések sablon séma

Egy felhasználó, csoport vagy szolgáltatás egyszerű hozzárendeli a megadott hatókör szerepkörbe.

## <a name="resource-format"></a>Erőforrás-formátum

A sablon a források szakaszában a következő séma hozzáadása a szerepkör-hozzárendelés létrehozásához.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Értékek

Az alábbi táblázat ismerteti a értékeket kell beállítania a sémában.

| név | Szükséges | Leírás |
| ---- | -------- | ----------- |
| típus | igen    | Az erőforrás típusa hozhat létre.<br /><br /> Az erőforráscsoport:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Az erőforrás:<br />**{provider-névtér} / {erőforrástípus} / szolgáltatók/roleAssignments** |
| apiVersion |igen | Az erőforrás létrehozásához használandó API verziószáma.<br /><br /> **2015-07-01**használja. | 
| név | igen | Az új szerepkör-hozzárendelés globálisan egyedi azonosítója |
| dependsOn | nem | Egy vesszővel tagolt tömböt egy erőforrás nevét, vagy az erőforrás-egyedi azonosítókat.<br /><br />Az erőforrások függ, hogy a szerepkör-hozzárendelés gyűjteménye. Erőforrás keresési tartománya, és erőforrás telepítik ugyanazt a sablont a szerepkör hozzárendelése, terjed ki, hogy az erőforrás neve biztosítja, hogy az erőforrás először telepíti az elemben. |
| Tulajdonságok | igen | A Tulajdonságok objektum, amely azonosítja a szerepkör-definíció, az egyszerű és a hatókör. |

### <a name="properties-object"></a>objektum tulajdonságai

| név | Szükséges | Leírás |
| ---- | -------- | ----------- |
| roleDefinitionId | igen |  Egy meglévő szerepkör definícióját szerepkör-hozzárendelés használandó azonosítója.<br /><br /> Használja a következő formátumban:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | igen | Egy meglévő egyszerű globálisan egyedi azonosítója. A térképek belül a címtárban az azonosító értéke és a felhasználó, szolgáltatás egyszerű vagy biztonsági csoport mutathat. |
| hatókör | nem | A hatókör, amelynél a szerepkör-hozzárendelés vonatkozik.<br /><br />Az erőforrás-csoportok használja:<br />**{erőforrás-csoportnevet} {előfizetés azonosítója} /Subscriptions/ /resourceGroups/**  <br /><br />Erőforrások használata:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>A szerepkör-hozzárendelés erőforrás használata

Ha egy felhasználó, csoport vagy szolgáltatás fő szerepkörbe felvenni a telepítés során kell a szerepkör-hozzárendelés hozzáadása a sablonhoz. Szerepkör-hozzárendelés öröklődnek magasabb szintű hatókörét, így ha már hozzáadta a rendszerbiztonsági tag előfizetés szintre szerepkörbe, nem kell az erőforráscsoport vagy az erőforrás hozzárendelni.

Meg kell adnia a szerepkör-hozzárendelés való munkavégzés során sok azonosító értékek is. Az értékek PowerShell vagy Azure CLI keresztül meghallgathatja.

### <a name="powershell"></a>A PowerShell

Szerepkör-hozzárendelés neve szükséges globálisan egyedi azonosítója. Készíthet egy új **nevet** , és azonosító:

    $name = [System.Guid]::NewGuid().toString()

A szerepkör-definíció azonosítóját meghallgathatja:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Az azonosító számára a tőketörlesztés mértékét, az alábbi parancsok egyikére és meghallgathatja.

Csoport neve **ellenőrök**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

A felhasználó neve **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Egy szolgáltatás fő nevű **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure CLI

A szerepkör-definíció azonosítóját meghallgathatja:

    azure role show Reader --json | jq .[].Id -r

Az azonosító számára a tőketörlesztés mértékét, az alábbi parancsok egyikére és meghallgathatja.

Csoport neve **ellenőrök**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

A felhasználó neve **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Egy szolgáltatás fő nevű **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Példák

A következő sablon kap egy szerepkör az azonosító és a felhasználói, csoport vagy szolgáltatás egyszerű azonosítót. A szerepkör a erőforrás csoport szintjén oszt ki.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



A következő sablon létrehoz egy tárterület-fiókot, és az Olvasó szerepkör rendel a tárterület-fiókot. A két csoportok és a rendszer olvasó alkalmazásának szerepkör az azonosítók egyszerűsítheti sablon is szerepelnek. Ezeket az értékeket a esetleg beolvasott időben telepítési keresztül parancsfájl, és paraméterként a.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Quickstart útmutató sablonok

Az alábbi sablonok használatáról a szerepkör-hozzárendelés erőforrás megjelenítése:

- [Beépített szerepkör hozzárendelése erőforráscsoport](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Beépített szerepkör hozzárendelése meglévő virtuális](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Beépített szerepkör hozzárendelése több meglévő VMs](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Következő lépések

- A sablon szerkezet kapcsolatos tudnivalókért lásd: a [szerzői Azure erőforrás-kezelő sablonok](resource-group-authoring-templates.md).
- Szerepköralapú hozzáférés-vezérlés kapcsolatos további tudnivalókért olvassa el az [Azure Active Directory szerepköralapú hozzáférés-vezérlés](./active-directory/role-based-access-control-configure.md)című témakört.
