<properties
    pageTitle="Nyomon követheti a folyamat az Azure diagnosztika Cloud Services alkalmazásban |} Microsoft Azure"
    description="Adja hozzá a nyomkövetési üzenetek hibakeresési mérési teljesítmény, figyelés, forgalom elemzés és további segítséget az Azure alkalmazás."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>A folyamat az Azure diagnosztika Cloud Services alkalmazás nyomon követése

Nyomkövetés módja a Lync-végrehajtása során az alkalmazás futása közben is meg. A [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)és [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) osztályok kapcsolatos hibák és az alkalmazások végrehajtását információk rögzítése a naplók, szövegfájlokból és újabb elemzéshez más eszközökről is használhatja. Nyomkövetés kapcsolatos további tudnivalókért olvassa el a [nyomkövetési és az alkalmazások leírására](https://msdn.microsoft.com/library/zs6s4h68.aspx)című témakört.


## <a name="use-trace-statements-and-trace-switches"></a>Nyomkövetés kimutatások és a nyomkövetési kapcsolók

A Cloud Services alkalmazásban a [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) hozzáadása az alkalmazás-konfiguráció és az alkalmazás kódja System.Diagnostics.Trace vagy System.Diagnostics.Debug híváskezdeményezési megvalósítása nyomon követését. Használja a konfigurációs fájl *app.config* dolgozói és a webes szerepkörök *web.config* . Visual Studio sablon használatával új szolgáltatott szolgáltatás létrehozásakor Azure diagnosztika automatikusan felkerül a projektet, és a DiagnosticMonitorTraceListener bekerül a megfelelő konfigurációs fájl hozzáadása a szerepköröket.

Információ a úgy, hogy a nyomkövetési kimutatások [hogyan: nyomkövetési kimutatások hozzáadása alkalmazás kódja](https://msdn.microsoft.com/library/zd83saa2.aspx).

A kód [Nyomkövetési kapcsolók](https://msdn.microsoft.com/library/3at424ac.aspx) bejelölésével megadhatja, hogy nyomkövetési fordul elő, és hogyan teljes körű. Ezzel a radírral figyelheti a alkalmazás munkakörnyezetben állapotát. Ez különösen fontos egy üzleti alkalmazásban, amely több számítógépen futó több összetevőt használja. További tudnivalókért lásd: [hogyan: állítsa be a nyomkövetési kapcsolók](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Állítsa be a nyomkövetési figyelő az Azure alkalmazásokban

Nyomkövetés, hibakeresési és TraceSource, csak a "hallgatók" összegyűjtése és küldött üzenetek rekord beállítása. Hallgatók összegyűjtése, tárolni és továbbítani az üzeneteket a nyomkövetés. Őket közvetlenül a nyomkövetés eredménye egy megfelelő célba, például napló, ablak vagy szövegfájlt. Azure diagnosztika [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) osztály használja.

Az alábbi eljárás elvégzése előtt inicializálni a a Azure diagnosztikai monitort. Ehhez, olvassa el a [Diagnosztika segítségével, így a Microsoft Azure-ban](cloud-services-dotnet-diagnostics.md)című témakört.

Figyelje meg, hogy a Visual Studio által biztosított sablonok használata esetén a figyelő konfigurációja bekerül automatikusan meg.


### <a name="add-a-trace-listener"></a>A nyomkövetés-figyelő hozzáadása

1. Nyissa meg a szerepkör a web.config vagy app.config fájlt.
2. A következő kód hozzáadása a fájlhoz. A hivatkozott összeállítási verziószámának használni a verzió attribútum módosítása. Az összeállítás-verzió kivéve, ha vannak olyan frissítéseket nem változik minden egyes Azure SDK kiadással feltétlenül.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Győződjön meg arról, hogy a Microsoft.WindowsAzure.Diagnostics összeállítási projekt hivatkozás. Frissítse a hivatkozott Microsoft.WindowsAzure.Diagnostics összeállítás verziójának megfelelő fenti az XML verziószámát.

3. A konfigurációs fájl mentéséhez.

Hallgatók kapcsolatos további tudnivalókért olvassa el a [Nyomkövetési hallgatók](https://msdn.microsoft.com/library/4y5y10s7.aspx)című témakört.

Miután a figyelő hozzáadásának lépéseit, utódok kimutatások hozzáadhatja a kódot.


### <a name="to-add-trace-statement-to-your-code"></a>A kód nyomkövetési utasítás hozzáadása

1. Nyissa meg a forrásfájlt, az alkalmazás. Ha például a <RoleName>dolgozó szerepkör vagy webes szerepkör .cs fájlt.
2. Adja hozzá a következő utasítással, ha már nem szerepel:
    ```
        using System.Diagnostics;
    ```
3. Adja meg, amelyre az alkalmazás állapotával kapcsolatos információk rögzítése a nyomkövetési kimutatások. A kimenet a nyomkövetési kimutatás formázásához többféle módszerrel is használhatja. További tudnivalókért lásd: [hogyan: nyomkövetési kimutatások hozzáadása alkalmazás kódja](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Mentse a forrásfájl nélkül.
