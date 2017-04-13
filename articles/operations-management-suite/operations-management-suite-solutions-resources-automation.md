<properties
   pageTitle="Automatizálási erőforrásainak MOBILE megoldások |} Microsoft Azure"
   description="A MOBILE megoldások általában fog bele runbooks Azure automatizálási automatizálhatja a folyamatokhoz, mint a összegyűjtése és felügyeleti adatfeldolgozás.  Ez a cikk ismerteti, hogyan szeretné hozzáadni runbooks és a kapcsolódó források megoldást."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Automatizálási erőforrásainak MOBILE megoldások (előzetes verzió)

>[AZURE.NOTE]Az előzetes dokumentáció management megoldások létrehozásához a MOBILE, amely jelenleg előzetes verzióban. Bármely leírása alább séma van változhatnak.   

[Megoldások kezelése MOBILE](operations-management-suite-solutions.md) általában fog bele runbooks Azure automatizálási például összegyűjtése és felügyeleti adatfeldolgozás a folyamatok automatizálása.  Kívül runbooks automatizálási fiókok eszközök, például a változók és az ütemterveket, amely támogatja a használt runbooks a megoldást is tartalmaz.  Ez a cikk ismerteti, hogyan szeretné hozzáadni runbooks és a kapcsolódó források megoldást.

>[AZURE.NOTE]Ez a cikk a minták paramétereket és nem szükséges, vagy közös management megoldások és [létrehozása management megoldások műveletek Management csomagja (MOBILE)](operations-management-suite-solutions-creating.md) ismertetett változók használata 


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy Ön már ismeri [kezelési megoldás létrehozása](operations-management-suite-solutions-creating.md) és a megoldás fájl felépítésének módját.

