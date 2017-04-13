<properties
    pageTitle="Azure diagnosztika (.NET) használata a Felhőszolgáltatásaihoz |} Microsoft Azure"
    description="Azure diagnosztika segítségével Azure cloud Services hibakeresése során, mérési teljesítményét, felügyeletet igényel, forgalom elemzés és egyéb adatok összegyűjtése."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Azure diagnosztika az Azure Cloud Services engedélyezése

Azure diagnosztika háttér a [Azure diagnosztika áttekintése](../azure-diagnostics.md) című témakörben találhat.


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Egy dolgozó szerepkör diagnosztika engedélyezése

A részletes ismerteti, hogyan tudnak megvalósítani a .NET EventSource osztály használatával telemetriai adatokat bocsát Azure dolgozó szerepet. Azure diagnosztika telemetriai adatok összegyűjtése és tárolja azt a Azure tárterület-fiókkal használják. Dolgozó szerepkörbe létrehozásakor a Visual Studio automatikusan lehetővé teszi, hogy a megoldás az Azure SDK .NET 2.4 és korábbi részeként diagnosztika 1.0. Az alábbi lépéseket ismertetik a folyamat hozhat létre a dolgozó szerepkört, diagnosztika 1.0 letiltása a megoldást, és a központi telepítés diagnosztika 1.2-es vagy 1.3 a dolgozó szerepkörhöz.

### <a name="pre-requisites"></a>Előzetes követelmények
Ez a cikk tartalma feltételezi, Azure verzióra szóló előfizetése, és a Visual Studio 2013 alkalmazást az Azure SDK csomagjában talál. Ha nem rendelkezik az Azure előfizetéssel, jelentkezzen az [Ingyenes próbaverziót][]. Ügyeljen arra, hogy [Telepítse és állítsa be az Azure PowerShell verzió 0.8.7 vagy újabb][].

### <a name="step-1-create-a-worker-role"></a>Lépés: 1: Dolgozó szerepkör létrehozása
1.  Indítsa el a **Visual Studio 2013**.
2.  Új **Azure Felhőszolgáltatásában** projektet létrehozni a **felhőben** sablonból, amely ezeket a .NET-keretrendszer 4.5.  A projekt "WadExample" nevet, és kattintson az OK gombra.
3.  Jelölje ki a **Dolgozó szerepkört** , és kattintson az OK gombra. A projekt jön létre.
4.  A **Megoldás Explorer**kattintson duplán a **WorkerRole1** tulajdonságok fájlt.
5.  A **konfiguráció** bejelölését **Diagnosztika engedélyezése** , letiltása diagnosztika 1.0 (Azure SDK 2.4 és eariler) lap.
6.  A megoldás ellenőrizheti, hogy hibátlan összeállítása.

### <a name="step-2-instrument-your-code"></a>Lépés: 2: A kód eszköz
Cserélje le a WorkerRole.cs tartalmát a következő kódot. A [EventSource osztály][]öröklődik osztály SampleEventSourceWriter, négy naplózás módszerek hajtja végre: **SendEnums**, **MessageMethod**, **SetOther** és **HighFreq**. Az első paraméter a **WriteEvent** módszerrel a megfelelő Azonosítóját határozza meg. A Futtatás módszer, amely felhívja a naplózás módszer a **SampleEventSourceWriter** osztály végrehajtott minden 10 másodperc végtelen ciklust hajtja végre.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Lépés 3: Üzembe dolgozó szerepköre
1.  Visual Studio belül az Azure dolgozó szerepköre üzembe kiválasztásával a **WadExample** project Explorer megoldást, majd a **Közzététel** a **Szerkesztés** menüben.
2.  Válassza ki azt az előfizetést.
3.  A **Microsoft Azure-Közzététel beállításai** párbeszédpanelen jelölje ki az **Új létrehozása...**.
4.  A **felhőalapú szolgáltatás hozzon létre és a tárterület-fiók** párbeszédpanelen írjon be egy **nevet** (például "WadExample"), és jelöljön ki egy régió affinitás csoport.
5.  A **környezet** beállítása **átmeneti**.
6.  Az egyéb **beállításokat** tetszés szerint módosíthatja, és kattintson a **Közzététel**gombra.
7.  Telepítésének befejeződése után ellenőrizze az Azure klasszikus portálon, hogy a felhőalapú szolgáltatást **futtató** állapotban van.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Lépés: 4: A diagnosztikai konfigurációs fájl létrehozása, és a bővítmény telepítése
1.  A nyilvános konfigurációs fájl sémadefiníciója letöltése hajtja végre az alábbi PowerShell-parancsot:
2.
        (Get-AzureServiceAvailableExtension - Kiegészítő_mező_neve "PaaSDiagnostics" - ProviderNamespace "Microsoft.Azure.Diagnostics"). PublicConfigurationSchema |} Kimenő fájl – kódolás utf8 - fájl elérési útja "WadConfig.xsd"

