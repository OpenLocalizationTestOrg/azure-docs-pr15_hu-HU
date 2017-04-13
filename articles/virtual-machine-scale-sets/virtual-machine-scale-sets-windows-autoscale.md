<properties
    pageTitle="Automatikus méretezéssel Windows virtuális gép skála beállítása |} Microsoft Azure"
    description="Autoscaling beállítása a Windows virtuális gép skála megadása Azure PowerShell használatával"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>Automatikus méretezése gépek egy virtuális gép skála megadása

Virtuális gép skála készletek megkönnyítik az üzembe helyezéséhez és kezeléséhez azonos virtuális gépeken futó tárolt. A szükséges jól méretezhető és testre szabható számítási réteg skála készletek hyperscale alkalmazások, és Windows platformon képek, Linux platform képek, egyéni képeket és bővítmények támogatják. Méretarány készletek kapcsolatos további tudnivalókért olvassa el a [Virtuális gép skála állítja be](virtual-machine-scale-sets-overview.md)című témakört.

Ebből az oktatóanyagból megtudhatja, hogy miként, hozza létre a Windows virtuális gépeken futó skála csoportját, és automatikusan átméretezni a gép megadása. A méretezés és beállítania egy erőforrás-kezelő Azure-sablon létrehozása és üzembe helyezés Azure PowerShell használatával méretezés hoz létre. Sablonok kapcsolatos további tudnivalókért olvassa el a [szerzői Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)című témakört. Skála beállítja az automatikus méretezés kapcsolatos további információért lásd: [automatikus méretezése és virtuális gép skála állítja be](virtual-machine-scale-sets-autoscale-overview.md).

Ez a cikk telepíti, az alábbi források és bővítmények:

- Microsoft.Storage/storageAccounts
- Microsoft.Network/virtualNetworks
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/loadBalancers
- Microsoft.Network/networkInterfaces
- Microsoft.Compute/virtualMachines
- Microsoft.Compute/virtualMachineScaleSets
- Microsoft.Insights.VMDiagnosticsSettings
- Microsoft.Insights/autoscaleSettings

Erőforrás-kezelő erőforrások kapcsolatos további tudnivalókért olvassa el az [Azure számítja ki, a hálózatba, és a tárterület-szolgáltatók csoportban az Azure erőforrás-kezelő](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)című témakört.

## <a name="step-1-install-azure-powershell"></a>Lépés: 1: Azure PowerShell telepítése

Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) Azure PowerShell legújabb verziójának telepítése, kijelöli az előfizetést, és bejelentkezés az Azure információt.

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>Lépés: 2: Erőforráscsoport és a tárterület-fiók létrehozása

1. **Erőforrás csoport létrehozása** – összes erőforrás kell üzembe helyezni egy erőforrás csoportot. [Új-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) segítségével **vmsstestrg1**nevű erőforráscsoport.

2. **Tárterület-fiók létrehozása** – ehhez a fiókhoz tároló, a sablon tároló. [Új-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) segítségével **vmsstestsa**nevű tárterület-fiók létrehozása.

## <a name="step-3-create-the-template"></a>3 lépés: A sablon létrehozása
Az erőforrás-kezelő Azure-sablon üzembe helyezéséhez és Azure erőforrások együttes kezelése az erőforrások és a kapcsolódó telepítési paraméterek JSON leírását használatával teszi lehetővé teszi.

1. A kedvenc szerkesztőben hozza létre a C:\VMSSTemplate.json fájlt, és a kezdeti JSON rendszerezni a sablon támogatja.

        {
          "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
          ]
        }

