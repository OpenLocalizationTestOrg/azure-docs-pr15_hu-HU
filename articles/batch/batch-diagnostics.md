<properties
   pageTitle="Azure köteg diagnosztikai naplózás |} Microsoft Azure"
   description="Rögzítheti és elemzése a diagnosztikai naplókban események Azure köteg fiók erőforrások, mint a készletek és a feladatokat."
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure köteg diagnosztikai naplózás

Sok Azure szolgáltatás, és a köteg szolgáltatás naplóbeli események az egyes erőforrások az erőforrás élettartama során bocsát ki. A diagnosztikai naplók Azure köteg rekord események az erőforrások, mint készletek és a tevékenységek engedélyezése, és alkalmazza a naplókat diagnosztikai értékelő és ellenőrzési. Eseményeket, például a készlet létrehozása, készlet törlés, tevékenység kezdési, a tevékenység készültségi és mások köteg a diagnosztikai naplók szerepelnek.

>[AZURE.NOTE] Ez a cikk azt ismerteti, hogy a köteg fiók erőforrások maguk eseményeknek, nem feladat, és a tevékenység kimeneti adatai. A feladatok és a feladatok kimeneti adatai tárolja a részletekért olvassa el [továbbra is fennáll Azure köteg feladatot, és a tevékenység kimeneti](batch-task-output.md).

## <a name="prerequisites"></a>Előfeltételek

* [Azure köteg-fiók](batch-account-create-portal.md)

* [Azure tárterület-fiók](../storage/storage-create-storage-account.md#create-a-storage-account)

  Továbbra is fennáll a diagnosztikai naplók köteg, létre kell hoznia az Azure hol tárolja a naplókat Azure tárterület-fiókkal. Megadhatja a tárterület-fiók esetén a [diagnosztikai naplózás engedélyezéséhez](#enable-diagnostic-logging) a köteg fiókjához. Tárterület fiók, adja meg, ha engedélyezi a napló a webhelycsoport még nem ugyanaz, mint egy [alkalmazáscsomagok](batch-application-packages.md) és a [tevékenység kimeneti adatmegőrzési](batch-task-output.md) cikkekben említett csatolt tárterület-fiókkal.

  >[AZURE.WARNING] Ön a Azure tárterület-fiókjában tárolt adatok **fel** . Ide tartoznak a diagnosztikai naplók, a jelen cikkben tárgyalt. Tartsa szem előtt a [napló adatmegőrzési szabály](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)tervezésekor.

## <a name="enable-diagnostic-logging"></a>Diagnosztikai naplózás engedélyezése

Diagnosztikai naplózás a köteg fiók alapértelmezés szerint nincs engedélyezve. Diagnosztikai naplózás az összes figyelni kívánt köteg-fiókhoz külön engedélyezni kell:

[Hogyan engedélyezhető a diagnosztikai naplók gyűjteménye](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Azt javasoljuk, hogy olvassa el a teljes [Azure a diagnosztikai naplók áttekintése](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) cikk nemcsak megértéséhez szükséges, hogyan ahhoz, hogy a naplózás, de a napló kategóriák a különböző Azure szolgáltatás által támogatott. Azure köteg például támogatja egy napló kategória: **Szolgáltatás naplók**.

## <a name="service-logs"></a>Szolgáltatás a naplók

Azure köteg szolgáltatás naplók eseményeket egy köteg erőforrás, például egy készlet vagy a tevékenység időtartama alatt a köteg Azure szolgáltatás által kibocsátott tartalmazzák. Egyes események köteg által kibocsátott a megadott tároló fiók JSON formátumban vannak tárolva. Például: minta **készlet létrehozása esemény**törzsében az:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Minden esemény törzsében található .json fájl a megadott Azure tárterület-fiókjában. Ha szeretne közvetlenül elérhető a naplókat, tekintse át a [tárterület-fiók a diagnosztikai naplók sémájának](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account)kívánt.

## <a name="service-log-events"></a>Szolgáltatás naplóbeli események

A köteg szolgáltatás jelenleg a következő szolgáltatás naplóbeli események bocsát ki. Ebben a listában valószínűleg nem teljes, további események előfordulhat, hogy hozzáadta a mivel ez a cikk legutolsó frissítés óta.

| **Szolgáltatás naplóbeli események** |
| ------------------ |
| [Erőforráskészlet létrehozása][pool_create] |
| [Készlet törlés indítása][pool_delete_start] |
| [Teljes készlet törlése][pool_delete_complete] |
| [Készlet átméretezés indítása][pool_resize_start] |
| [Teljes készlet átméretezése][pool_resize_complete] |
| [Tevékenység kezdési][task_start] |
| [A feladat befejezése][task_complete] |
| [Tevékenység fail:][task_fail] |

## <a name="next-steps"></a>Következő lépések

Nemcsak a diagnosztikai naplókban események tárolása Azure tárterület-fiókkal, is adatfolyam-szolgáltatás naplóesemények köteg [Azure esemény központi](../event-hubs/event-hubs-what-is-event-hubs.md), és küldje el nekik [Azure napló Analytics](../log-analytics/log-analytics-overview.md).

* [Adatfolyam-esemény hubok Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  Adatfolyam-köteg diagnosztikai események az adatok nagymértékben méretezhető bejövő adatok szolgáltatás esemény hubok. Esemény hubok is ingest másodpercenként, amely, átalakítás és tárolni a bármely valós idejű analytics-szolgáltató használata események milliónyi.

* [Log Analytics használatával Azure a diagnosztikai naplók elemzése](../log-analytics/log-analytics-azure-storage-json.md)

  A diagnosztikai naplók küldése napló Analytics, amelyen elemzése azokat a műveleteket Management csomagja (MOBILE) portálon, vagy exportálása után a Power BI vagy az Excelben az elemzéshez.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
