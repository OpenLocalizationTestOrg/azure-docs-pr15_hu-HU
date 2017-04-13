<properties
    pageTitle="Automatikus méretezés és virtuális gép méretezni beállítása |} Microsoft Azure"
    description="Tudjon meg többet diagnosztikai és Automatikus méretezéssel erőforrások automatikusan átméretezni a virtuális gépeken futó skála meg."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="automatic-scaling-and-virtual-machine-scale-sets"></a>Automatikus méretezés és virtuális gép méretezni beállítása

Virtuális gépeken futó skála meg az automatikus méretezés az, a létrehozási vagy a törlés gépek teljesítmény igényeknek megfelelően alakíthatja szükség szerint megadása. A munka mennyiségű növekedésével alkalmazás megkövetelheti további információforrásokat lehetővé teszi, hogy hatékony feladatokat.

Automatikus méretezés egy automatizált eljárással, hogy a segít management terhelést megkönnyítése érdekében. Csökkentésével felsőbb, nem kell folyamatosan rendszerteljesítmény megfigyelése, és eldöntheti, hogyan kezelheti az erőforrásokat. Méretezés az egy rugalmas folyamat. További források manuálisan is hozzáadhatók, mint a betöltés nő, de igény szerint csökkenések erőforrások költségek kisméretűvé alakítása és karbantartása teljesítményszint lehet eltávolítani.

Állítsa be az automatikus méretezés méretaránnyal egy erőforrás-kezelő Azure-sablont, Azure PowerShell, Azure CLI vagy az Azure portál használatával.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Erőforrás-kezelő használatával történő méretezési beállítása

Telepítésével és kezelésével külön-külön, az egyes erőforrásokhoz az alkalmazás használata helyett egy sablont, amely egyetlen, összehangolt művelet összes erőforrás üzembe helyezése. A sablonban alkalmazás erőforrások határozza meg, és üzembe paraméterek különböző környezetekben esetében a meghatározott. A sablont, ahol a JSON és kifejezések, amelyeket a telepítéshez értékek összeállításához használhat. További információért tekintse meg a [szerzői Azure erőforrás-kezelő sablonokat](../resource-group-authoring-templates.md).

A sablont adja meg a beosztását elem:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 3
    },

Kapacitás azonosítja a virtuális gépeken futó megadása számát. Manuálisan módosíthatja a kapacitás sablon eltérő értékű telepítésével. Ha szeretné módosítani a kapacitás sablon telepít, csak a frissített kapacitással Termékváltozat elemet is.

Automatikus módosítása a méretezés az autoscaleSettings erőforrás, ha a diagnosztika bővítmény használatával kapacitása.

### <a name="configure-the-azure-diagnostics-extension"></a>Az Azure diagnosztika bővítmény konfigurálása

Automatikus méretezés csak végezhető el a virtuális gépeken skála megadása a sikeres mértékek webhelycsoport esetén. Az Azure diagnosztika bővítmény az igényeinek megfelelő a mértékek webhelycsoport az Automatikus méretezéssel erőforrás figyelés és diagnosztika funkciók ismertetése A bővítmény telepítheti az erőforrás-kezelő sablon részeként.

Ebben a példában használt változók a sablonban a diagnosztika bővítmény konfigurálása:

    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"

Paraméterek találhatók, ha telepíti a sablont. Ebben a példában a adatokat tároló tárterület-fiók és a nevét, amelyhez adatgyűjtés skála megadása találhatók. A Windows Server példában is csak a szál teljesítmény számláló kell felvenni. A Windows vagy Linux rendszerhez elérhető teljesítmény számláló diagnosztika információk összegyűjtéséhez is használható, és a bővítmény konfiguráció beépíthetők.

Ebben a példában a bővítmény meghatározása a sablonban:

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
              "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
              "storageAccountEndPoint": "https://core.windows.net"
            }
          }
        }
      ]
    }

A diagnosztikai bővítmény futtatásakor az adatgyűjtés egy táblázat, amely a megadott tárterület-fiókjában található. A WADPerformanceCounters táblázatban megtalálhatja a gyűjtött adatok:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>A autoScaleSettings erőforrások beállítása

A autoscaleSettings erőforrás kívánja-e növelheti vagy csökkentheti hány virtuális gépeken futó skála megadása a diagnosztika bővítmény adatait használja.

Ebben a példában az erőforrás beállításait a sablonban:

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
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 650
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "\\Process(_Total)\\Thread Count",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 550
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
      }
    }

A fenti példában két szabályt az automatikus méretezés műveletek definiálása jönnek létre. Az első szabály definiálja a méretezési műveletet, és a második szabály határozza meg, a méretarány a műveletet. Ezeket az értékeket a szabályok találhatók:

