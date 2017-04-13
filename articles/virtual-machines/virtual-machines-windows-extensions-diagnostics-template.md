<properties
    pageTitle="A figyelés és Azure erőforrás-kezelő sablonnal diagnosztika Windows virtuális gép létrehozása |} Microsoft Azure"
    description="Azure erőforrás manager sablon használatával hozzon létre egy új Windows virtuális gép Azure diagnosztika kiterjesztésű."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# <a name="create-a-windows-virtual-machine-with-monitoring-and-diagnostics-using-azure-resource-manager-template"></a>A figyelés és Azure erőforrás-kezelő sablonnal diagnosztika Windows virtuális gép létrehozása

Az Azure diagnosztika bővítmény tartalmaz, nyomon követése, illetve diagnosztika funkciók a Windows Azure virtuális gép alapján. Ezek a funkciók a virtuális gépen engedélyezheti a mellékkel együtt a azure erőforrás-kezelő sablon részeként. Lásd: [Azure erőforrás-kezelő sablonok létrehozása virtuális kiterjesztésű](virtual-machines-windows-extensions-authoring-templates.md) többek között a bármelyik mellék virtuális gép sablon részeként további információt. Ez a cikk azt ismerteti, hogyan adhat az Azure diagnosztika bővítmény windows virtuális gép sablon.  
  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>A virtuális erőforrás meghatározása a Azure diagnosztika bővítmény hozzáadása 

Ahhoz, hogy a diagnosztika kiterjesztése a Windows virtuális gép fel kell vennie a bővítmény, az erőforrás-kezelő sablon virtuális erőforrásként.

Az egyszerű erőforrás-kezelő alapján virtuális gép hozzáadása a bővítmény konfiguráció a *források* tömböt a virtuális gépen: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Egy másik közös kiállítás van adjon hozzá a bővítmény konfiguráció a legfelső szintű erőforrások csomópontot a sablon helyett definiálásáról a csoportban a virtuális gép erőforrások csomópontot. Az ezt a módszert, ha van pontos megadásához a bővítmény és a virtuális gép hierarchikus kapcsolata a *név* és *típus* értékekkel. Példa: 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

A bővítmény mindig a virtuális gép társítva, vagy közvetlenül közvetlenül meghatározása a virtuális gép erőforrás csomópont alatt, vagy adjon meg egy alap szintre és használható a hierarchikus elnevezési konvenció a virtuális gép társítandó.

A bővítmények konfiguráció virtuális gép skála eredménye a a *VirtualMachineProfile* *extensionProfile* tulajdonsága van megadva.
   
A *publisher* tulajdonság **Microsoft.Azure.Diagnostics** értékkel és a *típusa* tulajdonság **IaaSDiagnostics** értékkel egyedileg azonosító az Azure diagnosztika kiterjesztést.

A *name* tulajdonság értéke lehet hivatkozni a bővítmény az erőforráscsoport használható. Kifejezetten **Microsoft.Insights.VMDiagnosticsSettings** beállítást lehetővé teszi, hogy egyszerűen azonosíthatók az Azure klasszikus portál portál biztosítva, hogy a felügyeleti diagramok jelenik meg megfelelően az Azure klasszikus portálon.

A *typeHandlerVersion* adja meg a használni kívánt bővítmény verzióját. *AutoUpgradeMinorVersion* beállítás **Igaz** alverzió biztosítja, a bővítmény elérhető legújabb alverzió fog kapni. Ajánlott *autoUpgradeMinorVersion* **mindig igaznak így mindig a legújabb elérhető diagnosztika bővítmény használatára az új szolgáltatások és hibajavítások** mindig beállítása. 