2. Paraméterek nem mindig kötelező, de azok kínál bemeneti értékek, ha telepíti a sablont. Adja hozzá ezeket a paramétereket a paraméterek szülőelem, amelyhez hozzáadta a sablon alapján.

        "vmName": { "type": "string" },
        "vmSSName": { "type": "string" },
        "instanceCount": { "type": "string" },
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" },
        "resourcePrefix": { "type": "string" }

    - Állítsa a külön virtuális gép a méretezés gépek eléréséhez használt nevét.
    - A sablon tároló tárterület-fiók neve.
    - Az eredetileg létrehozása skála megadása a virtuális gépeken futó száma.
    - A név és a virtuális gépeken a rendszergazdafiók jelszavát.
    - Név előtaggal az erőforrásokat, a támogatási skála megadása jönnek létre.

3. Változók értékek, amelyek gyakran módosíthatja és értékek, amelyek létre kívánja hozni az adatok kombinációjának paraméter megadása sablon használható. Adja hozzá ezeket a felvett a sablon változói szülő elem alatt változók.

        "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
        "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
        "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
        "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
        "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
        "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
        "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
        "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
        "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
        "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
          "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
        "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

  - A hálózati kapcsolatok által használt DNS-neveket.
    - Az IP-cím nevét és a prefixumokban a virtuális hálózati és alhálózat.
    - A nevek és azonosítóját, a virtuális hálózat terheléselosztó, és a hálózati kapcsolatok betöltése.
    - A fiókok a méretezés gépek társított tároló fióknevét beállítása.
    - Telepítve van a virtuális gépeken diagnosztika kiterjesztésű beállításait. Többet szeretne tudni a diagnosztika bővítmény a [Figyelés és Azure erőforrás-kezelő sablonnal diagnosztika Windows virtuális készülék létrehozása](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)című témakör tartalmaz.

4. Az erőforrások szülőelem, amelyhez hozzáadta a sablon alapján a tárhely fiók erőforrás hozzáadása Ezzel a sablonnal használ hurkot létrehozásához az ajánlott öt tárterület-fiókok operációs rendszer lemez és a diagnosztikai adatokat tároló. A fiókok beállítása támogatniuk kell a legfeljebb 100 virtuális gépeken futó skála meg, amely a jelenlegi maximális. Az Ön által a paraméterek a sablon előtag kombinálni változók definiált betű jelzéssel minden tároló fiók neve.

        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. A virtuális hálózati erőforrás hozzáadása További tudnivalókért olvassa el a [Hálózati erőforrás-szolgáltató](../virtual-network/resource-groups-networking.md)című témakört.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[variables('virtualNetworkName')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "subnet1",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },

6. Adja hozzá a terheléselosztó és a hálózati kapcsolat által használt nyilvános IP-cím erőforrások.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP1')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName1')]"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "[variables('publicIP2')]",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "[variables('dnsName2')]"
            }
          }
        },

7. A betöltés skála megadása által használt terheléselosztó erőforrás hozzáadása További tudnivalókért lásd: az [Erőforrás-kezelő támogatási Azure-terheléselosztó](../load-balancer/load-balancer-arm.md).

        {
          "apiVersion": "2015-06-15",
          "name": "[variables('loadBalancerName')]",
          "type": "Microsoft.Network/loadBalancers",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
          ],
          "properties": {
            "frontendIPConfigurations": [
              {
                "name": "loadBalancerFrontEnd",
                "properties": {
                  "publicIPAddress": {
                    "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
                  }
                }
              }
            ],
            "backendAddressPools": [ { "name": "bepool1" } ],
            "inboundNatPools": [
              {
                "name": "natpool1",
                "properties": {
                  "frontendIPConfiguration": {
                    "id": "[variables('frontEndIPConfigID')]"
                  },
                  "protocol": "tcp",
                  "frontendPortRangeStart": 50000,
                  "frontendPortRangeEnd": 50500,
                  "backendPort": 3389
                }
              }
            ]
          }
        },

8. Adja hozzá a hálózati kapcsolat erőforrás különálló virtuális gépen által használt. A skála megadása a gép nem egy nyilvános IP-cím keresztül érhető el, mert egy külön virtuális gép jön létre a azonos virtuális hálózat távoli eléréséhez a gép.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[variables('nicName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
            "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": {
                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
                  },
                  "subnet": {
                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                  }
                }
              }
            ]
          }
        },