## <a name="samples"></a>A minták
Erőforrás-kezelő mintasablonok automatizálási erőforrások érheti a [quickstart útmutató sablonok GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Automatizálási fiók
Összes erőforrásában található automatizálás Azure található [automatizálás fiók](../automation/automation-security-overview.md#automation-account-overview).  [MOBILE munkaterület és automatizálási fiók](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) ismertetett módon az automatizálási fiók nem tartalmazza a projektmenedzsment megoldás, de léteznie kell, mielőtt a megoldás telepítve van.  Ha nem érhető el, akkor a megoldás telepítése sikertelen lesz.

Automatizálási fiókjukat neve minden automatizálási erőforrás neve.  Ez a megoldást, ahogy az alábbi példa az erőforrás-runbook **fióknév** paraméterrel végezhető el.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
[Azure automatizálási runbook](../automation/automation-runbook-types.md) erőforrások **Microsoft.Automation/automationAccounts/runbooks** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| runbookType | Adja meg a runbook típusú. <br><br> Parancsfájl - PowerShell-parancsprogramot <br>A PowerShell - PowerShell-munkafolyamat <br> GraphPowerShell - grafikus PowerShell parancsprogramot runbook <br> GraphPowerShellWorkflow - grafikus PowerShell munkafolyamat runbook   |
| logProgress | Itt adhatja meg, hogy [előrehaladását rekordok](../automation/automation-runbook-output-and-messages.md) esetében a runbook hozható létre. |
| logVerbose  | Itt adhatja meg, hogy a [részletes rekordok](../automation/automation-runbook-output-and-messages.md) esetében a runbook hozható létre. |
| Leírás | A runbook leírását. |
| publishContentLink | Adja meg a runbook tartalmát. <br><br>URI - Uri a runbook tartalmát.  Ez lesz egy PowerShell és parancsfájl runbooks .ps1 fájlt, és egy Graph runbook exportált grafikus runbook fájlként.  <br> verzió – a saját nyomon követés céljából runbook verzióját. |

Példa egy runbook erőforrás nem éri el.  Ebben az esetben azt veszi a runbook a [PowerShell gyűjtemény](https://www.powershellgallery.com).

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Egy runbook indítása
Kétféleképp szeretne indítani egy runbook kezelési megoldás.

- Szeretné kezdeni a runbook, ha a megoldás telepítve van, hozzon létre egy [feladat erőforrás](#automation-jobs) , az alábbiaknak.
- Szeretné kezdeni a runbook egy ütemtervet, hozzon létre egy [Erőforrás ütemezése](#schedules) , az alábbiaknak. 


## <a name="automation-jobs"></a>Feladatok automatizálása
Annak érdekében, hogy egy runbook indítása a kezelési megoldás telepítve van, hozzon létre egy **feladat** erőforrást.  Feladat erőforrások **Microsoft.Automation/automationAccounts/jobs** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| runbook    | Egy **nevet** a szervezet a runbook a nevet.  |
| Paraméterek | Az egyes paraméterek követel meg az runbook entitás. |


A feladat runbook nevét és a runbook küldendő paraméterértékeket tartalmazza.  A feladat kell, [attól függenek,](operations-management-suite-solutions-creating.md#resources) a runbook, amely azt az runbook óta indítása előtt a feladatot kell létrehozni.  Függőségek más feladatok runbooks, amely az aktuális importálás előtt meg kell adni a is létrehozhat.

Egy feladat erőforrás nevét kell tartalmaznia egy globálisan egyedi azonosítója, amely általában hozzárendelték paraméter.  Erről további globálisan egyedi azonosítója paraméterek [létrehozása](operations-management-suite-solutions-creating.md#parameters)megoldásokban műveletek Management csomagja (MOBILE).  

Következő képen egy feladat erőforrás, amelyek a kezelési megoldás telepítve van egy runbook indításakor.  Más runbooks előtt meg kell befejezni egy induljon el, hogy az függőségek tartalmazza az adott runbook feladatok.  A részletek, a feladat paramétereket és a változók, hanem közvetlenül megadott között találhatók.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Tanúsítványok
[Azure automatizálási tanúsítványok](../automation/automation-certificates.md) **Microsoft.Automation/automationAccounts/certificates** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| base64Value   | A tanúsítvány 64 számrendszerben megadott értéket. |
| ujjlenyomat    | A tanúsítvány ujjlenyomat. |

Példa egy tanúsítványt erőforrás nem éri el.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Hitelesítő adatok
[Azure automatizálási hitelesítő adatok](../automation/automation-credentials.md) **Microsoft.Automation/automationAccounts/credentials** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| Felhasználónév   | A felhasználónév, a hitelesítő adatokhoz. |
| jelszó   | A hitelesítő adatok jelszavát. |

Példa egy hitelesítőadat-erőforrás nem éri el.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Kimutatások
[Azure automatizálási ütemezések](../automation/automation-schedules.md) **Microsoft.Automation/automationAccounts/schedules** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| Leírás | Az ütemezés leírását. |
| Kezdés időpontja   | Itt adhatja meg a kezdési időpontot, az ütemezést DateTime objektumként. Egy karakterlánc is biztosítható, ha egy érvényes DateTime konvertálható. |
| isEnabled   | Itt adhatja meg, hogy engedélyezve van-e az ütemezésben. |
| intervallum    | Az ütemezés intervallum típusa.<br><br>nap<br>óra |
| gyakoriság   | Az gyakoriság, amely az ütemtervet kell fire óraszám vagy a napok száma. |

Példa az erőforrás-ütemezés nem éri el.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Változók
[Azure automatizálási változók](../automation/automation-variables.md) **Microsoft.Automation/automationAccounts/variables** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| Leírás | A változó leírását. |
| isEncrypted | Itt adhatja meg, hogy van-e a változó titkosítva. |
| típus        | Írja be a változó adatok. |
| érték       | Változó értékét. |

Példa egy változó erőforrás nem éri el.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Modulok
A kezelési megoldás nem szükséges definiálása [globális modulok](../automation/automation-integration-modules.md) használni a runbooks, mert azok mindig elérhetők lesznek.  Is használni kell a runbooks által használt bármely más modulhoz erőforrás, és a runbook annak érdekében, hogy van-e létre a runbook előtt a modul erőforrás függ.

[Integrációs modulok](../automation/automation-integration-modules.md) **Microsoft.Automation/automationAccounts/modules** típusú, és tárolja az a tulajdonságokat az alábbi táblázatban.

| A tulajdonság | Leírás |
|:--|:--|
| contentLink | Adja meg a modul tartalmát. <br><br>URI - Uri a runbook tartalmát.  Ez lesz egy PowerShell és parancsfájl runbooks .ps1 fájlt, és egy Graph runbook exportált grafikus runbook fájlként.  <br> verzió – a saját nyomon követés céljából runbook verzióját. |

Példa egy modul erőforrás nem éri el.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Modulokat frissítése
Frissíti egy ütemezett használó runbook tartalmazó kezelési megoldás, és a megoldás az új verzió új modul, hogy runbook által használt, a runbook a modul régi verzióját is használja.  A következő runbooks szerepeltetni a megoldás kell, és hozzon létre egy projektet, mielőtt bármilyen egyéb runbooks futtatásához.  Ezzel biztosíthatja, hogy modulokat frissülnek, mint előtt töltődnek be a runbooks szükséges.

- [Frissítés-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) biztosítja, hogy a megoldás a runbooks által használt modulokat összes a legújabb verzióra.  
- [ReRegisterAutomationSchedule-MS-kezelése](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) annak érdekében, hogy a runbooks hozzájuk kapcsolódó használatával a legújabb modulok ütemezés erőforrásokat fog újraregisztrálása.


Az alábbiakban a modul frissítése támogatásához megoldást szükséges elemeinek minta.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Következő lépések

- [Hozzáadás a megoldás nézet](operations-management-suite-solutions-resources-views.md) gyűjtött adatok ábrázolásához.