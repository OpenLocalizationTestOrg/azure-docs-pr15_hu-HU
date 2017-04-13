<properties
   pageTitle="Egyéni Tesztelési forgatókönyvek |} Microsoft Azure"
   description="Hogyan lehet a szolgáltatások biztonságos és ungraceful hibáit elleni védekezés megerősítésére."
   services="service-fabric"
   documentationCenter=".net"
   authors="anmolah"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="anmola"/>

# <a name="simulate-failures-during-service-workloads"></a>Hibák szimulálhatja szolgáltatás munkaterhelésekből során

Az tesztelhetőségi jelenik meg az Azure Service háló engedélyezze a fejlesztők számára, hogy az egyes hibák kezelésének nem aggódnia. Vannak helyzetek, akkor jó helyen jár, ahol explicit kihagyásos ügyfél terhelést és hibák szükség lehet. Az ügyfél terhelést és hibák kihagyásos biztosítja, hogy a szolgáltatás ténylegesen végrehajtásához néhány művelet hiba történik, amikor. A vezérlő tesztelhetőségi biztosítja szintjét adni, ezek lehetnek a terhelést végrehajtás pontos pontokon. Ez az alkalmazás különböző államokban a hibák indukciós hibák keresheti meg és minőségének.

## <a name="sample-custom-scenario"></a>Példa egyéni
A tesztet a vállalati terhelést a [biztonságos és ungraceful hibáit](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions)interleaves példa látható. A hibák lehet elérni, szolgáltatási műveletek vagy a legjobb eredményt számítási közepére.

Vegyük végigvezetik egy szolgáltatás közzététele négy munkaterhelésekből, például: A, B, C és a d minden munkafolyamatainak együttese felel meg, és számítási, tároló vagy kombinálja lehet. Az egyszerűség kedvéért azt fogja absztrakt, a feladatok ebben a példában. Az ebben a példában végrehajtható különböző hibák a következők:
  + RestartNode: A számítógép újraindítása hasonlóan Ungraceful hiba.
  + RestartDeployedCodePackage: A szolgáltatás host folyamat hasonlóan Ungraceful hibafa összeomlik.
  + RemoveReplica: Biztonságos hibafa replika eltávolítása hasonlóan.
  + MovePrimary: Replika hasonlóan biztonságos hiba lép a szolgáltatás háló terheléselosztó által indított.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```
