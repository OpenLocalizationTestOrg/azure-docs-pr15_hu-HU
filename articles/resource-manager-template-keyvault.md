<properties
   pageTitle="Fő tárolóból elemre az erőforrás-kezelő sablon |} Microsoft Azure"
   description="Az erőforrás-kezelő séma üzembe helyezése a fő tárolókban keresztül sablon látható."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Fő tárolóra sablon séma

Létrehoz egy fő tárolóból elemre.

## <a name="schema-format"></a>Séma formázása

Hozzon létre egy fő tárolóból elemre, adja hozzá a következő sémát a források szakaszában a sablon.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Értékek

Az alábbi táblázat ismerteti a értékeket kell beállítania a sémában.

| név | Érték |
| ---- | ---- | 
| típus | Felsorolás<br />Szükséges<br />**Microsoft.KeyVault/vaults**<br /><br />Az erőforrás típusa hozhat létre. |
| apiVersion | Felsorolás<br />Szükséges<br />**2015-06-01** vagy **2014-es – 12-19 – előzetes verzió**<br /><br />Az erőforrás létrehozásához használandó API verziószáma. | 
| név | Karakterlánc<br />Szükséges<br />Egy keresztül Azure egyedi leíró nevet.<br /><br />A fő tárolóra létrehozása neve. Célszerű használni a [uniqueString](resource-group-template-functions.md#uniquestring) függvény és az elnevezésük is egységes hozzon létre egy egyedi nevet az alábbi példában látható módon. |
| hely | Karakterlánc<br />Szükséges<br />Fő tárolókban érvényes terület. Annak megállapításához, érvényes régiók, olvassa el a [támogatott régiók](resource-manager-supported-services.md#supported-regions)című témakört.<br /><br />A terület tárolni a fő tárolóból elemre. |
| Tulajdonságok | Objektum<br />Szükséges<br />[objektum tulajdonságai](#properties)<br /><br />Objektum, adja meg a fő tárolóra hozhat létre. |
| erőforrások | Tömb<br />Nem kötelező.<br />A megengedett értékeket: [kulcs tárolóra titkos erőforrások](resource-manager-template-keyvault-secret.md)<br /><br />A fő tárolóra gyermek források. |

<a id="properties" />
### <a name="properties-object"></a>objektum tulajdonságai

| név | Érték |
| ---- | ---- | 
| enabledForDeployment | Logikai érték<br />Nem kötelező.<br />**Igaz** vagy **hamis**<br /><br />Itt adhatja meg, ha a tárolóra virtuális gép vagy szolgáltatás háló telepítéshez engedélyezve van-e. |
| enabledForTemplateDeployment | Logikai érték<br />Nem kötelező.<br />**Igaz** vagy **hamis**<br /><br />Itt adhatja meg, ha a tárolóból elemre engedélyezve van-e a erőforrás-kezelő sablon környezetekben való használatra. További tudnivalókért lásd: [a telepítés során biztonságos értékeket adják át](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Logikai érték<br />Nem kötelező.<br />**Igaz** vagy **hamis**<br /><br />Itt adhatja meg, ha a tárolóból elemre az mennyiségi titkosítás engedélyezve van-e. |
| tenantId | Karakterlánc<br />Szükséges<br />**Globálisan egyedi azonosítója**<br /><br />Az előfizetés bérlői azonosítója. A [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) PowerShell-parancsmag vagy az **azure-fiók megjelenítése** Azure CLI parancs meghallgathatja. |
| accessPolicies | Tömb<br />Szükséges<br />[accessPolicies objektum](#accesspolicies)<br /><br />Adja meg a kívánt felhasználót vagy a szolgáltatás egyszerű engedélyeinek legfeljebb 16-objektumok tömb. |
| Raktári szám | Objektum<br />Szükséges<br />[Termékváltozat objektum](#sku)<br /><br />A fő tárolóra SKU. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>properties.accessPolicies objektum

| név | Érték |
| ---- | ---- | 
| tenantId | Karakterlánc<br />Szükséges<br />**Globálisan egyedi azonosítója**<br /><br />Az Azure Active Directory bérlői webhely, a **objektumazonosító** a hozzáférési házirend tartalmazó bérlői azonosítója |
| Objektumazonosító | Karakterlánc<br />Szükséges<br />**Globálisan egyedi azonosítója**<br /><br />Az objektum azonosító Azure Active Directory-felhasználó vagy szolgáltatás egyszerű, hogy hozzáférést kap a tárolóból elemre. Az érték beolvasni a [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) vagy a [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) PowerShell-parancsmagok, vagy az **azure Active Directory-felhasználó** vagy az **azure Active Directory-sp** Azure CLI parancsok. |
| engedélyek | Objektum<br />Szükséges<br />[engedélyek objektum](#permissions)<br /><br />A tárolóból elemre az Active Directory-objektumra vonatkozó engedélyeket. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>properties.accessPolicies.permissions objektum

| név | Érték |
| ---- | ---- | 
| billentyűk | Tömb<br />Szükséges<br />**az összes**, **biztonsági**, **létrehozása**, **visszafejtése**, **törlése**, **titkosítása**, **első**, **importálása**, **lista**, **visszaállítása**, **bejelentkezési**, **unwrapkey**, **frissítés**, **Ellenőrizze**, **wrapkey**<br /><br />A tárolóból elemre kattintva az Active Directory-objektum hívóbetűi az engedélyeket. Egy vagy több engedélyezett értékek tömbjét ezt az értéket kell megadni. |
| titkos kulcsok | Tömb<br />Szükséges<br />**az összes**, **törlése**, **juthat**, **lista**, **megadása**<br /><br />A titkos kulcsok a tárolóból elemre kattintva az Active Directory-objektum az engedélyeket. Egy vagy több engedélyezett értékek tömbjét ezt az értéket kell megadni. |

<a id="sku" />
### <a name="propertiessku-object"></a>Properties.SKU objektum

| név | Érték |
| ---- | ---- | 
| név | Felsorolás<br />Szükséges<br />**normál**vagy **prémium verzió** <br /><br />A használandó KeyVault szolgáltatás réteg.  Normál titkos kulcsok és a billentyűk szoftverrel védett támogatja.  Prémium támogatja a billentyűparancsok HSM védett felveszi. |
| családi | Felsorolás<br />Szükséges<br />**A** <br /><br />A használandó termékváltozat család. |
 
    
## <a name="examples"></a>Példák

Az alábbi példa a fő tárolóból elemre, és a titkos üzembe helyezése.

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

## <a name="quickstart-templates"></a>Quickstart útmutató sablonok

A következő quickstart útmutató sablon üzembe helyezése a fő tárolóból elemre.

- [Hozzon létre fő tárolóból elemre](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Következő lépések

- Fő tárolókban, általános információt lásd: [első lépések az Azure kulcs tárolóból elemre](./key-vault/key-vault-get-started.md).
- Példa egy fő tárolóra titkos arra hivatkozó sablonok telepítésekor olvassa el a [a telepítés során biztonságos értékeket adják át](resource-manager-keyvault-parameter.md)című témakört.

