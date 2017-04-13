<properties
    pageTitle="Azure diagnosztika használatáról virtuális gépeken futó |} Microsoft Azure"
    description="Azure diagnosztika segítségével az Azure virtuális gépeken futó adatok gyűjtése hibakeresése során, a teljesítményt, figyelése, forgalom elemzés és az egyéb mérési."
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Azure virtuális gépeken futó diagnosztika engedélyezése

Azure diagnosztika háttér a [Azure diagnosztika áttekintése](azure-diagnostics.md) című témakörben találhat.

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Virtuális gépen diagnosztika engedélyezése

A részletes távolról telepítése fejlesztési számítógépről egy Azure virtuális géphez diagnosztika ismerteti. Azt is megtudhatja, hogy miként olyan alkalmazás, amely Azure virtuális gépen fut, és a .NET [EventSource osztály][]használatával telemetriai adatokat bocsát végrehajtásához. A telemetriai gyűjtötte és tárolta azt Azure tároló fiók Azure diagnosztika segítségével.

### <a name="pre-requisites"></a>Előzetes követelmények
A részletes feltételezi Azure verzióra szóló előfizetése, és a Visual Studio 2013 alkalmazást az Azure SDK csomagjában talál. Ha nem rendelkezik az Azure előfizetéssel, jelentkezzen az [Ingyenes próbaverziót][]. Ügyeljen arra, hogy [Telepítse és állítsa be az Azure PowerShell verzió 0.8.7 vagy újabb][].

### <a name="step-1-create-a-virtual-machine"></a>Lépés: 1: Virtuális gép létrehozása
1.  A fejlesztői számítógépen nyissa meg a Visual Studio 2013.
2.  A Visual Studio **Server Explorer** bontsa ki az **Azure**, kattintson a jobb gombbal a **virtuális gépeken futó** lehetőséget, majd jelölje ki a **virtuális gép létrehozása**.
3.  Az **előfizetés kiválasztása** párbeszédpanelen jelölje ki a Azure-előfizetése, és kattintson a **Tovább**gombra.
4.  Jelölje ki a **Windows Server 2012 R2 adatközponthoz November 2014-es** párbeszédpanelen **Jelöljön ki egy virtuális gép képet** , és kattintson a **Tovább**gombra.
5.  A **Virtuális gép alapvető beállításai**meg a "wadexample" virtuális gép nevét. A rendszergazdai felhasználónév és jelszó beállítása, és kattintson a **Tovább**gombra.
6.  A **Felhőalapú szolgáltatás beállításai** párbeszédpanel "wadexampleVM" nevű új felhőalapú szolgáltatás hozzon létre. Hozzon létre egy új "wadexample" nevű tárterület-fiókot, és kattintson a **Tovább**gombra.
7.  Kattintson a **létrehozása**gombra.

### <a name="step-2-create-your-application"></a>Lépés: 2: Az alkalmazás létrehozása
1.  A fejlesztői számítógépen nyissa meg a Visual Studio 2013.
2.  Új képi C# Console-alkalmazás létrehozása .NET-keretrendszer 4.5 függvénynél. A projekt "WadExampleVM" nevet.
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  Cserélje le a Program.cs tartalmát a következő kódot. Az osztály **SampleEventSourceWriter** hajtja végre négy naplózás módszer: **SendEnums**, **MessageMethod**, **SetOther** és **HighFreq**. Az első paraméter a WriteEvent módszerrel a megfelelő Azonosítóját határozza meg. A Futtatás módszer, amely felhívja a naplózás módszer a **SampleEventSourceWriter** osztály végrehajtott minden 10 másodperc végtelen ciklust hajtja végre.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
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

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

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
        }
        }


4.  Mentse a fájlt, és jelölje ki a **Megoldást összeállítása** a **Szerkesztés** menüben össze a kód.


