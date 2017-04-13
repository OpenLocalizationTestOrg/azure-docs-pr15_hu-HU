<properties
   pageTitle="Titkos kulcs tárolóból elemre a erőforrás-kezelő sablonnal |} Microsoft Azure"
   description="Megtudhatja, hogy miként át egy titkos egy fő tárolóra paraméterként a telepítés során."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Biztonságos értékeket adják át a telepítés közben

Kell paraméterként biztonságos (például a jelszó) értéket a telepítés során adják át, amikor egy titkos az [Azure kulcs tárolóból elemre](./key-vault/key-vault-whatis.md) az adott értéket tárolni, és más erőforrás-kezelő sablonok az az érték hivatkozás. Csak a titkos hivatkozást tartalmaz a sablon, a titkos soha nem van téve, és nem kell manuálisan adja meg az érték a titkos minden alkalommal, amikor az erőforrások rendszerbe. Megadhatja, hogy mely felhasználók vagy a szolgáltatás rendszerbiztonsági érheti el a titkos.  

## <a name="deploy-a-key-vault-and-secret"></a>A fő tárolóból elemre, és a titkos terjesztése

Fő tárolóból elemre, amely hivatkozhat más erőforrás-kezelő sablonokból létrehozásához be kell a **enabledForTemplateDeployment** tulajdonság **Igaz**, és meg kell hozzáférést a felhasználónak vagy a szolgáltatás egyszerű, amely hajtja végre a telepítést, amely hivatkozik, a titkos.

Üzembe helyezése a fő tárolóból elemre, és a titkos kapcsolatos további tudnivalókért lásd: [kulcs tárolóra sémával](resource-manager-template-keyvault.md) és egy [kulcsot tárolóra titkos séma](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>A titkos statikus azonosítójú hivatkozás

A paraméterek fájlban átadja értékeket a sablont, amely a titkos kulcs hivatkozik. A titkos referencia a megkerülhetők, a fő tárolóból elemre az erőforrás-azonosító és a titkos nevét. Ebben a példában a fő tárolóra titkos már léteznie kell, és használja az statikus értéknek-, erőforrás-azonosító.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Egy teljes paraméter fájlt a következőhöz hasonló lehet:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Fogadja el a titkos paraméter egy **securestring**kell lennie. A következő példa bemutatja egy sablon, amely a rendszergazdai jelszó szükséges SQL server üzembe helyezése a vonatkozó szakaszokat.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>A titkos dinamikus azonosítójú hivatkozás

Az előző szakaszban továbbítása egy statikus erőforrás-azonosító számára a fő tárolóra titkos mutatott. Azonban bizonyos esetekben szüksége egy fő tárolóra titkos változó alapján az aktuális környezetben hivatkozni szeretne. Ebben az esetben nem tud merevlemez-kódot az erőforrás-azonosító a paraméterek fájlban. Sajnos nem dinamikusan hozható létre az erőforrás-azonosító a paraméterek fájl, mert a sablon kifejezések nem használhatók a paraméterek fájlt.

Dinamikusan készítése a fő tárolóra titkos az erőforrás-azonosító, helyezze a az erőforrás a titkos elvégzendő beágyazott sablonba. A fő sablon, a beágyazott sablon hozzáadása fázisban és a dinamikusan létrehozott erőforrás-azonosító tartalmazó paraméter.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Következő lépések

- Fő tárolókban, általános információt lásd: [első lépések az Azure kulcs tárolóból elemre](./key-vault/key-vault-get-started.md).
- A fő tárolóból elemre használatáról virtuális gép további tudnivalókért lásd [biztonsági megfontolások az Azure erőforrás parancsra](best-practices-resource-manager-security.md).
- Teljes példák a fő titkos kulcsok hivatkozó példák azt [Kulcs tárolóból elemre](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

