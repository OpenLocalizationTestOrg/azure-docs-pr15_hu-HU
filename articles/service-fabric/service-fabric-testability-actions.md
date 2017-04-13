<properties
   pageTitle="Tesztelhetőségi művelet |} Microsoft Azure"
   description="Ez a cikk a Microsoft Azure Service háló található tesztelhetőségi műveleteket is bemutat."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Tesztelhetőségi műveletek
Annak érdekében, hogy egy mellékletei nem megbízható infrastruktúra hasonlóan, Azure Service háló biztosít, a Fejlesztőeszközök különböző valós életből hibák és állapot áttűnések hasonlóan lehetőséget. Ezek tesztelhetőségi műveletek vannak közzétéve. A műveletek a követő API-khoz egy adott van, állapot áttűnés vagy érvényességi okozó. Kombinálva ezeket a műveleteket, a szolgáltatások teljes Tesztelési forgatókönyvek írhat.

Szolgáltatás háló tartalmaz néhány gyakori próba forgatókönyvet az alábbi műveletek tevődik össze. Azt javasoljuk, hogy a beépített forgatókönyvekben, tesztelje a közös állam áttűnések és a hiba esetek gondosan kiválasztott csatlakozást. Műveletek azonban egyéni próba esetek létrehozása, ha fel szeretné venni a követés, amely nem szerepelnek a beépített forgatókönyvek még, illetve amelyeket egyéni az alkalmazás szabott felhasználási területei használható.

A műveletek rendszerbeli C# System.Fabric.dll összeállítás találhatók. A rendszer háló PowerShell-modult Microsoft.ServiceFabric.Powershell.dll összeállítás található. A ServiceFabric PowerShell-modult futtatókörnyezet telepítés részeként telepítve van a engedélyezni az egyszerű használat érdekében.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Biztonságos ungraceful hibafa műveletek és összehasonlítása
Tesztelhetőségi műveletek két fő időszakok sorolják:

* Ungraceful hibák: E hibák szimulálhatja hibákat, például a számítógép újraindítása és lefagy a folyamat. Ebben az esetben a hibák a folyamat végrehajtási környezetében váratlanul leáll. Ez azt jelenti, hogy nincs karbantartása állam futtatását is lehetővé teszi az alkalmazás indítása ismét előtt.

* Biztonságos hibák: E hibák biztonságos műveletek, például replika helyezi át, és a terheléselosztás által indított csepp hasonlóan. Ebben az esetben a szolgáltatás a Bezárás gombra a értesítést kap, és is karbantartása: az állapot bezárása előtt.

Jobb minőségű adatérvényesítéshez futtatása közben, hogy a különböző biztonságos és ungraceful hibák szolgáltatás és az üzleti terhelését. Ungraceful hibák gyakorolják forgatókönyvek, ahol a szolgáltatás folyamat váratlanul kilép néhány munkafolyamat közepén. Ez a helyreállítási elérési teszteli, amint a szolgáltatási replika helyreáll háló szolgáltatás által. Ez segít tesztelése adatok egységesebb, és azt, hogy a szolgáltatás állapota karbantartása megfelelően hiba után. A hibák (a biztonságos hibák) megadása tesztelése, hogy a szolgáltatás kópiákkal szolgáltatás háló helyezi át helyesen reagál. A RunAsync módszer ez teszteli a lemondás kezelése. A szolgáltatás a lemondás jogkivonat éppen megadásához megfelelően menteni az állapotába, valamint Kilépés a RunAsync módszer van szüksége.

## <a name="testability-actions-list"></a>Tesztelhetőségi műveletlista