- **metricName** – Ez az érték megegyezik az a teljesítmény számláló diagnosztika bővítményhez wadperfcounter változóban megadott. A fenti példában a szál számláló használják.  
- **metricResourceUri** – Ez az érték az erőforrás-azonosító virtuális gép skála megadása a rendszer. Az azonosító az erőforráscsoport a nevét, az erőforrás-szolgáltató neve és a skála megadása, ha át kívánja méretezni a nevét tartalmazza.
- **timeGrain** – az értéke granularitása a begyűjtési mérési módja miatt. Az előző példában adatgyűjtés a elvégezve egy perc alatt. Ez az érték az időtartomány értékének használatos.
- **Statisztika** – Ez az érték azt határozza meg, hogyan a mértékek együttes használata kiterjesztve azt az automatikus méretezés művelet. A lehetséges értékek: átlag, minimum, maximum.
- **az időtartomány értékének** – Ez az érték cellatartomány, amely az idő, amelyben példány adatgyűjtés. 5 perccel és 12 órás között kell lennie.
- **timeAggregation** – Ez az érték azt határozza meg, hogyan gyűjtött adatok időbeli össze kell-e. Az alapértelmezett érték átlagát. A lehetséges értékek: átlag, Minimum, Maximum, utolsó, összeg, darab.
- **operátor** – az értéke az operátor, amely a metrikus adatok és a küszöbértékét összehasonlítása. A lehetséges értékek: egyenlő, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
- **küszöb** – ezt az értéket az érték eseményindítók a méretezés műveletet. Ügyeljen arra, hogy a méretezési művelet küszöbértékét és a méretezés művelet küszöbértékét elegendő különbségét adja meg. Ha meg kell egyeznie az értékek, a rendszer helyesen a állandó módosítás, amely megakadályozza, hogy egy méretezési művelet végrehajtása. Ha például az előző példában beállítás mind 600 szálak nem működik.
- **irányba** – Ez az érték határozza meg, hogy a műveletet, amely a küszöbértéket érhető el. A lehetséges értékei növelése vagy csökkentése.
- **típus** – Ez az érték a művelet, amelyet a mi történjen, és meg kell ChangeCount típusa.
- **érték** – az értéke virtuális gépeken futó hozzáadni vagy el vannak távolítva a skála megadása a számát. Ez az érték 1, vagy nagyobb kell lennie.
- **cooldown** – ezt az értéket az az időtartam a következő művelet beállítása óta a legutóbbi méretezési művelet elteltével. Ez az érték egy perc alatt, és egy hét között kell lennie.

Attól függően, hogy a teljesítmény számláló használja a elemeivel a sablon konfigurációban használt másképp. Az előző példában a teljesítmény számláló szálak száma, küszöb értéke méretezési tevékenység 650, pedig a küszöbérték 550 a méretezés a művelet. A számláló például % processzor használata esetén küszöb értéke a határozza meg, hogy egy méretezési művelet processzorhasználata százalékos.

A betöltés eseményindítók a méretezés művelet virtuális gépeken létrehozásakor megadása kapacitása az érték a sablon alapján nő. Például a skálával azok a kapacitás értéke (3) és a méretezés művelet értéket beállítása értéke 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

A betöltés létrehozásakor az átlagos szálak 650 küszöbérték felett Ugrás okozó:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

A méretezési művelet induljanak okozó kapacitása ahhoz meg, hogy újat:

    "sku": {
      "name": "Standard_A0",
      "tier": "Standard",
      "capacity": 4
    },

És virtuális gép bekerül a skála megadása:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Egy után cooldown öt perc átlagos száma az számítógépeken szálak maradjon feletti 600, ha egy másik számítógépen bekerül a beállítás. Az átlagos szálak 550 alatt maradjon, ha eggyel csökken skála megadása kapacitásával, és egy számítógépre megadása törlődik.

## <a name="set-up-scaling-using-azure-powershell"></a>Azure PowerShell használatával méretezési beállítása

Példák a PowerShell használatá autoscaling beállítani, tekintse meg [Azure Monitor PowerShell gyors indítása a minták](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Azure CLI méretezést beállítása

Példák az Azure CLI használó autoscaling beállítani, tekintse meg [Azure Monitor platformok CLI gyors indítása minták](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Az Azure portálon méretezési beállítása

Példa az Azure portal segítségével állíthatja be autoscaling megtekintéséhez nézze meg [a virtuális gép skála megadása létrehozása az Azure portál használatával](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Vizsgálja meg a méretezési műveletek

- [Azure portál]() - információk a portálon korlátozott mennyiségű jelenleg elérheti.
- [Azure erőforrás Explorer]() – Ez az eszköz nem leginkább megfelelő felfedezése a méretezés beállítása az aktuális állapotát. Kövesse az elérési út, és meg kell jelennie a példány nézete az Ön által létrehozott skála megadása: előfizetések > {az előfizetés} > resourceGroups > {az erőforráscsoport} > szolgáltató > Microsoft.Compute > virtualMachineScaleSets > {a skála megadása} > virtualMachines
- Azure PowerShell – ezzel a paranccsal néhány információ:

        Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
        Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput

- Csatlakozás a jumpbox virtuális gép ugyanúgy, mint ahogyan bármelyik másik számítógépre, és ezután távolról érheti el a Lync-folyamatok beállítása méretezés a virtuális gépeken futó.

## <a name="next-steps"></a>Következő lépések

- Tekintse meg az [automatikus méretezése gépek virtuális gép skála megadása](virtual-machine-scale-sets-windows-autoscale.md) egy példa bemutatja, hogyan hozhat létre az automatikus méretezés konfigurált skálával megtekintéséhez.
- Keresse meg az [Azure Monitor PowerShell gyors indítása minták](../monitoring-and-diagnostics/insights-powershell-samples.md) lehetőségei figyelése Azure Monitor példák
- Funkciókról értesítési az [előre küldése e-mailek és webhook értesítések az Azure Monitor Automatikus méretezéssel műveleteket](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).
- Ismerje meg arról, hogy miként [használata naplók küldése e-mailek és webhook értesítések az Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
- Tudjon meg többet a [Speciális Automatikus méretezéssel esetek](./virtual-machine-scale-sets-advanced-autoscale.md).
