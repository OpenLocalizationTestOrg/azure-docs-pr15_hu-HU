<properties
    pageTitle="Azure Monitor autoscaling közös mértékek. | Microsoft Azure"
    description="Ismerje meg, hogy melyik mértékek autoscaling gyakran használják a Felhőszolgáltatások, virtuális gépeken futó és Web Apps alkalmazások."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitor autoscaling közös mérőszámok

Azure Monitor autoscaling lehetővé teszi, ha át kívánja méretezni futó példányok száma, felfelé vagy lefelé, telemetriai adatokat (metrikus) alapján. A dokumentum közös mértékek, akkor előfordulhat, hogy használni kívánt ismerteti. Az Azure portálon a Felhőszolgáltatások és a kiszolgálófarm megadhatja a mérőszám, ha át kívánja méretezni az erőforrás. Azonban is választhat tetszőleges metrikus egységek különböző erőforrás által méretezni.

Íme a részletek a megkeresése, és azt szeretné, ha át kívánja méretezni a mérőszámok listában. A következő vonatkozik, valamint a virtuális gép skála készletek méretezéshez.

## <a name="compute-metrics"></a>Mértékek kiszámítása
Alapértelmezés szerint Azure virtuális v2 konfigurált diagnosztika kiterjesztésű megtalálható, és be van kapcsolva, a következő mértékek rendelkeznek.