9. Adja hozzá a külön virtuális gép ugyanabba a hálózatba, a méretarányra beállított.

        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[parameters('vmName')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "storageLoop",
            "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_A1" },
            "osProfile": {
              "computername": "[parameters('vmName')]",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "[concat(parameters('resourcePrefix'), 'os1')]",
                "vhd": {
                  "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"        
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
                }
              ]
            }
          }
        },

10. A virtuális gép skála beállítani az erőforrás hozzá, és adja meg a telepítve van az összes virtuális gépeken futó skála megadása a diagnosztika kiterjesztésű. Az erőforrás a beállítások közül számos hasonlítanak a virtuális gép erőforrással. A főbb különbségek a kapacitás elem, amely a virtuális gépeken futó számát adja meg a skála megadása, és arról, hogy hogyan módosításai frissítések virtuális gépeken futó upgradePolicy. Skála megadása nem jön létre, amíg a tárterület-fiókok létrehozásakor a dependsOn elem a megadott módon.

            {
              "type": "Microsoft.Compute/virtualMachineScaleSets",
              "apiVersion": "2016-03-30",
              "name": "[parameters('vmSSName')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "storageLoop",
                "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
              ],
              "sku": {
                "name": "Standard_A1",
                "tier": "Standard",
                "capacity": "[parameters('instanceCount')]"
              },
              "properties": {
                "upgradePolicy": {
                  "mode": "Manual"
                },
                "virtualMachineProfile": {
                  "storageProfile": {
                    "osDisk": {
                      "vhdContainers": [
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                        "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
                      ],
                      "name": "vmssosdisk",
                      "caching": "ReadOnly",
                      "createOption": "FromImage"
                    },
                    "imageReference": {
                      "publisher": "MicrosoftWindowsServer",
                      "offer": "WindowsServer",
                      "sku": "2012-R2-Datacenter",
                      "version": "latest"
                    }
                  },
                  "osProfile": {
                    "computerNamePrefix": "[parameters('vmSSName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                  },
                  "networkProfile": {
                    "networkInterfaceConfigurations": [
                      {
                        "name": "networkconfig1",
                        "properties": {
                          "primary": "true",
                          "ipConfigurations": [
                            {
                              "name": "ip1",
                              "properties": {
                                "subnet": {
                                  "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                                },
                                "loadBalancerBackendAddressPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                                  }
                                ],
                                "loadBalancerInboundNatPools": [
                                  {
                                    "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                                  }
                                ]
                              }
                            }
                          ]
                        }
                      }
                    ]
                  },
                  "extensionProfile": {
                    "extensions": [
                      {
                        "name": "Microsoft.Insights.VMDiagnosticsSettings",
                        "properties": {
                          "publisher": "Microsoft.Azure.Diagnostics",
                          "type": "IaaSDiagnostics",
                          "typeHandlerVersion": "1.5",
                          "autoUpgradeMinorVersion": true,
                          "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                            "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                          },
                          "protectedSettings": {
                            "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                          }
                        }
                      }
                    ]
                  }
                }
              }
            },

11. Adja hozzá a autoscaleSettings erőforrás, amely definiálja, hogyan skála megadása a processzor használatát skála megadása a gépeken alapján állítja be.

            {
              "type": "Microsoft.Insights/autoscaleSettings",
              "apiVersion": "2015-04-01",
              "name": "[concat(parameters('resourcePrefix'),'as1')]",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              ],
              "properties": {
                "enabled": true,
                "name": "[concat(parameters('resourcePrefix'),'as1')]",
                "profiles": [
                  {
                    "name": "Profile1",
                    "capacity": {
                      "minimum": "1",
                      "maximum": "10",
                      "default": "1"
                    },
                    "rules": [
                      {
                        "metricTrigger": {
                          "metricName": "\\Processor(_Total)\\% Processor Time",
                          "metricNamespace": "",
                          "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                          "timeGrain": "PT1M",
                          "statistic": "Average",
                          "timeWindow": "PT5M",
                          "timeAggregation": "Average",
                          "operator": "GreaterThan",
                          "threshold": 50.0
                        },
                        "scaleAction": {
                          "direction": "Increase",
                          "type": "ChangeCount",
                          "value": "1",
                          "cooldown": "PT5M"
                        }
                      }
                    ]
                  }
                ],
                "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
              }
            }

    Ebben az oktatóanyagban fontosak a következő értékeket:

    - **metricName** – Ez az érték megegyezik a teljesítmény számláló a wadperfcounter változóban definiált. Változó használ, a diagnosztikai bővítmény gyűjti össze a **processzor(_Total)\% processzor** számláló.
    - **metricResourceUri** – Ez az érték az erőforrás-azonosító virtuális gép skála megadása a rendszer.
    - **timeGrain** – az értéke granularitása a begyűjtési mérési módja miatt. Ez a sablon a beállítás egy perc alatt.
    - **Statisztika** – Ez az érték azt határozza meg, hogyan a mértékek együttes használata kiterjesztve azt az automatikus méretezés művelet. A lehetséges értékek: átlag, minimum, maximum. A virtuális gépeken futó átlagos teljes processzorhasználata kell felvenni, az ezzel a sablonnal.
    - **az időtartomány értékének** – Ez az érték cellatartomány, amely az idő, amelyben példány adatgyűjtés. 5 perccel és 12 órás között kell lennie.
    - **timeAggregation** – Ez az érték azt határozza meg, hogyan gyűjtött adatok időbeli össze kell-e. Az alapértelmezett érték átlagát. A lehetséges értékek: átlag, Minimum, Maximum, utolsó, összeg, darab.
    - **operátor** – az értéke az operátor, amely a metrikus adatok és a küszöbértékét összehasonlítása. A lehetséges értékek: egyenlő, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    - **küszöb** – ezt az értéket az érték eseményindítók a méretezés műveletet. Ez a sablon a megadása esetén az átlagos processzorhasználata megadása gépek között több mint 50 %-os méretarányra gépeken futó ad hozzá.
    - **irányba** – Ez az érték határozza meg, hogy a műveletet, amely a küszöbértéket érhető el. A lehetséges értékei növelése vagy csökkentése. A sablonban skála megadása a virtuális gépeken futó száma nő, amikor a küszöbértékét több mint a definiált időkeret 50 %-kal.
    - **típus** – Ez az érték a művelet, amelyet a mi történjen, és meg kell ChangeCount típusa.
    - **érték** – az értéke virtuális gépeken futó hozzáadott vagy el vannak távolítva a skála megadása a számát. Ez az érték 1, vagy nagyobb kell lennie. Az alapértelmezett érték 1. Ez a sablon a skála gépek számát állítsa nő 1-gyel küszöbértéket teljesülése esetén.
    - **cooldown** – ezt az értéket az az időtartam a következő művelet beállítása óta a legutóbbi méretezési művelet elteltével. Ez az érték egy perc alatt, és egy hét között kell lennie.

