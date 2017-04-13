<properties
    pageTitle="Erőforrás-kezelő sablon használatával diagnosztikai beállítások automatikusan engedélyezése |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre, amely lehetővé teszi a diagnosztikai naplók esemény hubok adatfolyam vagy tárolja őket a tárterület-fiók diagnosztikai beállítások erőforrás-kezelő sablonnal."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Diagnosztikai beállítások automatikusan engedélyezése az erőforrás hoz létre egy erőforrás-kezelő sablon használatával
Ebben a cikkben bemutatjuk, hogyan használhatja az [erőforrás-kezelő Azure-sablon](../resource-group-authoring-templates.md) diagnosztikai beállítások konfigurálása után egy erőforrás. Lehetővé teszi, hogy az automatikus indítása: a diagnosztikai naplók és mérőszámok esemény hubok archiválja tárterület-fiókjában vagy elküldeni a napló Analytics erőforrás létrehozásakor a folyamatos átvitelű.

Erőforrás-kezelő sablon használatával a diagnosztikai naplók engedélyezése módszer a attól függ, hogy az erőforrás típusa.

- **Nem-számítási** erőforrások (például hálózati a biztonsági csoportok összefüggés-alkalmazásokat, automatizálási) használata a [jelen cikkben ismertetett diagnosztikai beállítások](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Kiszámítása** Erőforrások (ÜVEGVATTA/LAD-alapú) használata a [jelen cikkben ismertetett ÜVEGVATTA/LAD konfigurációs fájl](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

Ez a cikk azt ismertetik, hogyan lehet módszerek valamelyikével diagnosztika konfigurálása.

Az alapvető lépések a következők:

1. Hozzon létre egy sablont, amely bemutatja, hogy miként hozhat létre az erőforrás, és diagnosztika engedélyezése JSON-fájlként.
2. [A sablon telepítési módszerrel Deploy](../resource-group-template-deploy.md).

Az alábbi példa a JSON sablonfájl szeretne létrehozni, nem számítási és a számítási erőforrások ad.

## <a name="non-compute-resource-template"></a>Nem-számítási erőforrás-sablon
Nem számítási erőforrások meg kell két dolgot kell tennie:

1. Paraméterek hozzáadása a paraméterek blob a tárterület-fiók nevét, a szolgáltatás bus szabály azonosítója és a MOBILE napló Analytics munkaterület-azonosító (egy tárterület-fiókot, a folyamatos átvitelű esemény hubok naplót és/vagy napló Analytics naplók küldése a diagnosztikai naplók archiválás engedélyezése).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Az erőforrás ahhoz, hogy a diagnosztikai naplók kívánt erőforrások tömbben típusú erőforrás hozzáadása `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

A Tulajdonságok blob a diagnosztikai beállítás követi [formátuma a jelen cikkben ismertetett](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Íme egy teljes példa, hogy a hálózati biztonsági csoportokat hoz létre, és kapcsolja be a folyamatos átvitelű esemény hubok és tároló tárterület-fiókjában.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Kiszámítása az erőforrás-sablon
Ahhoz, hogy a diagnosztika számítási erőforrás, például egy virtuális gép vagy szolgáltatás háló fürt kell:

1. Az Azure diagnosztika bővítmény hozzáadása a virtuális erőforrás-definíciót.
2. Adja meg a tárhely fiók, illetve az esemény központi paraméter.
3. A WADCfg XML-fájl tartalmának hozzáadása a XMLCfg tulajdonság, az összes XML-karakter megfelelően kijutását.

> [AZURE.WARNING] Lehet, hogy ezt a lépést utolsó körlevelekben, hogy a megfelelő. [Lásd: Ez a cikk](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) példát, amely a diagnosztika konfigurációs séma osztja escape és megfelelően formázott változók.

A teljes folyamat, minták, többek között leírt [a dokumentumban](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Következő lépések
- [További információ: a diagnosztikai naplók Azure](./monitoring-overview-of-diagnostic-logs.md)
- [Adatfolyam-esemény hubok Azure diagnosztikai naplók](./monitoring-stream-diagnostic-logs-to-event-hubs.md)