- [A vendégként való bekapcsolódáshoz mértékek Windows virtuális v2](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [A vendégként való bekapcsolódáshoz mértékek Linux virtuális v2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Használhatja a `Get MetricDefinitions` API/PoSH/CLI megtekintése a mérési módja miatt a VMSS erőforrás elérhető. 

Virtuális skála készletek használ, és nem látja az egy adott mérőszám szerepel a listában, majd érdemes valószínűleg *tiltható le* a diagnosztikai bővítmény.

Ha egy adott mérőszám nem készül mintát, vagy meghatározott gyakorisággal, amelyet át, frissítheti a diagnosztikai konfigurációja.

Ha a fenti mindkét esetben eredménye IGAZ, majd tekintse át a [PowerShell használatá engedélyezése egy virtuális gépen futó Windows Azure diagnosztika](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) kapcsolatban a PowerShell beállítása és frissítése az Azure virtuális diagnosztika bővítmény ahhoz, hogy a mérőszám. A cikk a diagnosztika konfigurációs mintafájl is tartalmaz.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Operációs rendszer vendégként számítja ki a mértékek Windows virtuális v2

Azure-ban egy új virtuális (v2) létrehozásakor diagnosztika engedélyezve van a diagnosztika bővítmény használatával.

A mérési módja miatt listáját a következő parancsot a PowerShell használatával hozhat létre.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

A következő mérési módja miatt értesítés hozhat létre.

|Metrikus neve|   Egység|
|---|---|
|\Processor(_Total)\% processzor    |Százalék|
|\Processor(_Total)\% kiemelt jogosultságokkal rendelkezik az idő   |Százalék|
|\Processor(_Total)\% felhasználói idő |Százalék|
|\Processor információk (_Total) \Processor gyakorisága |Darabszám|
|\System\Processes| Darabszám|
|\Process (_Total) \Thread száma| Darabszám|
|\Process (_Total) \Handle száma  |Darabszám|
|\Memory\% előjegyzett véglegesítése   |Százalék|
|\Memory\Available bájt|   Bájt|
|\Memory\Committed bájt    |Bájt|
|\Memory\Commit korlát|  Bájt|
|\Memory\Pool lapozható bájt|  Bájt|
|\Memory\Pool lapozható|   Bájt|
|\PhysicalDisk(_Total)\% lemezre idő| Százalék|
|\PhysicalDisk(_Total)\% lemez olvasási idő|    Százalék|
|\PhysicalDisk(_Total)\% lemezre írási idő|   Százalék|
|\PhysicalDisk (_Total) \Disk átvitelek/sec   |CountPerSecond|
|Olvasás/sec \PhysicalDisk (_Total) \Disk   |CountPerSecond|
|Írások/sec \PhysicalDisk (_Total) \Disk  |CountPerSecond|
|\PhysicalDisk (_Total) \Disk bájt/sec   |BytesPerSecond|
|Olvassa el a bájt/sec \PhysicalDisk (_Total) \Disk| BytesPerSecond|
|\PhysicalDisk (_Total) \Disk írási bájt/sec |BytesPerSecond|
|\Avg \PhysicalDisk (_Total). Lemez hossza|  Darabszám|
|\Avg \PhysicalDisk (_Total). Lemez olvasható hossza| Darabszám|
|\Avg \PhysicalDisk (_Total). Lemez írási hossza |Darabszám|
|\LogicalDisk(_Total)\% szabad lemezterület| Százalék|
|\LogicalDisk (_Total) \Free MB|   Darabszám|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Operációs rendszer vendégként számítja ki a mértékek Linux virtuális v2

Azure-ban egy új virtuális (v2) létrehozásakor diagnosztika alapértelmezés szerint engedélyezve van diagnosztika bővítmény használatával.

A mérési módja miatt listáját a következő parancsot a PowerShell használatával hozhat létre.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 A következő mérési módja miatt értesítés hozhat létre.

|Metrikus neve                            |Egység|
|---|---|
|\Memory\AvailableMemory                |Bájt|
|\Memory\PercentAvailableMemory         |Százalék|
|\Memory\UsedMemory                     |Bájt|
|\Memory\PercentUsedMemory              |Százalék|
|\Memory\PercentUsedByCache             |Százalék|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Bájt|
|\Memory\PercentAvailableSwap           |Százalék|
|\Memory\UsedSwap                       |Bájt|
|\Memory\PercentUsedSwap                |Százalék|
|\Processor\PercentIdleTime             |Százalék|
|\Processor\PercentUserTime             |Százalék|
|\Processor\PercentNiceTime             |Százalék|
|\Processor\PercentPrivilegedTime       |Százalék|
|\Processor\PercentInterruptTime        |Százalék|
|\Processor\PercentDPCTime              |Százalék|
|\Processor\PercentProcessorTime        |Százalék|
|\Processor\PercentIOWaitTime           |Százalék|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Másodperc|
|\PhysicalDisk\AverageWriteTime         |Másodperc|
|\PhysicalDisk\AverageTransferTime      |Másodperc|
|\PhysicalDisk\AverageDiskQueueLength   |Darabszám|
|\NetworkInterface\BytesTransmitted     |Bájt|
|\NetworkInterface\BytesReceived        |Bájt|
|\NetworkInterface\PacketsTransmitted   |Darabszám|
|\NetworkInterface\PacketsReceived      |Darabszám|
|\NetworkInterface\BytesTotal           |Bájt|
|\NetworkInterface\TotalRxErrors        |Darabszám|
|\NetworkInterface\TotalTxErrors        |Darabszám|
|\NetworkInterface\TotalCollisions      |Darabszám|




## <a name="commonly-used-web-server-farm-metrics"></a>A gyakran használt webes (kiszolgálófarm) mérőszámok

Automatikus méretezéssel, például a Http várólista hossza közös webes kiszolgáló mértékek alapján is elvégezheti. Annak metrikus neve **HttpQueueLength**.  A következő szakasz megjeleníti a kiszolgáló elérhető farm (a Web Apps alkalmazások) mértékek.

### <a name="web-apps-metrics"></a>Web Apps alkalmazások mérőszámok

A Web Apps alkalmazások mérési módja miatt listáját a következő parancsot a PowerShell használatával hozhat létre.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

A riasztást, vagy átméretezheti a mértékek alapján.

|Metrikus neve        |Egység|
|---                |---|
|CpuPercentage      |Százalék|
|MemoryPercentage   |Százalék|
|DiskQueueLength    |Darabszám|
|HttpQueueLength    |Darabszám|
|BytesReceived      |Bájt|
|BytesSent          |Bájt|


## <a name="commonly-used-storage-metrics"></a>A gyakran használt tárolási mértékek
Méretezheti tároló várólista hossz a tárhely várakozási sorban található üzenetek számát. Tárterület várólista hossza egy speciális mérőszám, és a alkalmazott küszöbértékét példányonként üzenetek számát. Ez azt jelenti, ha két esetben a küszöbértékét 100 van beállítva, ha azt fogja méretezni 200 esetén a várakozási sorban található üzenetek száma. Ha például 100 üzenetek példányonként.

Beállíthatja az Azure-portálon kattintson a **Beállítások** lap: az. Virtuális skála készletek frissítheti az Automatikus méretezéssel beállítás a ARM sablonban *metricName* *ApproximateMessageCount* és adják át a tárhely várólista azonosítója, *metricResourceUri*.

Ha például klasszikus tároló fiókkal az Automatikus méretezéssel beállítás metricTrigger tartalmazza:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

(Nem klasszikus) tároló fiókot a metricTrigger tartalmazza:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>A gyakran használt szolgáltatás Bus mérőszámok

Méretezheti szolgáltatás Bus várólista hossz a szolgáltatás Bus várakozási sorban található üzenetek számát. Szolgáltatás Bus várólista hossza egy speciális mérőszám, és a megadott küszöbértékét alkalmazott példányonként üzenetek számát. Ez azt jelenti, ha két esetben a küszöbértékét 100 van beállítva, ha azt fogja méretezni 200 esetén a várakozási sorban található üzenetek száma. Ha például 100 üzenetek példányonként.

Virtuális skála készletek frissítheti az Automatikus méretezéssel beállítás a ARM sablonban *metricName* *ApproximateMessageCount* és adják át a tárhely várólista azonosítója, *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] A szolgáltatás Bus az erőforrás csoport fogalmat nem létezik, de Azure erőforrás-kezelő létrehoz egy alapértelmezett erőforráscsoport / régió. Az erőforráscsoport az általában az "Alapértelmezett - ServiceBus-[Körzet]" formátumot. Ha például "Alapértelmezett ServiceBus, EastUS" "Alapértelmezett ServiceBus, WestUS", "Alapértelmezett-ServiceBus-AustraliaEast" stb.