12. Mentse a sablont fájlt.    

## <a name="step-4-upload-the-template-to-storage"></a>Lépés: 4: A sablon feltöltése a tárhely

A sablon tölthet fel, feltéve, hogy tudja, hogy a neve és a tárterület-fiók lépés: 1 létrehozott elsődleges kulcs.

1.  A Microsoft Azure PowerShell ablakában beállítása egy változó, amely meghatározza az Ön által létrehozott az 1 tároló fiók nevét.

            $storageAccountName = "vmstestsa"

2.  Egy változó, amely meghatározza az elsődleges kulcs a tárterület-fiók beállítása

            $storageAccountKey = "<primary-account-key>"

    A kulcs elérheti a fő ikonjára kattint, a tárhely fiók erőforrás az Azure-portálon megtekintésekor.

3.  Hozzon létre tároló fiók helyi-objektumot műveletek a tárterület-fióknak érvényesítésére szolgál.

            $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

4.  Hozzon létre a tárolója tárolja a sablont.

            $containerName = "templates"
            New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob

5.  A webhelysablon feltöltése a új tároló.

            $blobName = "VMSSTemplate.json"
            $fileName = "C:\" + $BlobName
            Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx

## <a name="step-5-deploy-the-template"></a>5 lépés: A sablon terjesztése