### <a name="step-3-deploy-your-application"></a>3 lépés: Az alkalmazás telepítése
1.  Kattintson a jobb gombbal a **Megoldást Intézőben** **WadExampleVM** projekten, és válassza a **Mappa megnyitása a Fájlkezelővel**.
2.  Keresse meg a *bin\Debug* mappát, és másolja a fájlokat (WadExampleVM.*)
3.  A **Kiszolgáló Intézőben** kattintson a jobb gombbal a virtuális gépen, és válassza a **Csatlakozás távoli asztali változatában**.
4.  Miután létrejött a virtuális gép WadExampleVM és beillesztés az alkalmazás mappába a fájlok nevű mappa létrehozása
5.  Indítsa el az alkalmazás WadExampleVM.exe. Meg kell jelennie egy üres konzol ablakot.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Lépés: 4: A diagnosztikai konfigurációja létrehozása, és a bővítmény telepítése
1.  Töltse le a nyilvános konfigurációs fájl sémadefiníciója a fejlesztés számítógépére hajtja végre az alábbi PowerShell-parancsot:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Nyissa meg az új XML-fájl a Visual Studióban, akár a projekt már megnyitott vagy a Visual Studio példány nem a megnyitott projektek. A Visual Studióban, válassza a **Hozzáadás** -> **Új elem...**  ->  **Visual C# elemek** -> **adatok** -> **XML-fájlt**. Nevezze el a fájlt "WadExample.xml"
3.  A WadConfig.xsd társítani a konfigurációs fájl. Győződjön meg arról, hogy a WadExample.xml editor ablak az aktív ablak. Nyomja le az **F4 billentyűt** a **Tulajdonságok** ablak megnyitásához. Kattintson a **Tulajdonságok** ablak a **sémák** tulajdonság alapján. Kattintson a **...** a **sémák** tulajdonságban. Kattintson a **Hozzáadás** gombra, és nyissa meg azt a XSD fájl mentési helyére, és jelölje ki a fájlt WadConfig.xsd. Kattintson az **OK gombra**.
4.  A következő XML cserélje ki a WadExample.xml konfigurációs fájl tartalmát, és mentse a fájlt. Ez a konfigurációs fájl határozza meg néhány teljesítmény számláló gyűjthetők össze az: egy processzort és egy memóriahasználat kihasználtság. A konfiguráció majd a megfelelő módszerek az SampleEventSourceWriter osztály négy események határozza meg.

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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Lépés az 5: Távolról telepítése diagnosztika az Azure virtuális gép
A PowerShell-parancsmagok egy virtuális a diagnosztika kezelésére szolgáló vannak: Set-AzureVMDiagnosticsExtension Get-AzureVMDiagnosticsExtension és eltávolítás-AzureVMDiagnosticsExtension.

1.  A Fejlesztőeszközök számítógépen nyissa meg az Azure Powershellt.
2.  Hajtsa végre a parancsfájl diagnosztika távolról telepítése a virtuális (csere *StorageAccountKey* wadexamplevm tároló fiókjának tároló fiók kulccsal):

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Lépés 6: Adatok telemetriai tekintse meg
A Visual Studio **Server Explorer** nyissa meg a wadexample tárterület-fiókot. A virtuális futó körülbelül 5 perc után meg kell jelennie a táblák **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** és **WADSetOtherTable**. Kattintson duplán a táblára a telemetriai gyűjtött megtekintéséhez.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Konfigurációs fájl séma

A diagnosztikai konfigurációs fájl inicializálni diagnosztikai beállítások, a diagnosztikai agent indításakor használt értékek határozza meg. A [legújabb séma hivatkozás](https://msdn.microsoft.com/library/azure/mt634524.aspx) érvényes értékeket és a példákat talál.

## <a name="troubleshooting"></a>Hibaelhárítás

[Azure diagnosztika hibaelhárítási](azure-diagnostics-troubleshooting.md) talál további információt.


## <a name="next-steps"></a>Következő lépések
[Lásd: a virtuális gép listáját kapcsolatos cikkek Azure diagnosztika](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) gyűjtött, adatmódosítás kapcsolatos problémák elhárítása vagy információk diagnosztika általános.


[EventSource osztály]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Ingyenes próbaverzió]: http://azure.microsoft.com/pricing/free-trial/
[Telepítse és állítsa be az Azure PowerShell verzió 0.8.7 vagy újabb verzió]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
