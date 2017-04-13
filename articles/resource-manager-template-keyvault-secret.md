<properties
   pageTitle="A titkos egy fő tárolóból elemre az erőforrás-kezelő sablon |} Microsoft Azure"
   description="Az erőforrás-kezelő séma üzembe helyezése a fő tárolóra titkos kulcsok keresztül sablon látható."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Fő tárolóra titkos sablon séma

Létrehoz egy titkos, amely található egy fő tárolóból elemre. Ez az erőforrás típusa gyakran telepítve van, gyermek erőforrásként [kulcs tárolóból elemre](resource-manager-template-keyvault.md).

## <a name="schema-format"></a>Séma formázása

A sablon a következő séma hozzáadása a fő tárolóra titkos létrehozásához. A titkos definiálható erőforrásként vagy gyermek egy fő tárolóból elemre, vagy a legfelső szintű erőforrásként. Itt megadhatja azt gyermek erőforrásként amikor a fő tárolóra telepítve van az ugyanazt a sablont. Meg kell meghatározása a titkos legfelső szintű erőforrásként, amikor a fő tárolóra nincs telepítve az ugyanazt a sablont, vagy több titkos kulcsok létrehozásához kattintson az erőforrás típusa hurok által szükséges. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Értékek

Az alábbi táblázat ismerteti a értékeket kell beállítania a sémában.

| név | Érték |
| ---- | ---- | 
| típus | Felsorolás<br />Szükséges<br />**titkos kulcsok** (amikor rendszerbe, gyermek erőforrásként kulcs tárolóból elemre) vagy<br /> **Microsoft.KeyVault/vaults/secrets** (Ha a legfelső szintű erőforrásként rendszerbe)<br /><br />Az erőforrás típusa hozhat létre. |
| apiVersion | Felsorolás<br />Szükséges<br />**2015-06-01** vagy **2014-es – 12-19 – előzetes verzió**<br /><br />Az erőforrás létrehozásához használandó API verziószáma. | 
| név | Karakterlánc<br />Szükséges<br />Egy adott szót, amikor rendszerbe gyermek erőforrásként egy fő tárolóból elemre, vagy a formátum **{billentyű-tárolóra-neve} / {titkos neve}** amikor rendszerbe erőforrásként legfelső szintű adható hozzá egy meglévő kulcs tárolóból elemre.<br /><br />A titkos létrehozása neve. |
| Tulajdonságok | Objektum<br />Szükséges<br />[objektum tulajdonságai](#properties)<br /><br />Egy objektum, amely meghatározza a titkos létrehozása értékét. |
| dependsOn | Tömb<br />Nem kötelező.<br />Erőforrás vesszővel tagolt listáját a nevét vagy az erőforrás egyedi azonosítóját.<br /><br />A webhelycsoport erőforrások függ, hogy ezt a hivatkozást. Ha a esetében a titkos kulcs tárolóra telepíti az ugyanazt a sablont, a fő tárolóból elemre nevét tartalmazza annak érdekében, hogy először telepítve van az elemben. |

<a id="properties" />
### <a name="properties-object"></a>objektum tulajdonságai

| név | Érték |
| ---- | ---- | 
| érték | Karakterlánc<br />Szükséges<br /><br />A titkos érték tárolása a a fő tárolóból elemre. A tulajdonság áthaladó értékkel, használatakor típus **securestring**paraméter.  |

    
## <a name="examples"></a>Példák

Az első példa egy fő tárolóból elemre a gyermek erőforrásként üzembe helyezése a titkos.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

A második példában a titkos, amely egy meglévő kulcs tárolóból elemre a legfelső szintű erőforrásként üzembe helyezése.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Következő lépések

- Fő tárolókban, általános információt lásd: [első lépések az Azure kulcs tárolóból elemre](./key-vault/key-vault-get-started.md).
- Példa egy fő tárolóra titkos arra hivatkozó sablonok telepítésekor olvassa el a [a telepítés során biztonságos értékeket adják át](resource-manager-keyvault-parameter.md)című témakört.