Most, hogy a sablon hozta létre, indítsa el az erőforrások telepítésével. Ez a parancs segítségével a folyamat megkezdéséhez:

    New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"

Amikor megnyomja adja meg, adjon meg értéket a hozzárendelt változók a rendszer kéri. Adja meg a következő értékeket:

    vmName: vmsstestvm1
      vmSSName: vmsstest1
      instanceCount: 5
      adminUserName: vmadmin1
      adminPassword: VMpass1
      resourcePrefix: vmsstest

Telepítendő sikeresen összes erőforrás körülbelül 15 percet kell vennie.

>[AZURE.NOTE] A portál lehetőséget az erőforrások üzembe is használhatja. Ide: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"

## <a name="step-6-monitor-resources"></a>Lépés a 6: Monitor erőforrások

Szerezheti be az alábbi módszerekkel virtuális gép skála beállítása információkat:

 - Az Azure - portálon jelenleg elérheti a portálon információk csak korlátozott mennyiségű.
 - Az [Azure erőforrás Intéző](https://resources.azure.com/) - e eszköze leginkább megfelelő felfedezése a méretezés beállítása az aktuális állapotát. Hajtsa végre az elérési út, és meg kell jelennie az Ön által létrehozott skála megadása a példány megtekintése:

        subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

 - Azure PowerShell – ezzel a paranccsal néhány információ:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

        Or

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView

 - Csatlakozás a külön virtuális gép ugyanúgy, mint ahogyan bármelyik másik számítógépre, és ezután távolról érheti el a Lync-folyamatok beállítása méretezés a virtuális gépeken futó.

>[AZURE.NOTE] Egy teljes REST API-skála készletek kapcsolatos információk beszerzése program a [Virtuális gép skála állítja be.](https://msdn.microsoft.com/library/mt589023.aspx)

## <a name="step-7-remove-the-resources"></a>7 lépés: Az erőforrások eltávolítása

Mivel az Azure-ban használt erőforrások előfizetést terhelő, tanácsos mindig egy információforrások, amelyek már nem szükséges törlése. Nem kell az egyes erőforrások erőforráscsoport külön-külön törlése. Az erőforráscsoport törölheti és összes erőforrás automatikusan törlődnek.

    Remove-AzureRmResourceGroup -Name vmsstestrg1

Ha meg szeretné tartani az erőforráscsoport, állítsa be a csak a méretezés törölheti.

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"

## <a name="next-steps"></a>Következő lépések

- Kezelése az imént létrehozott információk felhasználásával [kezelése virtuális gépeken futó virtuális gép skála megadása a](virtual-machine-scale-sets-windows-manage.md)skála megadása.
- További tudnivalók a [Függőleges Automatikus méretezéssel virtuális gép skála értékkészletet](virtual-machine-scale-sets-vertical-scale-reprovision.md) megtekintésével függőleges méretezése
- Keresse meg az [Azure Monitor PowerShell gyors indítása minták](../monitoring-and-diagnostics/insights-powershell-samples.md) lehetőségei figyelése Azure Monitor példák
- [Automatikus méretezéssel műveletek Azure Monitor e-mailek és webhook értesítések küldésére](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md) használt értesítési szolgáltatásainak bemutatása
- Megtudhatja, hogyan [használata naplók küldése e-mailek és webhook értesítések az Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
