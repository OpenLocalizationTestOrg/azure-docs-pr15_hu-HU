<properties
    pageTitle="Azure Monitor mértékek - erőforrás típusonként támogatott mértékek |} Microsoft Azure"
    description="A mértékek érhető el az egyes erőforrások: Azure monitorral rendelkező listája."
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

# <a name="supported-metrics-with-azure-monitor"></a>Azure monitorral rendelkező támogatott mérőszámok

Azure Monitor többféleképpen is együttműködhet a mértékek, beleértve a portálon Diagramkészítés őket, elérése őket az REST API-k és lekérdezése őket PowerShell alrendszerrel vagy CLI. Az alábbi képen jelenleg elérhető az Azure Monitor metrikus folyamat minden mérőszám listáját.

> [AZURE.NOTE] Lehet, hogy más mértékek érhető el a portálon vagy régebbi API-k használata. A lista csak a nyilvános előzetes mértékek használata az összesített Azure Monitor metrikus folyamat nyilvános mintája elérhető tartalmazza.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|CoreCount|Alapvető száma|Darabszám|Összesen|A köteg fiókban magmintákat száma|
|TotalNodeCount|Csomópont száma|Darabszám|Összesen|A köteg fiók található csomópontok száma|
|CreatingNodeCount|Csomópont darab létrehozása|Darabszám|Összesen|A létrehozandó csomópontok számának|
|StartingNodeCount|Kezdő csomópont száma|Darabszám|Összesen|A csomópontok kezdési száma|
|WaitingForStartTaskNodeCount|Várakozás kezdő feladatok csomópont száma|Darabszám|Összesen|Várakozás a feladat elvégzéséhez indítása csomópontok számának|
|StartTaskFailedNodeCount|Tevékenység kezdési nem sikerült csomópont száma|Darabszám|Összesen|Ha a feladat indítása sikertelen volt csomópontok számának|
|IdleNodeCount|Üresjárati csomópont száma|Darabszám|Összesen|Üresjárati csomópontok számának|
|OfflineNodeCount|Kapcsolat nélküli csomópont száma|Darabszám|Összesen|Kapcsolat nélküli csomópontok számának|
|RebootingNodeCount|Csomópont darab újraindítása|Darabszám|Összesen|A csomópontok újraindítása száma|
|ReimagingNodeCount|Reimaging csomópont száma|Darabszám|Összesen|Reimaging csomópontok számának|
|RunningNodeCount|A futó csomópont száma|Darabszám|Összesen|A csomópontok futó száma|
|LeavingPoolNodeCount|Hangpostaüzenetek készlet csomópont száma|Darabszám|Összesen|Hagyja meg a készlet csomópontok számának|
|UnusableNodeCount|Használhatatlan csomópont száma|Darabszám|Összesen|Használhatatlan csomópontok számának|
|TaskStartEvent|Tevékenység kezdési események|Darabszám|Összesen|Tevékenységek által indított van száma|
|TaskCompleteEvent|Tevékenység készültségi események|Darabszám|Összesen|Befejezett tevékenységek száma|
|TaskFailEvent|Tevékenység Fail események|Darabszám|Összesen|Hibás állapotban a befejezett tevékenységek száma|
|PoolCreateEvent|Erőforráskészlet létrehozása események|Darabszám|Összesen|Létrehozott készletek száma|
|PoolResizeStartEvent|Készlet átméretezési kezdő események|Darabszám|Összesen|Készlet átméretezi által indított van száma|
|PoolResizeCompleteEvent|Készlet átméretezési teljes események|Darabszám|Összesen|Készlet átméretezi végrehajtott száma|
|PoolDeleteStartEvent|Készlet törlés kezdő események|Darabszám|Összesen|Teljes számát, hogy van-e lépések készlet törlése|
|PoolDeleteCompleteEvent|Készlet törlés teljes események|Darabszám|Összesen|Készlet törlése végrehajtott száma|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|connectedclients|Kapcsolódó ügyfelek|Darabszám|Maximum||
|totalcommandsprocessed|Összes művelet|Darabszám|Összesen||
|cachehits|Gyorsítótár-találatok|Darabszám|Összesen||
|cachemisses|Gyorsítótár mért sikertelen találatok|Darabszám|Összesen||
|getcommands|Kap|Darabszám|Összesen||
|setcommands|Beállítása|Darabszám|Összesen||
|evictedkeys|Kizárt kulcsok|Darabszám|Összesen||
|totalkeys|Teljes kulcsok|Darabszám|Maximum||
|expiredkeys|Lejárt kulcsok|Darabszám|Összesen||
|usedmemory|Használt memória|Bájt|Maximum||
|usedmemoryRss|Használt memória RSS|Bájt|Maximum||
|serverLoad|Kiszolgáló betöltése|Százalék|Maximum||
|cacheWrite|Gyorsítótár írási|BytesPerSecond|Maximum||
|cacheRead|Gyorsítótár olvasása|BytesPerSecond|Maximum||
|percentProcessorTime|PROCESSZOR|Százalék|Maximum||
|connectedclients0|Kapcsolódó ügyfeleket (Shard 0)|Darabszám|Maximum||
|totalcommandsprocessed0|Teljes műveletek (Shard 0)|Darabszám|Összesen||
|cachehits0|Gyorsítótár-találatok (Shard 0)|Darabszám|Összesen||
|cachemisses0|Gyorsítótár mért sikertelen találatok (Shard 0)|Darabszám|Összesen||
|getcommands0|Kap (Shard 0)|Darabszám|Összesen||
|setcommands0|Kifejezéskészletek (Shard 0)|Darabszám|Összesen||
|evictedkeys0|Kizárt kulcsok (Shard 0)|Darabszám|Összesen||
|totalkeys0|Teljes billentyűk (csomópont 0)|Darabszám|Maximum||
|expiredkeys0|Lejárt kulcsok (Shard 0)|Darabszám|Összesen||
|usedmemory0|Használt memória (Shard 0)|Bájt|Maximum||
|usedmemoryRss0|Használt memória RSS (Shard 0)|Bájt|Maximum||
|serverLoad0|Kiszolgáló terhelés (Shard 0)|Százalék|Maximum||
|cacheWrite0|Gyorsítótár írási (Shard 0)|BytesPerSecond|Maximum||
|cacheRead0|Gyorsítótár olvasható (Shard 0)|BytesPerSecond|Maximum||
|percentProcessorTime0|Kitöltött Processzor (Shard 0)|Százalék|Maximum||
|connectedclients1|Kapcsolódó ügyfeleket (Shard 1)|Darabszám|Maximum||
|totalcommandsprocessed1|Teljes műveletek (Shard 1)|Darabszám|Összesen||
|cachehits1|Gyorsítótár-találatok (Shard 1)|Darabszám|Összesen||
|cachemisses1|Gyorsítótár mért sikertelen találatok (Shard 1)|Darabszám|Összesen||
|getcommands1|Kap (Shard 1)|Darabszám|Összesen||
|setcommands1|Kifejezéskészletek (Shard 1)|Darabszám|Összesen||
|evictedkeys1|Kizárt kulcsok (Shard 1)|Darabszám|Összesen||
|totalkeys1|Teljes billentyűk (csomópont-1)|Darabszám|Maximum||
|expiredkeys1|Lejárt kulcsok (Shard 1)|Darabszám|Összesen||
|usedmemory1|Használt memória (Shard 1)|Bájt|Maximum||
|usedmemoryRss1|Használt memória RSS (Shard 1)|Bájt|Maximum||
|serverLoad1|Kiszolgáló terhelés (Shard 1)|Százalék|Maximum||
|cacheWrite1|Gyorsítótár írási (Shard 1)|BytesPerSecond|Maximum||
|cacheRead1|Gyorsítótár olvasható (Shard 1)|BytesPerSecond|Maximum||
|percentProcessorTime1|Kitöltött Processzor (Shard 1)|Százalék|Maximum||
|connectedclients2|Kapcsolódó ügyfeleket (Shard 2)|Darabszám|Maximum||
|totalcommandsprocessed2|Teljes műveletek (Shard 2)|Darabszám|Összesen||
|cachehits2|Gyorsítótár-találatok (Shard 2)|Darabszám|Összesen||
|cachemisses2|Gyorsítótár mért sikertelen találatok (Shard 2)|Darabszám|Összesen||
|getcommands2|Kap (Shard 2)|Darabszám|Összesen||
|setcommands2|Kifejezéskészletek (Shard 2)|Darabszám|Összesen||
|evictedkeys2|Kizárt kulcsok (Shard 2)|Darabszám|Összesen||
|totalkeys2|Teljes billentyűk (csomópont 2)|Darabszám|Maximum||
|expiredkeys2|Lejárt kulcsok (Shard 2)|Darabszám|Összesen||
|usedmemory2|Használt memória (Shard 2)|Bájt|Maximum||
|usedmemoryRss2|Használt memória RSS (Shard 2)|Bájt|Maximum||
|serverLoad2|Kiszolgáló terhelés (Shard 2)|Százalék|Maximum||
|cacheWrite2|Gyorsítótár írási (Shard 2)|BytesPerSecond|Maximum||
|cacheRead2|Gyorsítótár olvasható (Shard 2)|BytesPerSecond|Maximum||
|percentProcessorTime2|Kitöltött Processzor (Shard 2)|Százalék|Maximum||
|connectedclients3|Kapcsolódó ügyfeleket (Shard 3)|Darabszám|Maximum||
|totalcommandsprocessed3|Teljes műveletek (Shard 3)|Darabszám|Összesen||
|cachehits3|Gyorsítótár-találatok (Shard 3)|Darabszám|Összesen||
|cachemisses3|Gyorsítótár mért sikertelen találatok (Shard 3)|Darabszám|Összesen||
|getcommands3|Kap (Shard 3)|Darabszám|Összesen||
|setcommands3|Kifejezéskészletek (Shard 3)|Darabszám|Összesen||
|evictedkeys3|Kizárt kulcsok (Shard 3)|Darabszám|Összesen||
|totalkeys3|Teljes billentyűk (csomópont 3)|Darabszám|Maximum||
|expiredkeys3|Lejárt kulcsok (Shard 3)|Darabszám|Összesen||
|usedmemory3|Használt memória (Shard 3)|Bájt|Maximum||
|usedmemoryRss3|Használt memória RSS (Shard 3)|Bájt|Maximum||
|serverLoad3|Kiszolgáló terhelés (Shard 3)|Százalék|Maximum||
|cacheWrite3|Gyorsítótár írási (Shard 3)|BytesPerSecond|Maximum||
|cacheRead3|Gyorsítótár olvasható (Shard 3)|BytesPerSecond|Maximum||
|percentProcessorTime3|Kitöltött Processzor (Shard 3)|Százalék|Maximum||
|connectedclients4|Kapcsolódó ügyfeleket (Shard 4)|Darabszám|Maximum||
|totalcommandsprocessed4|Teljes műveletek (Shard 4)|Darabszám|Összesen||
|cachehits4|Gyorsítótár-találatok (Shard 4)|Darabszám|Összesen||
|cachemisses4|Gyorsítótár mért sikertelen találatok (Shard 4)|Darabszám|Összesen||
|getcommands4|Kap (Shard 4)|Darabszám|Összesen||
|setcommands4|Kifejezéskészletek (Shard 4)|Darabszám|Összesen||
|evictedkeys4|Kizárt kulcsok (Shard 4)|Darabszám|Összesen||
|totalkeys4|Teljes billentyűk (csomópont 4)|Darabszám|Maximum||
|expiredkeys4|Lejárt kulcsok (Shard 4)|Darabszám|Összesen||
|usedmemory4|Használt memória (Shard 4)|Bájt|Maximum||
|usedmemoryRss4|Használt memória RSS (Shard 4)|Bájt|Maximum||
|serverLoad4|Kiszolgáló terhelés (Shard 4)|Százalék|Maximum||
|cacheWrite4|Gyorsítótár írási (Shard 4)|BytesPerSecond|Maximum||
|cacheRead4|Gyorsítótár olvasható (Shard 4)|BytesPerSecond|Maximum||
|percentProcessorTime4|Kitöltött Processzor (Shard 4)|Százalék|Maximum||
|connectedclients5|Kapcsolódó ügyfeleket (Shard 5)|Darabszám|Maximum||
|totalcommandsprocessed5|Teljes műveletek (Shard 5)|Darabszám|Összesen||
|cachehits5|Gyorsítótár-találatok (Shard 5)|Darabszám|Összesen||
|cachemisses5|Gyorsítótár mért sikertelen találatok (Shard 5)|Darabszám|Összesen||
|getcommands5|Kap (Shard 5)|Darabszám|Összesen||
|setcommands5|Kifejezéskészletek (Shard 5)|Darabszám|Összesen||
|evictedkeys5|Kizárt kulcsok (Shard 5)|Darabszám|Összesen||
|totalkeys5|Teljes billentyűk (csomópont 5)|Darabszám|Maximum||
|expiredkeys5|Lejárt kulcsok (Shard 5)|Darabszám|Összesen||
|usedmemory5|Használt memória (Shard 5)|Bájt|Maximum||
|usedmemoryRss5|Használt memória RSS (Shard 5)|Bájt|Maximum||
|serverLoad5|Kiszolgáló terhelés (Shard 5)|Százalék|Maximum||
|cacheWrite5|Gyorsítótár írási (Shard 5)|BytesPerSecond|Maximum||
|cacheRead5|Gyorsítótár olvasható (Shard 5)|BytesPerSecond|Maximum||
|percentProcessorTime5|Kitöltött Processzor (Shard 5)|Százalék|Maximum||
|connectedclients6|Kapcsolódó ügyfeleket (Shard 6)|Darabszám|Maximum||
|totalcommandsprocessed6|Teljes műveletek (Shard 6)|Darabszám|Összesen||
|cachehits6|Gyorsítótár-találatok (Shard 6)|Darabszám|Összesen||
|cachemisses6|Gyorsítótár mért sikertelen találatok (Shard 6)|Darabszám|Összesen||
|getcommands6|Kap (Shard 6)|Darabszám|Összesen||
|setcommands6|Kifejezéskészletek (Shard 6)|Darabszám|Összesen||
|evictedkeys6|Kizárt kulcsok (Shard 6)|Darabszám|Összesen||
|totalkeys6|Teljes billentyűk (csomópont 6)|Darabszám|Maximum||
|expiredkeys6|Lejárt kulcsok (Shard 6)|Darabszám|Összesen||
|usedmemory6|Használt memória (Shard 6)|Bájt|Maximum||
|usedmemoryRss6|Használt memória RSS (Shard 6)|Bájt|Maximum||
|serverLoad6|Kiszolgáló terhelés (Shard 6)|Százalék|Maximum||
|cacheWrite6|Gyorsítótár írási (Shard 6)|BytesPerSecond|Maximum||
|cacheRead6|Gyorsítótár olvasható (Shard 6)|BytesPerSecond|Maximum||
|percentProcessorTime6|Kitöltött Processzor (Shard 6)|Százalék|Maximum||
|connectedclients7|Kapcsolódó ügyfeleket (Shard 7)|Darabszám|Maximum||
|totalcommandsprocessed7|Teljes műveletek (Shard 7)|Darabszám|Összesen||
|cachehits7|Gyorsítótár-találatok (Shard 7)|Darabszám|Összesen||
|cachemisses7|Gyorsítótár mért sikertelen találatok (Shard 7)|Darabszám|Összesen||
|getcommands7|Kap (Shard 7)|Darabszám|Összesen||
|setcommands7|Kifejezéskészletek (Shard 7)|Darabszám|Összesen||
|evictedkeys7|Kizárt kulcsok (Shard 7)|Darabszám|Összesen||
|totalkeys7|Teljes billentyűk (csomópont 7)|Darabszám|Maximum||
|expiredkeys7|Lejárt kulcsok (Shard 7)|Darabszám|Összesen||
|usedmemory7|Használt memória (Shard 7)|Bájt|Maximum||
|usedmemoryRss7|Használt memória RSS (Shard 7)|Bájt|Maximum||
|serverLoad7|Kiszolgáló terhelés (Shard 7)|Százalék|Maximum||
|cacheWrite7|Gyorsítótár írási (Shard 7)|BytesPerSecond|Maximum||
|cacheRead7|Gyorsítótár olvasható (Shard 7)|BytesPerSecond|Maximum||
|percentProcessorTime7|Kitöltött Processzor (Shard 7)|Százalék|Maximum||
|connectedclients8|Kapcsolódó ügyfeleket (Shard 8)|Darabszám|Maximum||
|totalcommandsprocessed8|Teljes műveletek (Shard 8)|Darabszám|Összesen||
|cachehits8|Gyorsítótár-találatok (Shard 8)|Darabszám|Összesen||
|cachemisses8|Gyorsítótár mért sikertelen találatok (Shard 8)|Darabszám|Összesen||
|getcommands8|Kap (Shard 8)|Darabszám|Összesen||
|setcommands8|Kifejezéskészletek (Shard 8)|Darabszám|Összesen||
|evictedkeys8|Kizárt kulcsok (Shard 8)|Darabszám|Összesen||
|totalkeys8|Teljes billentyűk (csomópont-8)|Darabszám|Maximum||
|expiredkeys8|Lejárt kulcsok (Shard 8)|Darabszám|Összesen||
|usedmemory8|Használt memória (Shard 8)|Bájt|Maximum||
|usedmemoryRss8|Használt memória RSS (Shard 8)|Bájt|Maximum||
|serverLoad8|Kiszolgáló terhelés (Shard 8)|Százalék|Maximum||
|cacheWrite8|Gyorsítótár írási (Shard 8)|BytesPerSecond|Maximum||
|cacheRead8|Gyorsítótár olvasható (Shard 8)|BytesPerSecond|Maximum||
|percentProcessorTime8|Kitöltött Processzor (Shard 8)|Százalék|Maximum||
|connectedclients9|Kapcsolódó ügyfeleket (Shard 9)|Darabszám|Maximum||
|totalcommandsprocessed9|Teljes műveletek (Shard 9)|Darabszám|Összesen||
|cachehits9|Gyorsítótár-találatok (Shard 9)|Darabszám|Összesen||
|cachemisses9|Gyorsítótár mért sikertelen találatok (Shard 9)|Darabszám|Összesen||
|getcommands9|Kap (Shard 9)|Darabszám|Összesen||
|setcommands9|Kifejezéskészletek (Shard 9)|Darabszám|Összesen||
|evictedkeys9|Kizárt kulcsok (Shard 9)|Darabszám|Összesen||
|totalkeys9|Teljes billentyűk (csomópont 9)|Darabszám|Maximum||
|expiredkeys9|Lejárt kulcsok (Shard 9)|Darabszám|Összesen||
|usedmemory9|Használt memória (Shard 9)|Bájt|Maximum||
|usedmemoryRss9|Használt memória RSS (Shard 9)|Bájt|Maximum||
|serverLoad9|Kiszolgáló terhelés (Shard 9)|Százalék|Maximum||
|cacheWrite9|Gyorsítótár írási (Shard 9)|BytesPerSecond|Maximum||
|cacheRead9|Gyorsítótár olvasható (Shard 9)|BytesPerSecond|Maximum||
|percentProcessorTime9|Kitöltött Processzor (Shard 9)|Százalék|Maximum||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|NumberOfCalls|API-hívások száma|Darabszám|Összesen|API-hívások száma.|
|NumberOfFailedCalls|Nem sikerült API-hívások száma|Darabszám|Összesen|Nem sikerült API-hívások száma.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|Százalékos Processzor|Százalékos Processzor|Százalék|Átlag|Százalékos jelenleg használja-e a virtuális gép(ek) számítási hozzárendelt erőforrás-mennyiség.|
|A hálózati|A hálózati|Bájt|Összesen|A hálózati adaptereken megkapta a virtuális gép(ek) (bejövő forgalom) bájtok számát|
|Hálózati meg|Hálózati meg|Bájt|Összesen|Minden hálózati kapcsolaton, virtuális gép(ek) (kimenő forgalmának) által elvégzett bájtok számát|
|Lemezen olvasható bájt|Lemezen olvasható bájt|Bájt|Összesen|Az összes bájt lemezről olvassa el az időszak ellenőrzése során|
|Lemezen írási bájt|Lemezen írási bájt|Bájt|Összesen|Az összes bájt időszak ellenőrzése során lemezre írása|
|További műveletek/Sec lemez|További műveletek/Sec lemez|CountPerSecond|Átlag|Lemezen olvasható IOPS|
|Lemezen írási műveletek/Sec|Lemezen írási műveletek/Sec|CountPerSecond|Átlag|Lemezen írási IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|Százalékos Processzor|Százalékos Processzor|Százalék|Átlag|Százalékos jelenleg használja-e a virtuális gép(ek) számítási hozzárendelt erőforrás-mennyiség.|
|A hálózati|A hálózati|Bájt|Összesen|A hálózati adaptereken megkapta a virtuális gép(ek) (bejövő forgalom) bájtok számát|
|Hálózati meg|Hálózati meg|Bájt|Összesen|Minden hálózati kapcsolaton, virtuális gép(ek) (kimenő forgalmának) által elvégzett bájtok számát|
|Lemezen olvasható bájt|Lemezen olvasható bájt|Bájt|Összesen|Az összes bájt lemezről olvassa el az időszak ellenőrzése során|
|Lemezen írási bájt|Lemezen írási bájt|Bájt|Összesen|Az összes bájt időszak ellenőrzése során lemezre írása|
|További műveletek/Sec lemez|További műveletek/Sec lemez|CountPerSecond|Átlag|Lemezen olvasható IOPS|
|Lemezen írási műveletek/Sec|Lemezen írási műveletek/Sec|CountPerSecond|Átlag|Lemezen írási IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|Százalékos Processzor|Százalékos Processzor|Százalék|Átlag|Százalékos jelenleg használja-e a virtuális gép(ek) számítási hozzárendelt erőforrás-mennyiség.|
|A hálózati|A hálózati|Bájt|Összesen|A hálózati adaptereken megkapta a virtuális gép(ek) (bejövő forgalom) bájtok számát|
|Hálózati meg|Hálózati meg|Bájt|Összesen|Minden hálózati kapcsolaton, virtuális gép(ek) (kimenő forgalmának) által elvégzett bájtok számát|
|Lemezen olvasható bájt|Lemezen olvasható bájt|Bájt|Összesen|Az összes bájt lemezről olvassa el az időszak ellenőrzése során|
|Lemezen írási bájt|Lemezen írási bájt|Bájt|Összesen|Az összes bájt időszak ellenőrzése során lemezre írása|
|További műveletek/Sec lemez|További műveletek/Sec lemez|CountPerSecond|Átlag|Lemezen olvasható IOPS|
|Lemezen írási műveletek/Sec|Lemezen írási műveletek/Sec|CountPerSecond|Átlag|Lemezen írási IOPS|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetriai üzenet küldése kísérletek|Darabszám|Összesen|El lehet küldeni a IoT központi a próbált eszköz cloud telemetriai üzenetek száma|
|d2c.telemetry.ingress.SUCCESS|Telemetriai üzenetek|Darabszám|Összesen|Eszköz a IoT központi sikeresen küldött a felhőben telemetriai üzenetek száma|
|c2d.commands.egress.Complete.SUCCESS|Befejezett parancsok|Darabszám|Összesen|Az eszköz sikeresen eszköz parancsok felhő száma|
|c2d.commands.egress.Abandon.SUCCESS|Félbehagyott parancsok|Darabszám|Összesen|Felhőalapú száma szerint az eszközön hagyva eszköz parancsok|
|c2d.commands.egress.Reject.SUCCESS|Elutasították parancsok|Darabszám|Összesen|Az eszköz elutasítva eszköz parancsok felhő száma|
|devices.totalDevices|Teljes eszközök|Darabszám|Összesen|A IoT hubhoz regisztrált eszközök száma|
|devices.connectedDevices.allProtocol|Csatlakoztatott eszközök|Darabszám|Összesen|A IoT elosztóhoz csatlakoztatott eszközök száma|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|INREQS|Bejövő felkérést|Darabszám|Összesen|Esemény központi bejövő üzenet átviteli névtér|
|SUCCREQ|A sikeres kérelmeket|Darabszám|Összesen|Teljes sikeres kérelmeket névtér|
|FAILREQ|Sikertelen kérések|Darabszám|Összesen|Nem sikerült kérések teljes névtér|
|SVRBSY|Kiszolgálói elfoglalt hibák|Darabszám|Összesen|Kiszolgáló teljes elfoglalt hibák névtér|
|INTERR|Belső kiszolgáló hibák|Darabszám|Összesen|Belső kiszolgáló teljes hibák névtér|
|MISCERR|Egyéb hibák|Darabszám|Összesen|Nem sikerült kérések teljes névtér|
|INMSGS|Bejövő üzenetek|Darabszám|Összesen|Névtér összes bejövő üzenetek|
|OUTMSGS|A kimenő üzenetek|Darabszám|Összesen|Kimenő üzenetek névtér összesen|
|EHINMBS|Bejövő bájtok / szekundum|BytesPerSecond|Összesen|Esemény központi bejövő üzenet átviteli névtér|
|EHOUTMBS|Kimenő bájtok / szekundum|BytesPerSecond|Összesen|Kimenő üzenetek névtér összesen|
|EHABL|Tartalék mailek archiválása|Darabszám|Összesen|Esemény központi archív üzenetek névtér tartalék|
|EHAMSGS|Mailek archiválása|Darabszám|Összesen|Esemény központi archivált névtér az üzenetek|
|EHAMBS|Archiválás üzenet átviteli|BytesPerSecond|Összesen|Esemény központi névtér az üzenet átviteli archiválása|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|RunsStarted|Lépések fut|Darabszám|Összesen|Lépések száma a munkafolyamat fut.|
|RunsCompleted|Befejezett fut|Darabszám|Összesen|Befejezett száma a munkafolyamat fut.|
|RunsSucceeded|Sikeres fut|Darabszám|Összesen|Sikeres száma a munkafolyamat fut.|
|RunsFailed|Nem sikerült fut|Darabszám|Összesen|Nem sikerült száma a munkafolyamat fut.|
|RunsCancelled|Lemondott fut|Darabszám|Összesen|A visszavont száma a munkafolyamat fut.|
|RunLatency|Késés futtatása|Másodperc|Átlag|Befejezett munkafolyamatok késés fut.|
|RunSuccessLatency|Futtassa a sikeres időtartama|Másodperc|Átlag|Késés sikeres a munkafolyamat fut.|
|RunThrottledEvents|Csökkentett események futtatása|Darabszám|Összesen|Munkafolyamat indítása műveletet, vagy az eseményindító szabályozott eseményeket.|
|RunFailurePercentage|Hiba százalékos futtatása|Százalék|Összesen|Nem sikerült százalékos aránya a munkafolyamat fut.|
|ActionsStarted|Műveletek indítása |Darabszám|Összesen|Munkafolyamat-műveletek lépések száma.|
|ActionsCompleted|Befejezett tevékenységek |Darabszám|Összesen|Munkafolyamat-műveletek befejeződött száma.|
|ActionsSucceeded|A művelet sikeresen befejeződött |Darabszám|Összesen|A munkafolyamat-műveletek száma sikeresen befejeződött.|
|ActionsFailed|Nem sikerült műveletek|Darabszám|Összesen|Sikertelen a munkafolyamat-műveletek száma.|
|ActionsSkipped|Kihagyott műveletek |Darabszám|Összesen|A munkafolyamat-műveletek száma hagyva.|
|ActionLatency|Művelet időtartama |Másodperc|Átlag|Befejezett munkafolyamat-műveletek késés.|
|ActionSuccessLatency|A művelet sikerült időtartama |Másodperc|Átlag|A sikeres munkafolyamat-műveletek késés.|
|ActionThrottledEvents|Művelet szabályozott események|Darabszám|Összesen|A munkafolyamat indítása műveletet száma szabályozott eseményeket.|
|TriggersStarted|Eseményindítók lépések |Darabszám|Összesen|A munkafolyamat indítók lépések szám.|
|TriggersCompleted|Eseményindítók befejezése |Darabszám|Összesen|Befejezett munkafolyamatok indítók száma.|
|TriggersSucceeded|Eseményindítók sikeres |Darabszám|Összesen|A munkafolyamat indítók szám sikeresen befejeződött.|
|TriggersFailed|Eseményindítók nem sikerült. |Darabszám|Összesen|Sikertelen a munkafolyamat indítók száma.|
|TriggersSkipped|Eseményindítók kihagyott|Darabszám|Összesen|A munkafolyamat indítók szám hagyva.|
|TriggersFired|Eseményindítók, akkor következik be |Darabszám|Összesen|A munkafolyamat indítók szám akkor következik be.|
|TriggerLatency|Eseményindító időtartama |Másodperc|Átlag|Befejezett munkafolyamatok eseményindítók késés.|
|TriggerFireLatency|Eseményindító Fire időtartama |Másodperc|Átlag|Az időtartam munkafolyamat indítók következik.|
|TriggerSuccessLatency|Eseményindító sikeres időtartama |Másodperc|Átlag|A sikeres munkafolyamat indítók késés.|
|TriggerThrottledEvents|Eseményindító szabályozott események|Darabszám|Összesen|A munkafolyamat eseményindító száma szabályozott eseményeket.|
|BillableActionExecutions|Számlázható művelet végrehajtások|Darabszám|Összesen|Munkafolyamat-művelet végrehajtások első számlát kapni száma.|
|BillableTriggerExecutions|Az eseményindító számlázható végrehajtások|Darabszám|Összesen|Az első számlát kapni a munkafolyamat eseményindító végrehajtások száma.|
|TotalBillableExecutions|Teljes számlázható végrehajtások|Darabszám|Összesen|Az első számlát kapni munkafolyamat végrehajtások száma.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|Átviteli|Átviteli|BytesPerSecond|Átlag||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|SearchLatency|Keresési időtartama|Másodperc|Átlag|A keresési szolgáltatás átlagos keresési időtartama|
|SearchQueriesPerSecond|A keresési lekérdezések / szekundum|CountPerSecond|Átlag|A keresési szolgáltatás másodpercenként a keresési lekérdezések|
|ThrottledSearchQueriesPercentage|Csökkentett keresési lekérdezések százalékos|Százalék|Átlag|Százalékos kereséseket, amelyek a keresési szolgáltatás szabályozott volt.|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|CPUXNS|Minden névtér processzorhasználata|Százalék|Maximum|Szolgáltatás bus prémium névtér Processzor használatát metrikus|
|WSXNS|Névtér egy memóriahasználat mérete|Százalék|Maximum|Szolgáltatás bus prémium névtér memória használatát metrikus|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|cpu_percent|Processzor százalékos|Százalék|Átlag|Processzor százalékos|
|physical_data_read_percent|Adatok IO százalékos|Százalék|Átlag|Adatok IO százalékos|
|log_write_percent|Log IO százalékos|Százalék|Átlag|Log IO százalékos|
|dtu_consumption_percent|DTU százalékos|Százalék|Átlag|DTU százalékos|
|tárhely|Teljes adatbázisméretet|Bájt|Maximum|Teljes adatbázisméretet|
|connection_successful|A sikeres kapcsolatok|Darabszám|Összesen|A sikeres kapcsolatok|
|connection_failed|Hibás kapcsolatok|Darabszám|Összesen|Hibás kapcsolatok|
|blocked_by_firewall|Tűzfal blokkolja|Darabszám|Összesen|Tűzfal blokkolja|
|kölcsönös kizárás|Kölcsönös kizárások|Darabszám|Összesen|Kölcsönös kizárások|
|storage_percent|Adatbázis méretének százalékos|Százalék|Maximum|Adatbázis méretének százalékos|
|xtp_storage_percent|A memóriában OLTP tároló percent(Preview)|Százalék|Átlag|A memóriában OLTP tároló percent(Preview)|
|workers_percent|Munkatársak százalékos|Százalék|Átlag|Munkatársak százalék|
|sessions_percent|Munkamenetek százalék|Százalék|Átlag|Munkamenetek százalék|
|dtu_limit|DTU korlát|Darabszám|Átlag|DTU korlát|
|dtu_used|Használt DTU|Darabszám|Átlag|Használt DTU|
|service_level_objective|Az adatbázis szolgáltatás szintű célja|Darabszám|Összesen|Az adatbázis szolgáltatás szintű célja|
|dwu_limit|dwu korlát|Darabszám|Maximum|dwu korlát|
|dwu_consumption_percent|DWU százalékos|Százalék|Átlag|DWU százalékos|
|dwu_used|Használt DWU|Darabszám|Átlag|Használt DWU|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|cpu_percent|Processzor százalékos|Százalék|Átlag|Processzor százalékos|
|physical_data_read_percent|Adatok IO százalékos|Százalék|Átlag|Adatok IO százalékos|
|log_write_percent|Log IO százalékos|Százalék|Átlag|Log IO százalékos|
|dtu_consumption_percent|DTU százalékos|Százalék|Átlag|DTU százalékos|
|storage_percent|Tárterület százalékos|Százalék|Átlag|Tárterület százalékos|
|workers_percent|Munkatársak százalék|Százalék|Átlag|Munkatársak százalék|
|sessions_percent|Munkamenetek százalék|Százalék|Átlag|Munkamenetek százalék|
|eDTU_limit|eDTU korlát|Darabszám|Átlag|eDTU korlát|
|storage_limit|Tárterületkorlátja|Bájt|Átlag|Tárterületkorlátja|
|eDTU_used|használt eDTU|Darabszám|Átlag|használt eDTU|
|storage_used|Használt tárterület|Bájt|Átlag|Használt tárterület|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|ResourceUtilization|Kihasználtsága SZU (%)|Százalék|Maximum|Kihasználtsága SZU (%)|
|InputEvents|Beviteli események|Darabszám|Összesen|Beviteli események|
|InputEventBytes|Beviteli esemény bájt|Bájt|Összesen|Beviteli esemény bájt|
|LateInputEvents|Késői beviteli események|Darabszám|Összesen|Késői beviteli események|
|OutputEvents|Kimeneti események|Darabszám|Összesen|Kimeneti események|
|ConversionErrors|Adatkonverziós hibák|Darabszám|Összesen|Adatkonverziós hibák|
|Hibák|Futási idejű hiba|Darabszám|Összesen|Futási idejű hiba|
|DroppedOrAdjustedEvents|Megfelelő sorrendben események|Darabszám|Összesen|Megfelelő sorrendben események|
|AMLCalloutRequests|Kérések függvény|Darabszám|Összesen|Kérések függvény|
|AMLCalloutFailedRequests|Hibás függvény kérések|Darabszám|Összesen|Hibás függvény kérések|
|AMLCalloutInputEvents|Események függvény|Darabszám|Összesen|Események függvény|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|CpuPercentage|Processzor százalékos|Százalék|Átlag|Processzor százalékos|
|MemoryPercentage|A memória százalékos|Százalék|Átlag|A memória százalékos|
|DiskQueueLength|Lemez hossza|Darabszám|Összesen|Lemez hossza|
|HttpQueueLength|HTTP várólista hossza|Darabszám|Összesen|HTTP várólista hossza|
|BytesReceived|Az adatok|Bájt|Összesen|Az adatok|
|BytesSent|Adatok meg|Bájt|Összesen|Adatok meg|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|CpuTime|Processzor idő|Másodperc|Összesen|Processzor idő|
|Kérések|Kérések|Darabszám|Összesen|Kérések|
|BytesReceived|Az adatok|Bájt|Összesen|Az adatok|
|BytesSent|Adatok meg|Bájt|Összesen|Adatok meg|
|Http2xx|HTTP 2xx|Darabszám|Összesen|HTTP 2xx|
|Http3xx|HTTP 3xx|Darabszám|Összesen|HTTP 3xx|
|Http401|HTTP 401|Darabszám|Összesen|HTTP 401|
|Http403|HTTP 403|Darabszám|Összesen|HTTP 403|
|Http404|HTTP 404|Darabszám|Összesen|HTTP 404|
|Http406|HTTP 406|Darabszám|Összesen|HTTP 406|
|Http4xx|HTTP 4xx|Darabszám|Összesen|HTTP 4xx|
|Http5xx|HTTP-kiszolgáló hibák|Darabszám|Összesen|HTTP-kiszolgáló hibák|
|MemoryWorkingSet|Memória-Munkakészlet|Bájt|Összesen|Memória-Munkakészlet|
|AverageMemoryWorkingSet|Átlagos memória-munkakészlet|Bájt|Átlag|Átlagos memória-munkakészlet|
|AverageResponseTime|Átlagos válaszidő|Másodperc|Átlag|Átlagos válaszidő|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrikus|Metrikus megjelenítendő név|Egység|Összesítési típusa|Leírás|
|---|---|---|---|---|
|CpuTime|Processzor idő|Másodperc|Összesen|Processzor idő|
|Kérések|Kérések|Darabszám|Összesen|Kérések|
|BytesReceived|Az adatok|Bájt|Összesen|Az adatok|
|BytesSent|Adatok meg|Bájt|Összesen|Adatok meg|
|Http2xx|HTTP 2xx|Darabszám|Összesen|HTTP 2xx|
|Http3xx|HTTP 3xx|Darabszám|Összesen|HTTP 3xx|
|Http401|HTTP 401|Darabszám|Összesen|HTTP 401|
|Http403|HTTP 403|Darabszám|Összesen|HTTP 403|
|Http404|HTTP 404|Darabszám|Összesen|HTTP 404|
|Http406|HTTP 406|Darabszám|Összesen|HTTP 406|
|Http4xx|HTTP 4xx|Darabszám|Összesen|HTTP 4xx|
|Http5xx|HTTP-kiszolgáló hibák|Darabszám|Összesen|HTTP-kiszolgáló hibák|
|MemoryWorkingSet|Memória-Munkakészlet|Bájt|Összesen|Memória-Munkakészlet|
|AverageMemoryWorkingSet|Átlagos memória-munkakészlet|Bájt|Átlag|Átlagos memória-munkakészlet|
|AverageResponseTime|Átlagos válaszidő|Másodperc|Átlag|Átlagos válaszidő|

## <a name="next-steps"></a>Következő lépések

- [További információ: az Azure Monitor mérőszámok](./monitoring-overview.md#monitoring-sources)
- [Értesítések létrehozása mérési módja miatt](./insights-receive-alert-notifications.md)
- [Tárhely, esemény központi vagy napló Analytics mértékek exportálása](./monitoring-overview-of-diagnostic-logs.md)
