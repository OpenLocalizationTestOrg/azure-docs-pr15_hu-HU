<properties
    pageTitle="A folyamatos átvitelű az meleg mappa elérési útját esemény hubok Azure diagnosztikai adatok |} Microsoft Azure"
    description="Bemutatja, hogyan állítható be Azure diagnosztika az esemény hubok a végpont, például az útmutató a gyakori alkalmazási területek."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Adatfolyam Azure diagnosztika meleg elérési az esemény hubok használatával

Azure diagnosztika mértékek és a naplókat összegyűjtése cloud services virtuális gépeken futó (VMs) és a találatok átvitele Azure tároló rugalmas lehetőséget nyújt. Március 2016 (SDK 2.9) időkereten kezdve, diagnosztika gyűjtése teljesen egyéni adatforrásokhoz és adatátviteli meleg elérési útját a másodpercben megadott [Azure esemény hubok](https://azure.microsoft.com/services/event-hubs/)használatával.

Támogatott adattípusok a következők:

- Esemény-nyomkövetés a Windows (esemény-nyomkövetés) események
- Teljesítményét számláló
- Windows-eseménynaplók
- Alkalmazás naplók
- Azure diagnosztika infrastruktúra naplók

Ez a cikk bemutatja, hogyan állítható be Azure diagnosztika az esemény hubok végpontok közötti. Útmutató az a következő gyakori alkalmazási területek is elérhetők:

- Hogyan szabhatja testre a naplók és mértékek, amely az esemény hubok sinked beszerzése
- Minden környezetben beállítások módosítása
- Esemény hubok adatfolyam adatok megtekintése
- A kapcsolat hibáinak elhárítása  

## <a name="prerequisites"></a>Előfeltételek

Elhelyezés az Azure diagnosztika esemény hubok támogatja a Felhőszolgáltatások, VMs, virtuális gép skála beállítása és szolgáltatás háló kezdve az Azure SDK 2.9 és a megfelelő Azure eszközök for Visual Studio.

- Azure diagnosztika bővítmény 1,6 ([Azure SDK .NET 2.9 vagy újabb](https://azure.microsoft.com/downloads/) célként ez alapértelmezés szerint)
- [Visual Studio 2013-at vagy újabb verzió](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Meglévő konfigurációk az Azure diagnosztika a az alkalmazásokban *.wadcfgx* fájl és az alábbi lehetőségek közül:
    - Visual Studio: [Azure Felhőszolgáltatások és a virtuális gépeken futó diagnosztika konfigurálása](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - A Windows PowerShell: [diagnosztika a PowerShell használatá Azure Cloud Services engedélyezése](../cloud-services/cloud-services-diagnostics-powershell.md)
- A cikk [első lépések az esemény hubok](./event-hubs-csharp-ephcs-getstarted.md) per kiépítéstől esemény hubok névtér

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Esemény hubok gyűjtő Azure diagnosztika csatlakoztatása

Azure diagnosztika Mosogatók naplókról és a mértékek, mindig Azure tárterület-fiókhoz alapértelmezés szerint. Az alkalmazások továbbá előfordulhat, hogy gyűjtése az esemény hubok *.wadcfgx* fájl **PublicConfig** szakaszához **WadCfg** elemének új **mosdók** szakasz hozzáadásával. A Visual Studióban, a *.wadcfgx* fájlt tárolja az alábbi elérési út: **Felhőalapú szolgáltatás Project** > **szerepkörök** >  **(RoleName)** > **diagnostics.wadcfgx** fájlt.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

Ebben a példában az esemény központi URL-címe értékűre az esemény-központban teljesen minősített névtere: esemény hubok névtér + "/" + esemény központi nevét.  

Az esemény központi URL-címe megjelenik az [Azure portál](http://go.microsoft.com/fwlink/?LinkID=213885) az esemény hubok irányítópulton.  

A **gyűjtése** neve bármely érvényes karakterlánc mindaddig, amíg az azonos értékkel egységes használják a konfigurációs fájl beállíthatók.

> [AZURE.NOTE]  További felül, például *applicationInsights* ebben a részben konfigurált lehet. Azure diagnosztika lehetővé teszi, hogy egy vagy több mosdók határozni, ha minden gyűjtő deklarálva **PrivateConfig** szakaszában is van.  

Az esemény hubok gyűjtő is kell deklarálva és a *.wadcfgx* konfigurációs fájl **PrivateConfig** szakaszban meghatározott.

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

A `SharedAccessKeyName` értéket kell lennie egy megosztott Access-aláírás (Társítások) billentyű és a házirendet, amely az **Esemény hubok** névtér definiálva van. Tallózással keresse meg az esemény hubok irányítópultot az [Azure portál](https://manage.windowsazure.com), kattintson a **beállítás** fülre, és állítson be egy *küldése* engedélyekkel rendelkező elnevezett házirendet (például "SendRule"). A **StorageAccount** is van deklarálva **PrivateConfig**. Nincs szükség az módosításához az alábbi értékeket, ha dolgoznak. Ebben a példában azt az értékek üresen hagyja, amely olyan, hogy a későbbi eszköz elrendezéstípus az értékek meg. Például a *ServiceConfiguration.Cloud.cscfg* környezet konfigurációs fájl állítja be a környezet megfelelő neveket és billentyűt.  

> [AZURE.WARNING] Az esemény hubok Társítások kulcs egyszerű szöveg a *.wadcfgx* fájlban vannak tárolva. A kulcs gyakran verziókövetési szolgáltatása beadás, vagy pedig az összeállítás Server tárgyi eszköz érhető el, így tetszés szerint kell védeni. Azt javasoljuk, hogy használható itt Társítások kulcs *csak küldése* engedélyekkel, hogy rosszindulatú felhasználók írhat az esemény-hubon keresztül csatlakozott, de nem hallgatni vagy kezelheti.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Azure diagnosztikai naplók és -mértékek az esemény hubok gyűjtése beállítása

A korábban tárgyalt, az összes alapértelmezett és egyéni diagnosztikai adatok, ez azt jelenti, hogy mértékek és a naplókat, automatikusan sinked Azure tárolóhoz a beállított időközöket jelölik. Az esemény hubok és bármilyen további gyűjtő legfelső szintű vagy leveles csomópont megadhatja a hierarchiában a esemény hubhoz sinked kell. Ide tartoznak, esemény-nyomkövetés események, a teljesítmény számláló, a Windows-eseménynaplók és az alkalmazás naplók.   

Vegye figyelembe, hogy hány adatpontot ténylegesen átviszi esemény hubok. Általában fejlesztők adatátviteli alacsony-késés gyorsbillentyűk használatát elérési utat kell elfogyasztott és értelmezhető gyorsan. Lync-riasztások vagy Automatikus méretezéssel szabályok rendszerek példák. A Fejlesztőeszközök a előfordulhat, hogy is beállítása egy másik analysis áruházból, vagy keresse áruház – például Azure Értékáram-elemzés, Elasticsearch, egyéni ellenőrző rendszert vagy a többi felhasználó által a kedvenc felügyeleti rendszert.

Az alábbiakban néhány példát konfigurációk.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

A következő példában a szülő **PerformanceCounters** csomópontot a hierarchia, ami azt jelenti, az összes alárendelt **PerformanceCounters** esemény hubok küld a gyűjtő érvényes.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

Az előző példában, a program alkalmazza a gyűjtő csak három számláló: **Kérései** **Kérések elutasítva**és **kihasználtsága (%)**.  

A következő példa bemutatja, hogyan a fejlesztők korlátozhatja a kritikus mérési módja miatt a Szolgáltatásállapot használt küldött adatok mennyiségét.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

Ebben a példában a gyűjtő naplók alkalmazott, és csak a hiba szintű nyomkövetési szűrt.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Üzembe helyezéséhez és a Cloud Services alkalmazás és a diagnosztikai beállítások módosítása

Visual Studio legegyszerűbb elérési útját telepíteni az alkalmazást és az esemény hubok gyűjtő beállításokat tartalmaz. Megtekintheti és szerkesztheti a fájlt, a *.wadcfgx* fájl megnyitása a Visual Studio, szerkesztheti és mentheti azt. Az elérési út **Felhőalapú szolgáltatás Project** > **szerepkörök** > **(RoleName)** > **diagnostics.wadcfgx**.  

Ezen a ponton frissítése az összes telepítés és üzembe művelet Visual Studio, Visual Studio csoport a rendszer és az összes parancsok vagy parancsprogramok alapján MSBuild és használata a **/t: közzététele** cél a *.wadcfgx* szerepeltetni a csomagolást. Ezeken kívül telepítések és frissítések üzembe a fájl Azure a VMs a megfelelő Azure diagnosztika ügynök bővítmény használatával.

Az alkalmazás és a diagnosztikai Azure konfigurációja központi telepítése után tevékenység az esemény-központban, az irányítópult azonnal látni fogja. Ez azt jelzi, hogy készen áll a meleg elérési út adatainak megtekintése a figyelő ügyfél és elemzési eszközben lehetőség helyezheti.  

Az alábbi ábrán az esemény hubok irányítópult látható megfelelő küld a diagnosztikai adatok az esemény-központban valamikor 11 du után kezdődik. Ha ez az alkalmazás telepítették frissített *.wadcfgx* -fájllal, és a gyűjtő megfelelően történt-e beállítva.

![][0]  

> [AZURE.NOTE] Ha frissítéseket a Azure diagnosztika konfigurációs fájl (.wadcfgx), azt ajánljuk, hogy Ön leküldéses a frissítéseket a teljes alkalmazás, valamint a konfigurációs Visual Studio közzétételi vagy a Windows PowerShell-parancsprogramot.  

## <a name="view-hot-path-data"></a>Gyorsbillentyűk használatát elérési út adatainak megtekintése

A korábban tárgyalt, vannak figyelését és esemény hubok adatfeldolgozás sokszor használatát.

Egy egyszerű megközelítés, hozhat létre egy kis próba New hallgathatja meg az esemény-központban, és a kimeneti adatfolyam nyomtatása. Elhelyezheti a következő kódot, és az [első lépések az esemény hubok](./event-hubs-csharp-ephcs-getstarted.md)részletesebben magyarázza), egy konzol alkalmazásra.  

Figyelje meg, hogy a New kell tartalmaznia az [esemény processzor Host Nuget csomagot](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Ne feledje, hogy a **fő** függvény csúcsos zárójelek értékeket cserélje ki az erőforrásokhoz tartozó értékeket.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Esemény hubok gyűjtő – problémamegoldás

- Az esemény-központban nem jelennek meg a várt módon bejövő és kimenő esemény tevékenység.

    Jelölje be az esemény központi kiépítése sikeres. Minden kapcsolat információ *.wadcfgx* **PrivateConfig** szakaszában egyeznie kell az értékeket az erőforrás a portálon látható módon. Győződjön meg arról, hogy definiált Társítások házirend (a példában a "SendRule" jelöli) a portálon és, hogy *küldése* engedélyt.  

- A frissítés után az esemény-központban már nem bejövő és kimenő esemény tevékenység jeleníti meg.

    Mindenekelőtt győződjön meg róla, hogy az esemény központi és konfigurációs információk megfelelő korábban című cikkben ismertetett módon. Előfordul, hogy a **PrivateConfig** telepítési frissítés alaphelyzetbe állítása. A javasolt fix *.wadcfgx* a projekt összes módosítsa vagy egy teljes alkalmazásfrissítést majd leküldéses is. Ha ez nem lehetséges, győződjön meg arról, hogy a diagnosztika frissítés verembe küldi a teljes **PrivateConfig** , amely tartalmazza a Társítások billentyűt.  

- A javaslatok megpróbáltam, és az esemény-központban továbbra sem működik.

    Nézzük meg az Azure tároló táblázat naplókról és a hibákat tartalmazó az Azure diagnosztika magát: **WADDiagnosticInfrastructureLogsTable**. Egy, hogy például az [Azure tároló Explorer](http://www.storageexplorer.com) eszköz használata tároló ehhez a fiókhoz csatlakozhat, tekintse meg a táblát, és időbélyeg-lekérdezés hozzáadása az elmúlt 24 óra. Az eszköz használatával egy .csv fájl exportálására, és nyissa meg az alkalmazást, például a Microsoft Excel az. Az Excel egyszerűen kereshet hívófél-kártya karakterláncok, például **EventHubs**, hogy milyen jelenik meg hibaüzenet.  

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a esemény hubok](https://azure.microsoft.com/services/event-hubs/) •

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>. Melléklet: Azure diagnosztika konfigurációs fájl (.wadcfgx) Példa befejezése

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Ebben a példában a kiegészítő *ServiceConfiguration.Cloud.cscfg* néz ki a következő.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
