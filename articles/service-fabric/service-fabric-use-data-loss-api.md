<properties
   pageTitle="Az adatok elvesztését a szolgáltatás háló szolgáltatások meghívásához hogyan |} Microsoft Azure"
   description="Megtudhatja, hogy miként használhatja az adatok elvesztését api"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Hogyan indítsa az adatok elvesztését a szolgáltatások

>[AZURE.WARNING] A dokumentum ismertetik, hogyan lehet a szolgáltatások adatvesztést, és körültekintéssel használjon.

## <a name="introduction"></a>– Bevezetés
Az adatok elvesztését a szolgáltatás háló szolgáltatás által hívó StartPartitionDataLossAsync() partíciót a is hivatkozhat.  Ez az api van és Analysis Services szolgáltatásban használja az adatok elvesztését feltételek okozhat vonhatnak.

## <a name="using-the-fault-injection-and-analysis-service"></a>A van, és az Analysis Services szolgáltatásban

A van, és az Analysis Services szolgáltatásban jelenleg a következő API-k támogatja a diagramon az alábbi.  A diagram jobb oldalán a megfelelő PowerShell-parancsmag jeleníti meg.  Minden egyes API mindegyik további információt találhat az msdn dokumentációjában találhat.

|           C# API                    |         PowerShell-parancsmag                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Kezdés-ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Kezdés-ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Kezdés-ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>A parancs futtatása elvi áttekintése

A van, és az Analysis Services szolgáltatásban hol, kezdje el a parancsot egy API-val, a "Start" API a dokumentum a továbbiakban, majd ellenőrzi, egy "GetProgress" API használatával, amíg el nem éri a Terminálszolgáltatások állapot, vagy szakítsa meg ez a parancs végrehajtásának aszinkron modellt használ.
Parancs indításához hívja fel a megfelelő API a "Start" API.  Ez az API adja eredményül, ha az van, és az Analysis Services szolgáltatásban elfogadta a kérést.  Azonban azt nem azt jelzi, hogy milyen távol parancs futtatása, vagy, ha rendelkezik indult el.  Annak ellenőrzéséhez, parancs végrehajtásának, hívja fel a "GetProgress" API-val, amely megfelel a korábban ezek neve "Start" API.  A "GetProgress" API a parancsot a State tulajdonság belül aktuális állapotát jelző objektum ad eredményül.  Parancs határozatlan ideig, amíg fut:

1.  Befejeződik.  Ha "GetProgress" Ebben az esetben a hívás, a haladás objektum állapotának befejeződik.
2.  A kritikus hibák találkozik.  Ha "GetProgress" Ebben az esetben a hívás, a haladás objektum állapotának hibás lesz
3.  A [CancelTestCommandAsync] keresztül megszüntetése [ cancel] API-val, vagy a [Leállítás-ServiceFabricTestCommand]  [ cancelps] PowerShell-parancsmag.  Hívná "GetProgress" rajta ebben az esetben, ha a haladás objektum állapotának lesz lemondva vagy ForceCancelled, attól függően, hogy egy adott API-nak argumentum.  [CancelTestCommandAsync] dokumentációjában [ cancel] további információt.


## <a name="details-of-running-a-command"></a>A parancs futtatása részletei

Parancs indításához hívja fel a várt argumentumokkal indítása API.  Az összes indítása API-khoz egy globálisan egyedi azonosítója argumentum operationId nevű van.  Meg kell nyomon követheti a az operationId argumentum, mivel ez a parancs végrehajtásának nyomon követése szolgál.  Meg kell átadandó "GetProgress" API be a parancs végrehajtásának nyomon abból.  A operationId egyedinek kell lennie.

Miután sikeresen elérje a Start API-t, a GetProgress API kell hívni ismétlődve a visszaadott előrehaladását objektum State tulajdonság befejeztéig.  Az összes [FabricTransientException's]  [ fte] és OperationCanceledException's meg kell ismételni.
A parancs (kész, Faulted vagy lemondva) Terminálszolgáltatások állapot elérésekor a visszaadott előrehaladását objektum eredmény tulajdonság lesz további információt.  Ha az állapot befejeződik, Result.SelectedPartition.PartitionId fogják tartalmazni a kiválasztott partíciót azonosítója.  Result.Exception üres lesz.  Hibás állapotát, ha Result.Exception lesz az oka az hibafa utasítások beszúrását és Analysis Services szolgáltatásban hibával a parancsot.  Result.SelectedPartition.PartitionId kiválasztott partíciót azonosítója lesz.  Bizonyos esetekben a parancs előfordulhat, hogy nem rendelkezik kezdett elegendő partíciót válassza.  Ebben az esetben a PartitionId 0 lesz.  Ha az állapot megszakad, Result.Exception üres lesz.  A Faulted lehetőséget választja, például Result.SelectedPartition.PartitionId a kiválasztott partíciót azonosítója lesz, de ha a parancs nem járt elegendő kéri, 0 lesz.  Is találhat az alábbi minta.

Az alábbi példakódot bemutatja, hogyan kezdje el a, majd jelölje be a parancsot egy adott partíciót adatvesztést előrehaladását.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Az alábbi példa szemlélteti, hogyan lehet a PartitionSelector válasszon egy véletlen partíciót egy adott szolgáltatás:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Előzmények és csonkítva.

Parancs kapott Terminálszolgáltatások állapot, miután a metaadatait marad a van, és az Analysis Services szolgáltatásban adott időpontra, mielőtt törlődnek, így helyet takaríthat.  "GetProgress" nevezik a operationId parancs használatával, akkor eltávolítása után, ha egy FabricException egy KeyNotFound hibakód az ad vissza.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
