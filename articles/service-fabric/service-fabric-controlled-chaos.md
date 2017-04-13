<properties
   pageTitle="Chaos jutva szolgáltatás háló fürt |} Microsoft Azure"
   description="Chaos kezelése a fürt van és a fürt Analysis szolgáltatás API-k használata"
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>A szolgáltatás háló fürt ellenőrzött Chaos jutva
Nagyméretű elosztott rendszerek, például a felhőben infrastruktúra, eleve mellékletei nem megbízható. Azure Service háló lehetővé teszi, hogy a fejlesztők számára, hogy megbízható szolgáltatások fölött mellékletei nem megbízható infrastruktúrát írni. Robusztus szolgáltatások írása, a fejlesztők kell tudja jutva hibák ilyen mellékletei nem megbízható infrastruktúra kipróbálni a szolgáltatást a stabilitás szemben.

A van és fürt Analysis (más néven a hibafa Analysis) szolgáltatást lehetőséget nyújt a fejlesztők a jutva szolgáltatások tesztelése hibafa műveletek. Azonban célzott szimulált hibák segítenek a csak az eddigi. A tesztelés további meghozatala, Chaos is használhatja.

Chaos keltő szűrő megőrzi a folyamatos, időosztásos hibák (biztonságos és ungraceful) a fürt egész hosszabb idő alatt. Miután beállította a Chaos a kamatláb és milyen hibákat, indítsa el, vagy állítsa le a C# API-hoz vagy a PowerShell hibák a fürt és a szolgáltatást az szeretne keresztül.

Chaos futása közben rögzítése a Futtatás pillanatában állapotának különböző eseményeket hoz létre. Például egy ExecutingFaultsEvent tartalmazza az adott közelítését végrehajtott minden hibáit. Egy ValidationFailedEvent tartalmazza, amelyek található fürt ellenőrzés során hiba részleteit. A GetChaosReportAsync API-t a jelentés Chaos futtatja az első is hivatkozhat.

## <a name="faults-induced-in-chaos"></a>A Chaos okozta hibák
Chaos hoz létre a hibák között a teljes szolgáltatás háló fürt, és tömöríti a hibák, a hónapok vagy évek számának meghatározása be néhány órát láthatók. A magas hibafa sebesség időosztásos hibák kombinációjának egyébként is kihagyott sarok esetek találja meg. A szolgáltatás kód minőségének jelentős javítását a Chaos gyakorlat vezet.

Chaos kapott hibák, a következő kategóriákból:

 - Indítsa újra a csomópontot.
 - Indítsa újra a telepített kód csomag
 - Kópia eltávolítása
 - Indítsa újra a kópia
 - Ugrás egy elsődleges kópiát (konfigurálható)
 - A másodlagos kópia (konfigurálható) áthelyezése

Több közelítések Chaos fut. Minden alkalommal, ahol a hibák és fürt érvényesítése az adott időszakra vonatkozóan. Beállíthatja, hogy a fürt stabilizálásához és az érvényességi sikeres töltött időt. Ha fürt érvényesítési hibát talál, Chaos hoz létre, és továbbra is fennáll a az Egyezményes időbélyeg és a hiba adatait egy ValidationFailedEvent.

Például érdemes megvizsgálni, amely egy órával legfeljebb három egyidejű hibák futtatja Chaos egy példánya. Chaos három hibák kapott, és majd ellenőrzi a fürt állapota. Közelítő módszert használ, és az előző lépést mindaddig, amíg kifejezetten az StopChaosAsync API-k leállítva, vagy ha egy óra továbbítja. Ha a fürt bármely közelítését a sérült (Ez azt jelenti, akkor nem stabilizálásához egy megadott idő alatt) Chaos létrehoz egy ValidationFailedEvent. Ebben az esetben azt jelzi, hogy valamit, amit nem megfelelő leállt szükséges lehet a további vizsgálat.

A jelenlegi formájukban Chaos kapott csak megbízható hibáit. Ez azt jelenti, hogy külső hibák hiányában a kvórumvesztést vagy az adatok elvesztését soha ne jöhessen létre.

## <a name="important-configuration-options"></a>Fontos beállítási lehetőségek
 - **TimeToRun**: teljes időt, hogy Chaos futtatása előtt sikerrel fejeződjön. Az StopChaos API-k futtatásához van TimeToRun időszakra, akkor leállíthatja Chaos.
 - **MaxClusterStabilizationTimeout**: ideig várjon a fürt előtt ellenőrzése rajta megfelelő válik a legnagyobb mennyiségű. A Várakozás a csökkentse a fürt terhelését, miközben a helyreállítás, akkor is. Az elvégzett a következők:
    - Az OK gombra a fürt állapot esetén
    - Ha, hogy a szolgáltatás állapota
    - Ha a cél kópiakészlet méret érhető el a szolgáltatás partíciót
    - Nincs InBuild kópiák meglétének
 - **MaxConcurrentFaults**: az egyes példányaiban vannak okozta egyidejű hibák maximális számát. A nagyobb az érték, annál olyan szigorú Chaos van. Ennek eredményeképpen összetettebb feladatátadás és áttűnés kombinációi. Chaos garantálja, hogy külső hibák hiányában nem tartozik kvórumvesztést vagy az adatok elvesztését, függetlenül attól, hogy milyen magasan érték ebben a konfigurációban rendelkezik.
 - **EnableMoveReplicaFaults**: engedélyezi vagy tiltja a áthelyezése az elsődleges és másodlagos kópiák okozó hibák. E hibák alapértelmezés szerint le vannak tiltva.
 - **WaitTimeBetweenIterations**: közötti iterációk száma, ez azt jelenti, hogy a hibák és a megfelelő érvényességi kerekítés után várakozási idő mennyiségét.
 - **WaitTimeBetweenFaults**: ennyi ideig várjon egy közelítését a két egymást követő hibák között.

## <a name="how-to-run-chaos"></a>Hogyan tehető függővé Chaos
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**A PowerShell:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