2.  XML fájl hozzáadása a **WorkerRole1** projekthez kattintson a jobb gombbal a **WorkerRole1** projekten, és válassza a **Hozzáadás** -> **Új elem...**  ->  **Visual C# elemek** -> **adatok** -> **XML-fájlt**. Nevezze el a fájlt "WadExample.xml".

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  A WadConfig.xsd társítani a konfigurációs fájl. Győződjön meg arról, hogy a WadExample.xml editor ablak az aktív ablak. Nyomja le az **F4 billentyűt** a **Tulajdonságok** ablak megnyitásához. Kattintson a **Tulajdonságok** ablak a **sémák** tulajdonság alapján. Kattintson a **...** a **sémák** tulajdonságban. Kattintson a **Hozzáadás** gombra, és nyissa meg azt a XSD fájl mentési helyére, és jelölje ki a fájlt WadConfig.xsd. Kattintson az **OK gombra**.
4.  A következő XML cserélje ki a WadExample.xml konfigurációs fájl tartalmát, és mentse a fájlt. Ez a konfigurációs fájl határozza meg néhány teljesítmény számláló összegyűjtéséhez: egy processzort és egy memóriában kihasználtság. A konfiguráció majd a megfelelő módszerek az SampleEventSourceWriter osztály négy események határozza meg.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>5 lépés: A diagnosztikai telepítése dolgozó szerepköre
A PowerShell-parancsmagok diagnosztika projektvezetési egy webhely vagy dolgozó szerepkör a program: Set-AzureServiceDiagnosticsExtension Get-AzureServiceDiagnosticsExtension és eltávolítás-AzureServiceDiagnosticsExtension.

1.  Nyissa meg az Azure PowerShell.
2.  Hajtsa végre a parancsfájl diagnosztika telepítése dolgozó szerepköre (a tárhely fiókkulcs wadexample tároló fiókjának *StorageAccountKey* Csere erre):

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Lépés 6: Adatok telemetriai tekintse meg
A Visual Studio **Server Explorer** nyissa meg a wadexample tárterület-fiókot. Miután a felhőbe szolgáltatás futása körülbelül 5 percig, meg kell jelennie a táblák **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** és **WADSetOtherTable**. Kattintson duplán a táblára a telemetriai gyűjtött megtekintéséhez.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Konfigurációs fájl séma

A diagnosztikai konfigurációs fájl inicializálni diagnosztikai beállítások, a diagnosztikai agent indításakor használt értékek határozza meg. A [legújabb séma hivatkozás](https://msdn.microsoft.com/library/azure/mt634524.aspx) érvényes értékeket és a példákat talál.

## <a name="troubleshooting"></a>Hibaelhárítás

Ha problémát tapasztal, olvassa el a [Hibaelhárítás Azure diagnosztika](../azure-diagnostics-troubleshooting.md) közös problémákkal kapcsolatos segítségért.

## <a name="next-steps"></a>Következő lépések
[Lásd: a virtuális gép listáját kapcsolatos cikkek Azure diagnosztika](azure-diagnostics.md#cloud-services) gyűjtött, adatmódosítás kapcsolatos problémák elhárítása vagy információk diagnosztika általános.


[EventSource osztály]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Ingyenes próbaverzió]: http://azure.microsoft.com/pricing/free-trial/
[Telepítse és állítsa be az Azure PowerShell verzió 0.8.7 vagy újabb verzió]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