| Művelet | Leírás | Felügyelt API | PowerShell-parancsmag | Biztonságos/ungraceful hibák |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| A próba-állam eltávolítja a fürt abban az esetben, ha a hibás leállítása a próba-illesztőprogram. | CleanTestStateAsync | Eltávolítás-ServiceFabricTestState | Nem alkalmazható |
| InvokeDataLoss | Az adatok elvesztését kapott be egy szolgáltatási partíciót. | InvokeDataLossAsync | Meghívása ServiceFabricPartitionDataLoss | Biztonságos |
| InvokeQuorumLoss | Egy adott állapot-nyilvántartó szolgáltatás partíciót helyezi kvórumvesztést be. | InvokeQuorumLossAsync | Meghívása ServiceFabricQuorumLoss | Biztonságos |
| Az elsődleges áthelyezése | Ugrás a megadott elsődleges replika állapot-nyilvántartó szolgáltatás megadott csomópont. | MovePrimaryAsync | Áthelyezés-ServiceFabricPrimaryReplica | Biztonságos |
| Ugrás másodlagos | Ugrás az aktuális másodlagos replika állapot-nyilvántartó szolgáltatás különböző csomópont. | MoveSecondaryAsync | Áthelyezés-ServiceFabricSecondaryReplica | Biztonságos |
| RemoveReplica | Keltő szűrő megőrzi replika hiba fürt kópia eltávolításával. Ez a replika bezárul, és fog átmenet szerepkör "Nincs", a fürt eltávolítása az összes állapotába. | RemoveReplicaAsync | Eltávolítás-ServiceFabricReplica | Biztonságos |
| RestartDeployedCodePackage | Kód csomag folyamat hiba szimulálja fürt csomóponton rendszerbe kód csomag újraindításával. Ez megszakítja a csomag fog indítsa újra az összes felhasználó szolgáltatás replikát üzemeltetett folyamat kód folyamat. | RestartDeployedCodePackageAsync | Indítsa újra-ServiceFabricDeployedCodePackage | Ungraceful |
| RestartNode | Keltő szűrő megőrzi szolgáltatás háló fürt csomópont hiba újraindításával csomópontot. | RestartNodeAsync | Indítsa újra-ServiceFabricNode | Ungraceful |
| RestartPartition | Keltő szűrő megőrzi adatközponthoz kiesési vagy fürt kiesési példa néhány vagy az összes partíciót replikái újraindításával. | RestartPartitionAsync | Indítsa újra-ServiceFabricPartition | Biztonságos |
| RestartReplica | Keltő szűrő megőrzi replika hiba állandó kópia újraindítását fürt, a replika bezárása és majd ismételt megnyitásával azt. | RestartReplicaAsync | Indítsa újra-ServiceFabricReplica | Biztonságos |
| Kiindulási_csomópont | Csomópont elindítja a fürtre, amely már leállt. | StartNodeAsync | Kezdés-ServiceFabricNode | Nem alkalmazható |
| StopNode | Csomópont hiba szimulálja leállítása egy fürt szerint. A csomópont lefelé maradnak mindaddig, amíg Kiindulási_csomópont nevezik. | StopNodeAsync | Leállítás-ServiceFabricNode | Ungraceful |
| ValidateApplication | Ellenőrzi rendelkezésre állásának és állapotának szolgáltatás háló szolgáltatások általában után, hogy bizonyos hibafa a rendszerbe alkalmazáson belül. | ValidateApplicationAsync | Próba-ServiceFabricApplication | Nem alkalmazható |
| ValidateService | Ellenőrzi a rendelkezésre állásának és állapotának szolgáltatás háló szolgáltatás, általában után, hogy bizonyos hibafa a rendszerbe. | ValidateServiceAsync | Próba-ServiceFabricService | Nem alkalmazható |

## <a name="running-a-testability-action-using-powershell"></a>A PowerShell használatá tesztelhetőségi művelet futtatása

Ebből az oktatóanyagból megtudhatja, hogyan tehető függővé egy tesztelhetőségi művelet PowerShell használatával. Megtanulhatja, hogyan tehető függővé egy tesztelhetőségi művelet szemben a helyi (1-mező) fürtre vagy egy Azure fürthöz. Microsoft.Fabric.Powershell.dll – a szolgáltatás háló PowerShell-modult – automatikusan települ a Microsoft-szolgáltatás háló MSI telepítésekor. A modul automatikusan betöltött egy PowerShell-parancssorában megnyitásakor.

Oktatóanyag szegmensek:

- [Egy beépített fürt művelet futtatása](#run-an-action-against-a-one-box-cluster)
- [Az Azure fürt művelet futtatása](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Egy beépített fürt művelet futtatása

Művelet való futtatásához a tesztelhetőségi szemben a helyi fürtre először a fürthöz csatlakozni, és nyissa meg a PowerShell-parancssorában rendszergazdai módban. Tudassa velünk tekintse meg a **Restart-ServiceFabricNode** műveletet.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

A művelet **Restart-ServiceFabricNode** itt fut. a "Node1" nevű csomópontot. A befejezési mód Itt adhatja meg, hogy azt nem ellenőrizze, hogy a restart-csomópont művelet valójában sikeres volt. Adja meg a befejezési mód, mint "Ellenőrzése" eredményezi, hogy ellenőrizze, hogy az újraindítás művelet valójában sikeres. Helyett közvetlenül meghatározása a csomópontra a nevét, megadhatja azt partíciót kulcs és replika, milyen keresztül az alábbi képlettel történik:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

Indítsa újra a szolgáltatás háló csomópont fürt használandó a **Restart-ServiceFabricNode** . Ezzel leállítja a Fabric.exe folyamat újraindítása összes a rendszer szolgáltatás és a felhasználói szolgáltatás replikák csomópontra is. Ez az API segítségével a szolgáltatás tesztelése segítséget nyújt a hibák a feladatátvevő helyreállítási elérési utak mentén Kihúzás. Könnyebben hasonlóan a fürt csomópont hibáit.

Az alábbi képernyőképen látható művelet a **Restart-ServiceFabricNode** tesztelhetőségi parancsot.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

Az első **Get-ServiceFabricNode** (a szolgáltatás háló PowerShell modulból parancsmag) eredményét jeleníti meg, hogy rendelkezik-e a helyi fürtre öt csomópontok: a Node.5 Node.1. A csomóponton Node.4, nevű (parancsmag) **Restart-ServiceFabricNode** tesztelhetőségi művelet végrehajtása után láthatja, hogy a csomópont üzemidőt vissza nem állítja.

### <a name="run-an-action-against-an-azure-cluster"></a>Az Azure fürt művelet futtatása

Egy tesztelhetőségi művelet futó (PowerShell használatával) egy Azure fürthöz hasonlít a művelet futtatása szemben a helyi fürtre. Az egyetlen különbség, hogy a művelet helyett a helyi fürthöz kapcsolódó futtatása előtt, még nem csatlakozott a Azure fürthöz először.

## <a name="running-a-testability-action-using-c35"></a>A C és a #35; használatával tesztelhetőségi művelet futtatása

Egy tesztelhetőségi művelet futtatása használatával C#, először meg kell csatlakoznia a fürthöz FabricClient használatával. Ezután szerezze be a paramétereket, a művelet futtatásához szükséges. Különböző paraméterek ugyanazon művelet futtatásához használható.
A RestartServiceFabricNode művelet megnézve egyik futtatáshoz módja a fürt (csomópont nevét és csomópont-példány azonosító) csomópontot információk segítségével.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Paraméter Magyarázat:

- **CompleteMode** Itt adhatja meg, hogy a mód nem ellenőrizze, hogy az újraindítás művelet valójában sikeres. Adja meg a befejezésétől mód, mint "Ellenőrzése" eredményezi, hogy ellenőrizze, hogy az újraindítás művelet valójában sikeres.  
- **Így időtúllépés történt** a művelet előtt egy TimeoutException kivétel befejezési idő mennyiségét állítja be.
- **CancellationToken** lehetővé teszi a folyamatban lévő hívás szeretne vonni.

Közvetlen megadása a csomópontra a nevével, hanem partíciót kulcs és replika típusú megadhatja azt.

További információért olvassa el a [PartitionSelector és ReplicaSelector](#partition_replica_selector)című témakört.


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector és ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector tesztelhetőségi elérhetővé segítő, és jelölje ki azt a tesztelhetőségi műveletek elvégzéséhez egy adott partíciót szolgál. Jelölje be az egy adott partíciót, ha a partíciót azonosító előzetesen ismert használható. Vagy megadhatja a partíciót billentyűt, és a művelet belső megoldja a partíciót Azonosítót. Akkor is, hogy véletlen partíciót kijelölése.

A segítő használatához hozza létre az PartitionSelector objektumot, és válassza ki a partíciót a Select * módszerek valamelyikével. A megkívánja API-hoz, majd át a PartitionSelector objektumban. Ha nincs lehetőség ki van jelölve, lesz az alapértelmezett véletlen partíciót.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector tesztelhetőségi elérhetővé segítő és segíti, jelölje be a csak a másolata található, amelyben a tesztelhetőségi műveletek elvégzéséhez. Jelölje be az egy adott kópiát, ha replika azonosítója előzetesen ismert használható. Ezenkívül ha az elsődleges kópia vagy véletlen másodlagos kijelölése választógombot. ReplicaSelector PartitionSelector, származik, így ki kell választania a és a partíciót a tesztelhetőségi végrehajtása ki szeretne is.

A segítő használni, hozzon létre egy ReplicaSelector objektumot, és állítsa be a a és a partíciót jelölje ki a kívánt módon. Ezután átviheti azt be az API működéséhez szükség van rá. Ha nincs lehetőség ki van jelölve, lesz az alapértelmezett egy véletlen replika és véletlen partíciót.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Következő lépések

- [Tesztelhetőségi felhasználási területei](service-fabric-testability-scenarios.md)
- A szolgáltatás tesztelése
   - [Hibák szimulálhatja szolgáltatás munkaterhelésekből során](service-fabric-testability-workload-tests.md)
   - [Szolgáltatás kommunikációs hibák](service-fabric-testability-scenarios-service-communication.md)