A *Beállítások* elemet a bővítmény beállítása, beolvassa a bővítmény (néven is ismert konfigurációs nyilvános) konfigurációk tulajdonságait tartalmazza. A *xmlcfg* tulajdonság tartalmazza a diagnosztikai naplók xml-alapú konfiguráció a teljesítmény számláló, amely a diagnosztika ügynök lesznek összegyűjtve stb. [Diagnosztikai konfigurációs séma](https://msdn.microsoft.com/library/azure/dn782207.aspx) talál további információt az XML-sémát, magát. Általános gyakorlat, hogy a tényleges xml konfiguráció tárolására az erőforrás-kezelő Azure sablonban változóként, és kattintson az ÖSSZEFŰZ és base64 kódolás *xmlcfg*értékének beállítása. A [diagnosztikai konfigurációs változóinak](#diagnostics-configuration-variables) további információ az XML-tartalom tárolása a változók című szakaszában olvashat. A *storageAccount* tulajdonság neve a tárterület-fiókot, amelyhez diagnosztikai adatok átkerülnek. 
 
A Tulajdonságok *protectedSettings* (néven is ismert személyes konfigurációs) beállítható, hogy, de nem tudja olvasni után vissza beállítása. *ProtectedSettings* csak olvasásra jellegét lehetővé teszi a titkos kulcsok, például a tárhely fiókkulcs tárolásához hasznos hol kell írni a diagnosztikai adatok.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Diagnosztikai tároló fiók megadása paraméterként 

A diagnosztikai bővítmény json kódtöredékének fenti feltételezi, hogy két paraméterek *existingdiagnosticsStorageAccountName* és *existingdiagnosticsStorageResourceGroup* adja meg a diagnosztikai adatokat tároló diagnosztika tárterület-fiókhoz. A diagnosztikai tárterület-fiók megadása, mint paraméter teszi egyszerűen diagnosztika tárterület-fiók módosítása támogatása a különböző környezetekben például érdemes használjon másik diagnosztika tárhely a tesztelés és egy másik a éles üzemi a.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

Meg kell adnia egy diagnosztika tárolási számlát különböző erőforráscsoport, mint a virtuális gép az erőforráscsoport értékre. Erőforráscsoport lehet tekinteni egy saját élettartam telepítési egységgel, egy virtuális gép rendszerbe, és új konfigurációk frissítések történik azt, de továbbra is a ugyanazt a tárterület-fiókot adott virtuális gép telepítések végig a diagnosztikai adatok tárolása érdemes üzlethez. A tárterület-fiókot kellene egy másik kódforrás lehetővé teszi, hogy a tárhely fiók fogadja el a különféle, ezzel leegyszerűsítve kapcsolatos problémák megoldása a különböző verziói közötti virtuális gép telepítések adatait.

>[AZURE.NOTE] Ha windows virtuális gép sablon készítése a Visual Studio az alapértelmezett tároló fiók lehet beállítani ugyanazt a tárhely fiókot használja, ahol a virtuális gép virtuális töltenek fel. Ez az első telepítése a virtuális egyszerűsítése érdekében. Újra kell tényező, a sablon paraméterként továbbíthatók a különböző tárterület-fiókkal. 

## <a name="diagnostics-configuration-variables"></a>Diagnosztikai konfigurációs változóinak
 
A diagnosztikai bővítmény json kódtöredékének fenti egy *accountid* változó a tárhely fiókkulcs diagnosztika tárolására szolgáló első egyszerűsítése érdekében határozza meg:   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


A *xmlcfg* diagnosztika bővítményhez definiálva tulajdonsága közös tömb összefűzéséből több változók használata. E változók értékei az XML-ben, hogy helyesen escape a json változók beállításakor van szükségük.

A következő ismerteti, hogy összegyűjti a normál rendszer szintű teljesítmény számláló néhány windows-eseménynaplók és a diagnosztikai naplók-infrastruktúrát diagnosztika konfigurációs xml. Van már escape és formázott megfelelően, hogy a konfigurációs közvetlenül illeszthet be a sablon változói szakaszába. Lásd: a [Diagnosztikai konfigurációs séma](https://msdn.microsoft.com/library/azure/dn782207.aspx) konfigurációs XML több emberi olvasható például.
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

A mértékek definition xml csomópontot a fenti konfiguráció egy fontos konfigurációs elem megegyezik hogyan a teljesítmény számláló *PerformanceCounter* csomópont XML-adatok a korábban definiált fogja összesíteni és tárolt határozza meg. 

> [AZURE.IMPORTANT] Ezek a mértékek meghajtó nyomon a diagramok és értesítések az Azure-portálon.  Ha meg szeretné jeleníteni a virtuális felügyeleti adatok az Azure-portálon a virtuális diagnosztika konfigurációban szerepelnie kell a *resourceID* és **MetricAggregation** a **Mértékek** csomópontot. 

Az alábbi képen az xml-definíciók mértékek: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

A *resourceID* attribútum egyedileg kell azonosítania a virtuális gép az előfizetést. Győződjön meg róla, hogy a subscription() és resourceGroup() függvényt használjuk, így a sablon automatikusan frissíti az előfizetés és telepít, az erőforráscsoport alapján ezeket az értékeket.

Ha ismétlődve hoz létre több virtuális gépeken futó majd be kell egy copyIndex() függvény minden egyes virtuális megfelelően megkülönböztetéséhez *resourceID* értékkel feltöltéséhez. A *xmlCfg* értéket támogatásához a következőképpen frissíthető:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

*PT1H* és *PT1M* MetricAggregation értékének időtartamon egy összesítést egy perc alatt, és egy óra fölé egy összesítést.

## <a name="wadmetrics-tables-in-storage"></a>A tároló WADMetrics táblák

A fenti konfiguráció a mértékek táblák hoz létre, a diagnosztikai tároló fiókját, a következő elnevezési szabályokat:

- **WADMetrics** : az összes WADMetrics táblához előtaggal
- **PT1H** vagy **PT1M** : azt jelzi, hogy a táblázatnak több mint 1 órára vagy az 1 perc összesített adatok
- **P10D** : azt jelzi, hogy a táblázat adatok jelennek meg a bekapcsolásakor a adatok gyűjtése a táblázat 10 napig
- **V2S** : szövegkonstans.
- **ééééhhnn** : A dátum, amelyen a táblázat lépések adatok gyűjtése

Példa: *WADMetricsPT1HP10DV2S20151108* fölé egy óra, 11 – november-2015 elindítása 10 napig összesíti mértékek adatokat fog tartalmazni    

Minden egyes WADMetrics táblázat lesz az alábbi oszlopokat tartalmazza:

- **PartitionKey**: A partitionkey összeállítás azonosítja a virtuális erőforrás *resourceID* értéke alapján. a pl.: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : követi formátuma <Descending time tick>:<Performance Counter Name>. A csökkenő idő lépték számítás maximális időt osztások mínusz a összesítési kezdetétől időtartamát. Pl. Ha a minta időszak elindítva a 10-november-2015 és 00:00Hrs UTC, majd a számításban a következő lesz: DateTime.MaxValue.Ticks - (új DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Osztások). A rendelkezésre álló bájtok teljesítményének a sor kulcs Számláló memória jelenik meg, például: 2519551871999999999__:005CMemory:005CAvailable:0020 bájt
- **CounterName** : a teljesítmény számláló neve. Ez a xml config definiált *counterSpecifier* felel meg.
- **Maximális** : A maximális értéke a teljesítmény számláló összesítési időszak alatt.
- **Minimális** : A legkisebb értéket a teljesítmény számláló összesítési időszak alatt.
- **Teljes** : a teljesítmény számláló értékeinek összege a jelentett aggregáció időszak alatt.
- **Darab** : a teljesítmény számláló jelzett értékek száma.
- **Átlagos** : az összesítés időszakban a teljesítmény számláló átlag (összesen /) értékét.


## <a name="next-steps"></a>Következő lépések

- A Windows virtuális gép diagnosztika kiterjesztésű teljes űrlapsablonok című [201 virtuális-ellenőrzés – diagnosztika-bővítmény](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- Az erőforrás manager sablon [Azure PowerShell](virtual-machines-windows-ps-manage.md) alrendszerrel vagy [Azure parancssori](virtual-machines-linux-cli-deploy-templates.md) terjesztése
- További tudnivalók [a szerzői Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)







